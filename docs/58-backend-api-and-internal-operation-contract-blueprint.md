# HARAI — Backend API and Internal Operation Contract Blueprint

**Status:** Draft contract-planning proposal; not approved for implementation.
**Purpose:** Bridge frontend data/action needs (DOC-55), conceptual domains (DOC-56) and backend workflows (DOC-57) without pretending final routes or schemas have been approved.
**Gate:** G1 requirements approval remains NOT PASSED. G2 architecture approval remains NOT APPROVED.

## 1. Scope and contract principles

This blueprint describes API operation families, preconditions, authorization, idempotency, response semantics, side effects and recovery expectations. It is not a final OpenAPI document and does not define production endpoints, HTTP verbs, exact JSON schemas, headers, status codes, error codes, token formats or database tables.

Every operation contract must be explicit about: actor and tenant scope; capability/merchant-state preconditions; input validation; idempotency and concurrency; durable side effects; whether work is synchronous or asynchronous; canonical operation state; expected errors; audit/event consequences; and recovery path.

Keep the public merchant API, browser-facing dashboard API and internal worker/provider-adapter interfaces separate. They may share domain services, but must not inherit each other's trust assumptions. The browser is not a trusted source for tenant identity, financial state, approval status or provider credentials.

## 2. Contract layers

| Layer | Intended callers | Responsibilities | Trust boundary |
|---|---|---|---|
| Merchant API | Merchant backend systems | Create/query supported operations, links and refunds; retrieve authorized events/reports | API credential, scope, merchant/project and capability validation on every request |
| Dashboard API | Authenticated merchant and HARAI staff users | Screen data, settings, operations, cases and authorized actions | User identity, role, tenant scope, step-up/approval rules and support-access audit |
| Hosted checkout | Customer browser | Display approved payment details and collect customer action | Opaque session reference, expiry, abuse limits; never trust browser redirects as financial evidence |
| Provider adapter interface | Internal orchestration only | Translate normalized operations into provider-specific requests and evidence | Server-side credentials, explicit provider capability configuration and adapter policy |
| Callback ingress | Approved provider systems | Authenticate provider message, preserve evidence and enqueue idempotent processing | Provider-specific signature/authentication, replay defense, correlation and ingress restrictions |
| Internal worker/event interface | Trusted service/worker identities | Submit approved work, process events, retry delivery and reconcile | Least privilege, durable task identity, tenant-safe context and concurrency/fencing |

## 3. Common contract requirements

### 3.1 Authentication, scope and tenancy

- Authenticate every request using the approved mechanism for that caller type.
- Derive caller identity from trusted authentication, not from a user-supplied role or merchant ID.
- Resolve the merchant and project context and check it against credential scopes, membership, status and capability configuration.
- Re-check tenant ownership for referenced operations, links, beneficiaries, events, reports and files; an unguessable identifier is not authorization.
- Internal staff support access must be explicitly scoped, justified and audited. Sensitive actions require the approved step-up and separation-of-duties controls.
- Fail closed when identity, tenant context, provider environment or authorization policy cannot be resolved safely.

### 3.2 Validation and canonical values

- Validate required fields, format, supported currency/method, amount limits, link/session lifecycle, reference lengths and allowed metadata before creating a financial operation.
- Money must use the eventual approved exact representation and currency-specific rules. Do not accept ambiguous decimal/rounding conventions.
- Reject unknown or unsupported capability requests instead of silently downgrading to a different provider, method or currency.
- Distinguish client/merchant references from HARAI operation IDs and provider references. Never let a caller choose a provider reference that impersonates a persisted attempt.
- Define a canonical request fingerprint for idempotency; decide whether metadata and optional fields participate and how normalization works.

### 3.3 Idempotency and concurrency

Any operation that creates or may create a financial side effect needs an approved idempotency contract. It must define key scope, uniqueness, retention, fingerprint matching, concurrent requests, response replay and conflict behavior.

- Same key and same effective request: return or reference the original operation according to the final contract; do not create a second financial submission.
- Same key with a materially different request: reject as an idempotency conflict and do not mutate the original operation.
- Concurrent requests: atomically reserve the key and create one effective operation.
- A retry after a timeout must return the known operation or an explicit pending/unknown result; it must not assume the first provider submission failed.
- Reads and status enquiries should be safe to repeat within rate and provider limits.

Exact key syntax, storage duration, response replay and scope remain open decisions.

### 3.4 Response and state semantics

Responses must distinguish request validation, accepted durable work, further customer action, pending provider outcome, unknown outcome and definitive terminal outcomes. The final enum names and wire format must align with DOC-41 and provider-specific lifecycle evidence.

- An HTTP success/accepted response means only what the approved API contract says; it is not proof that money moved.
- Provider timeout or connection loss after submission must preserve an ambiguous/pending outcome until safely resolved.
- A browser return URL or customer screenshot cannot make a transaction successful.
- Expose only authorized, necessary fields; never expose provider credentials, internal risk signals or another tenant's metadata.
- Return stable machine-readable error categories in the final contract, with safe human-readable messages and a correlation ID. Do not expose stack traces, secrets or sensitive provider response bodies.

## 4. Operation family matrix

| Operation family | Caller and key preconditions | Idempotency/concurrency | Expected outcome semantics | Required audit/event/recovery behavior |
|---|---|---|---|---|
| Create collection/payment intent | Merchant API or approved checkout flow; merchant active, method/currency/capability enabled, amount/reference valid | Required; one effective operation per key/fingerprint | Created/accepted, requires customer action, pending or explicit validation failure | Persist canonical operation and provider attempt/work reference; status enquiry and callback/reconciliation recovery |
| Retrieve collection/payment status | Authorized merchant or dashboard user; operation belongs to tenant or staff scope | Safe read; rate-limited | Current canonical state and allowed details; may still be pending/unknown | Correlation and access audit where required; no financial side effect |
| Create payment link/checkout session | Authorized merchant; link policy, amount, expiry, usage and method validated | Required for creation; enforce single-use/reuse policy atomically | Link/session reference and lifecycle state; no payment-success implication | Audit creation and changes; enforce expiry, cancellation, concurrent use and checkout recovery |
| Cancel/expire link or session | Authorized merchant or expiry worker; current lifecycle permits action | Idempotent lifecycle transition | Cancelled/expired or already-terminal result | Audit actor/reason; cannot cancel an already completed financial operation by changing link state |
| Create refund/reversal request | Authorized role; original operation and provider support verified; refundable bound available | Required; reserve amount atomically against concurrent requests | Accepted/pending/unknown/terminal outcome per provider evidence | Link to original; independent approval where required; immutable journal effects and recovery |
| Create payout/money-out request | Authorized role, beneficiary verified, funding/limits/capability satisfied, approval policy met | Required; duplicate submission protection and operation fingerprint | Accepted/pending/unknown/terminal outcome; no inferred success from request acceptance | Audit approvals, beneficiary/policy versions and provider references; enquiry/reconciliation recovery |
| Create/update beneficiary | Authorized role and step-up/risk policy; destination format/provider support validated | Idempotent update semantics; concurrency/version control | Pending verification, active, restricted or rejected according to approved policy | Audit old/new safe summary, actor, approvals and notification; never log full secrets unnecessarily |
| Create/read API credential | Owner/admin permission; scope and environment explicit | Credential creation must avoid accidental duplicates; rotation/revocation lifecycle defined | Secret reveal only according to approved one-time display design; later retrieval must not reveal stored secret | Audit create/rotate/revoke and use; never log or expose credential values |
| Configure webhook endpoint | Authorized admin; URL/host policy and destination checks approved | Versioned update and concurrency control | Configured, disabled, verification-pending or rejected | Protect against SSRF; keep secret rotation controlled; audit delivery configuration |
| Retrieve webhook events/deliveries | Authorized tenant and event/endpoint scope | Read-only | Event lifecycle distinct from payment lifecycle | Safe payload redaction; show attempts and failure categories without cross-tenant leakage |
| Merchant dashboard summary/report | Authenticated user with role and tenant scope; filters bounded | Read-only; exports have separate limits | Derived view with freshness/consistency label where relevant | Audit sensitive exports; apply data minimization, pagination and report access controls |
| Reconciliation import/review/resolution | Internal finance/reconciliation role and source evidence | Deduplicate imported rows; optimistic concurrency on case resolution | Matched, discrepancy-open, awaiting-evidence, approved adjustment or resolved with evidence | Immutable source reference, reviewer decision, accounting effects and full audit trail |
| Internal status enquiry/recovery | Worker or authorized operator; operation and provider attempt known | Idempotent read/recovery task; no blind financial resubmission | Evidence appended and canonical state changed only through valid transition | Retry budgets, provider rate limits, stale-worker fencing, audit and alert on unresolved age |

## 5. Internal operation envelope

Every internal work item should conceptually carry:

- Stable task/work ID and task type/version.
- Internal operation, provider attempt, callback, event or reconciliation-case reference as applicable.
- Tenant/project scope where applicable, resolved from trusted persisted context rather than a caller-supplied free-form value.
- Correlation and causation identifiers linking the initiating request to downstream effects.
- Creation time, policy/configuration version, attempt count, lease/claim metadata and next eligible run time where relevant.
- A bounded retry class and safe terminal/quarantine disposition.
- Minimal necessary payload; sensitive data should be referenced through controlled storage rather than copied through queues.

Internal envelopes are not public API contracts. Exact serialization, queue product, storage model, schema evolution, signing and task lease mechanics remain architecture decisions.

## 6. Canonical operation versus provider attempt

Maintain a conceptual distinction between the merchant-facing operation and each provider interaction. One canonical collection/refund/payout operation may have one or more provider attempts or status checks, but each new financial submission must be justified by documented provider semantics and safe retry evidence.

The operation should answer: what the merchant asked HARAI to do, the authorized amount/currency/method, canonical lifecycle, idempotency identity and customer-facing references. A provider attempt should answer: which authorized provider/account/environment was used, what request/reference was sent, what was observed, and how uncertainty was resolved.

Do not expose internal provider credentials or unnecessary raw provider payloads to merchants. Preserve safe evidence needed for dispute and reconciliation with access/retention controls.

## 7. Error taxonomy planning

The final contract should map errors into stable categories with documented retryability and remediation. Names and wire codes are not fixed by this document.

| Category | Examples | Retry guidance |
|---|---|---|
| Authentication failure | Missing, invalid, expired or revoked credential/session | Do not retry unchanged credentials; correct authentication |
| Authorization/tenant denial | Scope missing, wrong tenant, role or capability denied | Do not retry until permission/configuration changes; avoid leaking resource existence |
| Validation failure | Invalid amount/currency/reference, malformed request, unsupported option | Correct request; no financial submission should have occurred |
| Idempotency conflict | Same key with a different request fingerprint | Use a new key only for a genuinely new intended operation; do not mutate original |
| Rate/limit restriction | Caller quota, provider throttle, amount/velocity limit | Respect retry guidance/backoff; financial submission status must remain accurate |
| Business-state conflict | Expired link, terminal operation, refund bound exceeded, beneficiary restricted | Refresh state or follow approved remediation; do not bypass invariants |
| Provider rejection before acceptance | Provider explicitly rejects request | Follow provider-specific mapping; confirm whether safe resubmission is permitted |
| Provider outcome pending/unknown | Timeout, connection loss, non-final provider response | Do not blindly resubmit; enquire/reconcile against original attempt |
| Internal temporary failure | Queue unavailable, database contention, downstream dependency outage | Retry only durable/idempotent work with bounded policy and monitoring |
| Security verification failure | Invalid callback signature, replay, environment mismatch | Reject/quarantine according to provider contract; audit safely and alert as appropriate |
| Configuration/eligibility failure | Provider capability not enabled, missing authorized configuration | Fail closed; fix approved configuration and re-evaluate before retry |
| Reconciliation discrepancy | Missing, duplicate, amount/currency/fee/settlement mismatch | Open owned case; require evidence and approval for correction |

Public errors must not reveal secrets, stack traces, internal topology, other-tenant existence or sensitive risk decisions. Provider error bodies should be mapped through a reviewed adapter policy and safely redacted.

## 8. API surface planning inventory

This inventory is conceptual only. It intentionally omits final paths and HTTP verbs.

### Merchant/developer surface

- Create and retrieve collection/payment operations.
- Retrieve permitted operation history and transaction details.
- Create, retrieve, expire or cancel payment links and checkout sessions within policy.
- Request and retrieve refund/reversal operations where authorized and provider-supported.
- Request and retrieve money-out operations only after all relevant gates are passed.
- Manage approved project settings, API credentials, webhook endpoints and event delivery visibility.
- Retrieve permitted reports and reconciliation status when included in the approved merchant role model.

### Dashboard surface

- Merchant/project overview, activity, transaction details and status history.
- Merchant profile, verification/activation status and capability configuration.
- Team membership, role assignments and permission-sensitive settings.
- API key lifecycle and IP allowlist configuration, with safe recovery controls.
- Webhook endpoints, event logs and delivery attempts.
- Payment links/checkout configuration and lifecycle.
- Refund/payout requests, approval queues, beneficiary management and audit trail.
- Reconciliation cases, reports, disputes and support cases according to role.
- Internal provider capability/configuration, incident monitoring, merchant review and audit tools under least privilege.

### Internal interfaces

- Submit approved provider attempts.
- Enquire status and recover pending/unknown outcomes.
- Verify/process provider callbacks.
- Publish committed domain events.
- Deliver merchant webhooks.
- Import and reconcile provider/settlement evidence.
- Execute reports/notifications and manage quarantined work.

## 9. Contract-to-screen alignment

Use DOC-55 as the frontend conceptual binding matrix. For each screen/action, the eventual implementation contract must record:

1. Caller type and required role/scope/capability.
2. Canonical data source and whether data may be eventually consistent.
3. Whether the action is read-only, configuration-changing or financially consequential.
4. Required confirmation, step-up authentication, independent approval and idempotency behavior.
5. Loading, empty, validation, permission-denied, pending, unknown, success, failure and retry UI states.
6. Correlation/reference values shown to the user and safe support diagnostics.
7. Audit/event side effects and recovery path.
8. Stable requirement IDs and concrete acceptance tests once implementation is authorized.

The UI must not enable an action merely because a route or button exists. It must reflect backend capability, merchant activation, authorization and operation state. Backend authorization remains authoritative even if the UI hides the control.

## 10. Versioning and compatibility

Before implementation, define public API versioning and compatibility policy, including how fields are added/deprecated, how unknown enum values are handled, how event payloads evolve and how old idempotency records are interpreted.

Webhook event versions and public API versions should be explicit and independent where necessary. Internal task envelopes may evolve on a different schedule, but workers must safely handle old persisted task versions during deployment and rollback.

Do not expose internal database models directly as public response schemas. Public contracts should be designed around stable business concepts and reviewed for data minimization and tenant leakage.



### 10.1 Public API/event contract authority and compatibility — product-owner-approved direction

DOC-49 §14.1 is the proposed merchant-facing API/event contract source and records the product owner's internal direction for versioned schemas, truthful lifecycle semantics, stable errors, bounded cursor pagination, explicit API/event versions, durable event IDs, authenticated signatures, at-least-once delivery, audited replay and data minimization. This blueprint remains the planning source for internal operation envelopes, worker/task versioning and service-to-service authorization; it must not introduce conflicting public wire semantics.

For every operation family, reconcile:
- the public request/response and error contract in DOC-49;
- canonical operation/attempt semantics in DOC-41 and provider evidence in DOC-48;
- ledger/reconciliation effects in DOC-42;
- threat/control requirements in DOC-43;
- requirement/test traceability in DOC-31/DOC-34 and evidence handling in DOC-45.

**Still open:** final schemas/routes, stable error catalogue and HTTP mapping, cursor integrity/limits, API/event compatibility and deprecation policy, task-envelope migration/rollback, webhook signing test vectors and key rotation, delivery retry/expiry values, replay authority, and requirement-level traceability. These need technical/API, security, finance/operations and any required independent review. Product-owner direction does not approve a physical architecture or authorize coding/tests.

## 11. Security and privacy requirements

- Never return provider credentials, API secret material, internal signing keys or full authentication headers.
- Avoid secrets and personal data in URLs, analytics, logs, errors and correlation IDs.
- Protect callback and webhook destinations against forgery, replay, SSRF, DNS/IP edge cases and unsafe redirects using an approved threat-reviewed design.
- Apply rate limits to authentication, status enquiries, idempotency-key abuse, report/export generation and recovery actions.
- Require explicit authorization and audit for API key rotation/revocation, IP allowlist changes, beneficiary changes, payout approvals, reconciliation adjustments and support overrides.
- Restrict raw provider evidence and callback payload access; redact by default and retain only under approved policy.
- Use environment-bound credentials and references; reject cross-environment use.
- Preserve tenant isolation across API, database access, events, queues, exports and support tooling.

Security controls and exact technical mechanisms must be reconciled with DOC-43 and reviewed independently before G2 approval.

## 12. Requirement and evidence mapping

Each operation family must be mapped to the relevant existing DOC-31 requirement IDs before implementation. Do not create duplicate IDs simply because an operation appears in this blueprint. Update DOC-31 first if a genuinely new requirement is discovered.

Use DOC-34 for requirement-level mapping and DOC-45 for evidence quality. Each eventual test record must identify the test artifact, case, level, environment, commit, synthetic test data, run date, result and reviewer where required. Test labels in a planning document are not test evidence.

Proposed review themes:

- Cross-tenant access attempts for every read/write and worker-driven action.
- Credential revocation, rotation, scope restrictions and secret non-disclosure.
- Idempotency under duplicates, concurrent requests, mismatched fingerprints and timeouts.
- Operation accepted but provider times out; callback arrives before/after enquiry; duplicate/out-of-order callback.
- Refund and payout concurrent operations, limits, approvals, beneficiary changes and uncertain submission.
- Event/outbox crash boundaries, queue retries, worker lease expiry and dead-letter replay.
- Webhook signature, replay, ordering, endpoint outage, secret rotation and manual redelivery.
- Error responses, rate limiting, report/export permissions and safe redaction.
- API/event compatibility during rolling deployment and rollback.
- Reconciliation evidence, duplicate imports, discrepancy resolution and audit completeness.

No tests have been created or run by this document.

## 13. Decisions required before final API specification

1. Approve caller identity and authentication for merchant API, dashboard, hosted checkout, callbacks and internal workers.
2. Approve tenant/project context, credential scopes and role/capability enforcement model.
3. Approve the API versioning, naming, schema, error and correlation conventions.
4. Approve idempotency scope, request fingerprint, retention and conflict response.
5. Approve final canonical operation states and provider-specific finality/uncertainty mapping.
6. Approve which operation families are enabled at launch; provider and legal eligibility remain hard gates.
7. Approve webhook/event payloads, signing, retries, ordering and replay behavior.
8. Approve refund/payout authorization, approval, limits and beneficiary controls.
9. Approve ledger side effects, fees, tax, settlement, reconciliation and correction workflows.
10. Approve data exposure, retention, pagination, export, audit and support-access policy.
11. Select persistence, transaction/outbox, queue and worker concurrency mechanisms consistent with DOC-57.
12. Reconcile every contract with DOC-31/DOC-34 and obtain accountable product, technical, security, finance and qualified legal/compliance review.

## 14. Related documents

- DOC-12 — API contracts and versioning (planned detailed contract specification).
- DOC-13 — API keys, IP controls and credential security.
- DOC-14 — Webhooks and event delivery.
- DOC-31 — Launch requirements and acceptance criteria.
- DOC-34 — Requirement-level traceability matrix.
- DOC-40 — G2 architecture and financial-safety baseline.
- DOC-41 — Transaction state machine and provider mapping.
- DOC-43 — Threat model and security controls.
- DOC-45 — Evidence register and acceptance-test planning standard.
- DOC-48 — Provider adapter contract and operation capability matrix.
- DOC-49 — Merchant API specification and developer integration guide.
- DOC-51–DOC-55 — Dashboard roles, navigation, design, screen blueprint and frontend API data binding.
- DOC-56 — Backend domain model and data ownership matrix.
- DOC-57 — Backend service boundaries and end-to-end data flow.

## 15. Gate and authorization statement

This is a contract-planning proposal, not an approved API contract or OpenAPI specification. It does not authorize implementation, test execution, provider contact, credential generation, live financial activity, merge or deployment.

**G1 remains NOT PASSED. G2 remains NOT APPROVED.** Final routes, verbs, schemas, headers, error codes and provider semantics remain open until requirements, architecture and accountable reviews are complete. Any implementation, tests, provider contact, production credentials, financial activity, merge or deployment requires separate explicit authorization and all relevant gates.