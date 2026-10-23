# HARAI — Backend Service Boundaries and End-to-End Data Flow

**Status:** Draft architecture-planning proposal; not approved for implementation.
**Scope:** Logical responsibilities, workflow sequencing, consistency boundaries, asynchronous recovery and operational handoffs.
**Gate:** G1 requirements approval remains NOT PASSED. G2 architecture approval remains NOT APPROVED.

## 1. Purpose and interpretation

This document connects the conceptual domain and ownership model in DOC-56 to the API and operational workflows described in DOC-40–DOC-49 and DOC-55. It describes responsibilities and data movement, not final endpoints, payload schemas, queue products, deployment topology or physical database design.

Treat each item below as a **logical module or responsibility**, not necessarily a separately deployed service. A modular monolith may be simpler initially; splitting modules into independently deployed services creates additional network, consistency, security and operations costs. The deployment choice remains open and requires G2 review.

## 2. Logical responsibility map

| Responsibility | Owns or coordinates | Must not do |
|---|---|---|
| API edge and request validation | Versioning, request shape checks, request IDs, rate limits, safe error translation | Decide financial truth based only on HTTP acceptance or browser state |
| Authentication and authorization | Credential/session validation, scopes, role/capability checks, tenant context | Trust a client-supplied tenant ID without authorization |
| Merchant, project and capability configuration | Merchant status, project configuration, enabled methods, limits and policy references | Enable a provider capability without verified permission and activation evidence |
| Payment operation coordinator | Canonical operation lifecycle, idempotency coordination, orchestration and provider-attempt linkage | Treat an external network call as part of a database transaction |
| Provider adapter boundary | Provider-specific request/response mapping, authentication, reference handling, supported capabilities | Invent provider behavior or silently normalize away meaningful uncertainty |
| Callback/webhook ingress | Authenticate or verify provider messages, preserve safe evidence, correlate, deduplicate and acknowledge per contract | Treat an unauthenticated or uncorrelated message as authoritative |
| Status enquiry and recovery | Resolve pending/unknown outcomes using provider enquiry, callbacks and reconciliation evidence | Blindly resubmit an ambiguous financial operation |
| Ledger and financial accounting | Immutable journal entries, balanced postings, fee/obligation accounting and corrections | Rewrite posted history or use floating-point arithmetic for money |
| Event/outbox publisher | Reliably publish committed domain events to internal consumers | Publish an event that describes a database change which did not commit |
| Merchant webhook delivery | Signed event delivery, retry schedule, delivery attempts and endpoint health | Couple merchant endpoint availability to payment-state persistence |
| Refund/reversal coordinator | Eligibility, original-operation linkage, bounds, lifecycle and accounting consequences | Treat refunds, reversals, cancellations and payout returns as interchangeable |
| Money-out coordinator | Beneficiary controls, funding/eligibility, approvals, provider attempt and uncertain-outcome recovery | Submit live payouts without explicit authority, funding and policy checks |
| Reconciliation | Compare internal operations, provider reports and settlement evidence; track discrepancies | Silently force balances or mark a discrepancy resolved without evidence |
| Support, audit and case handling | Traceable investigations, restricted support access, decision records and evidence links | Expose secrets or grant support staff unbounded financial permissions |
| Reporting and read models | Derived dashboards, exports and operational summaries | Become the source of truth for balances or operation finality |
| Observability and incident operations | Correlation, metrics, alerts, runbooks, incident records and recovery ownership | Log API keys, provider secrets, authentication headers or unnecessary personal data |

## 3. Cross-cutting request and identity path

Every state-changing request should follow a consistent conceptual path:

1. Validate transport, request size, version and basic syntax.
2. Authenticate the caller and derive the principal from trusted credentials or session state.
3. Resolve the merchant/project scope from that principal and verify the requested action against role, scope, merchant status and capability policy.
4. Apply abuse controls and operation-specific limits before costly or consequential work.
5. Validate the requested operation against the approved contract and current business rules.
6. Reserve or inspect the idempotency key in its documented scope; reject reuse with a materially different request fingerprint.
7. Persist the operation and its initial lifecycle record before external submission, using the chosen atomic consistency boundary.
8. Return only the contractually permitted response. A response that says accepted or pending is not proof of financial success.

All downstream work must carry a stable internal operation identifier and correlation context. Tenant scope must be checked again at data-access and worker boundaries; it must not depend solely on the initial controller or UI check.

## 4. Collection initiation and completion

~~~mermaid
flowchart TD
  A[Merchant API request] --> B[Authenticate and authorize tenant/project]
  B --> C[Validate capability, limits and request]
  C --> D[Reserve idempotency key and create operation]
  D --> E[Commit operation plus durable work/outbox record]
  E --> F[Worker claims provider attempt]
  F --> G[Provider adapter submits request]
  G --> H{Definitive result?}
  H -- No / timeout --> I[Mark pending or unknown]
  I --> J[Enquiry, callback or reconciliation recovery]
  J --> K[Validate evidence and apply legal state transition]
  H -- Yes --> K
  K --> L[Commit state change, journal effects and domain event atomically where required]
  L --> M[Publish internal event]
  M --> N[Merchant webhook delivery and read-model updates]
  K --> O[Reconciliation compares provider and settlement evidence]
~~~

### Collection workflow rules

- The payment operation and provider attempt are separate concepts. A single operation may have multiple safe status checks or provider interactions, but a new financial submission must not be created merely because a request timed out.
- Never hold a database transaction open while waiting for a provider network response. Persist intent, perform external I/O, then persist the observed result under concurrency controls.
- If the provider response is lost or ambiguous, record the uncertainty and use the provider's documented enquiry/callback/reconciliation mechanisms. Do not convert timeout into failure or success by assumption.
- A valid callback, enquiry response or settlement record is evidence to evaluate, not a reason to bypass correlation, deduplication or legal state-transition rules.
- A successful state transition and its required journal entries/domain event must share a consistency strategy that prevents a success state with missing required accounting effects. The exact transaction/outbox design remains open.
- Customer-facing checkout should show pending or further-action states honestly. Browser redirects and user-visible messages do not independently establish financial finality.

## 5. Payment links and hosted checkout

1. An authorized merchant creates a link or checkout session under its tenant/project scope and configured capability set.
2. The system stores expiry, permitted amount/currency, usage policy, merchant reference and applicable policy version. Sensitive values must not be placed in public URLs unless specifically designed for that purpose.
3. A customer opens the hosted experience. The page resolves the opaque reference, checks expiry/status and applies safe anti-enumeration and abuse controls.
4. The customer reviews the merchant, amount, currency and payment instructions and performs any required payment-provider authorization.
5. Checkout invokes the same canonical collection coordinator and idempotency/state rules as a direct API request; it must not create a parallel, weaker payment engine.
6. Completion, expiry, cancellation and repeated visits are handled by an explicit lifecycle. Concurrent attempts must not violate single-use or amount rules.
7. The merchant dashboard and webhook delivery read the canonical operation/event records; they do not infer success from a return URL.

Open decisions include the exact link lifecycle, whether partial payment is supported, reuse policy, guest/customer data minimization, checkout session binding and how browser sessions relate to merchant API operations.

## 6. Refunds, reversals and corrections

1. Authenticate the caller and verify tenant ownership, role, capability, policy, original operation state and available refundable amount.
2. Enforce idempotency and atomic bounds so concurrent refund requests cannot collectively exceed the permitted amount.
3. Create a distinct refund/reversal operation linked to the original transaction, preserving the original record.
4. Where policy requires approval, persist the approval request and evidence before any provider submission. The requester must not satisfy a required independent approver role.
5. Submit through the provider adapter only when the operation is supported and authorized; record the provider attempt and all references.
6. Resolve pending/unknown outcomes through the applicable enquiry, callback and reconciliation path. Never blindly resubmit an ambiguous refund.
7. Record approved accounting effects through immutable balanced postings and publish the committed lifecycle event.

Refund, provider reversal, customer cancellation, payout return and accounting correction are distinct concepts. Each needs provider-specific semantics, eligibility, timing and ledger treatment before implementation.

## 7. Money-out / payout flow

~~~mermaid
flowchart TD
  A[Authorized payout request] --> B[Check tenant, role, beneficiary and destination controls]
  B --> C[Check capability, available funding, limits and risk policy]
  C --> D{Approval required?}
  D -- Yes --> E[Independent approval and audit evidence]
  D -- No --> F[Persist payout operation and idempotency record]
  E --> F
  F --> G[Commit durable submission work]
  G --> H[Provider adapter submits once under approved policy]
  H --> I{Definitive outcome?}
  I -- No / timeout --> J[Unknown or pending; freeze unsafe resubmission]
  J --> K[Enquiry, callback and reconciliation]
  I -- Yes --> L[Validate provider evidence]
  K --> L
  L --> M[Commit state and required journal effects]
  M --> N[Notify authorized merchant users and audit]
~~~

Money-out is especially sensitive to beneficiary substitution, replay, compromised credentials, duplicate submissions, insufficient funds and ambiguous provider outcomes. Beneficiary creation/change should have separate controls, notification, cooldown or step-up approval where approved by risk policy. Exact rules and thresholds remain open.

No live payout path may be enabled until written provider permission, funding and settlement model, merchant eligibility, independent finance/security review, operational ownership and explicit implementation/release authorization are established.

## 8. Provider callback ingress

~~~mermaid
flowchart TD
  A[Provider callback received] --> B[Enforce transport and size limits]
  B --> C[Verify signature/authentication and timestamp/replay rules]
  C --> D{Authentic and acceptable?}
  D -- No --> E[Reject or quarantine per provider contract; audit safely]
  D -- Yes --> F[Persist minimal callback evidence and deduplication identity]
  F --> G[Correlate provider reference to expected attempt/operation]
  G --> H{Known and valid correlation?}
  H -- No --> I[Quarantine for investigation; do not guess]
  H -- Yes --> J[Queue idempotent callback processing]
  J --> K[Validate allowed state transition and provider semantics]
  K --> L[Commit state, accounting effects and outbox event as required]
  L --> M[Acknowledge/process outcome per provider contract]
~~~

Callback-specific controls:

- Use the exact provider authentication/signature algorithm, key handling and canonicalization rules confirmed for the selected provider and environment. Do not assume one provider's contract applies to another.
- Defend against replay, duplicate delivery, altered payloads, stale timestamps and out-of-order events. Store only the evidence needed for audit and recovery, with retention/access controls.
- A callback with a valid signature can still be stale, duplicated, miscorrelated or semantically invalid for the current operation state.
- Do not trust a provider reference supplied by the merchant as sufficient correlation; verify it against the persisted attempt and expected provider account/environment.
- Keep ingress acknowledgement behavior aligned with each provider's documented timeout/retry behavior. If processing is asynchronous, durably persist enough to process later before acknowledging where the provider contract permits this.
- If the provider does not retry callbacks, recovery must rely on enquiry/reconciliation and monitoring; do not assume callback delivery is guaranteed.

## 9. Internal events, outbox and merchant webhooks

Domain events should represent committed facts, such as a canonical operation state transition or a reconciliation discrepancy being opened. Where a database update must reliably trigger downstream work, use an approved transactional outbox or an equivalent design with demonstrated delivery guarantees.

Internal event consumers must be idempotent and tolerate duplicates, delay and reordering. Delivery state for a merchant webhook is separate from payment state: a merchant endpoint outage must never roll back a payment or change a successful transaction to failed.

Merchant webhook delivery should have:

- A stable event ID and a documented event version.
- A signed request using a secret isolated to the correct merchant endpoint and environment.
- A timestamp/replay defense and a documented signature verification procedure.
- Bounded retries with backoff, observable attempts and an eventual failed/dead-letter state.
- Safe manual redelivery that reuses the original event identity and cannot repeat a financial operation.
- Endpoint disablement, secret rotation and incident controls that do not expose other tenants' data.
- Clear separation between event creation, delivery attempts and merchant acknowledgement.

Exact signature format, retry schedule, retention, ordering guarantees, endpoint policy and payload contract are open API/security decisions; see DOC-14, DOC-43, DOC-45, DOC-48 and DOC-49.

## 10. Ledger, reconciliation and read models

Ledger posting must be driven by validated canonical financial events, not directly by UI clicks or untrusted callbacks. The journal must preserve immutable history and balanced postings in the approved accounting model. Corrections should use compensating entries with references to the original entries rather than rewriting history.

Reconciliation compares independently sourced records, including where available internal operations, provider transaction reports, provider fees, settlement statements, bank/settlement evidence and payout outcomes. A transaction status report is not necessarily settlement proof.

Discrepancies should be persisted as owned cases with type, amount/currency, related references, evidence, age, priority, permitted actions and resolution approval. A human resolution must record who decided, what evidence was used, and the accounting/state effects. Never auto-close a financial discrepancy merely because a timer expired.

Dashboards, exports and analytics should be derived read models. Eventual consistency must be labelled where relevant; balance-affecting decisions must consult the approved authoritative accounting/operation sources.

## 11. Atomicity and consistency boundaries

| Boundary | Required property | Planning implication |
|---|---|---|
| Idempotency key + operation creation | One effective operation per documented key scope and request fingerprint | Persist the reservation and operation atomically; define conflict behavior and expiry/retention |
| Operation state + required internal work | No committed state change whose required next step is silently lost | Use an outbox or equivalent durable handoff; recovery must detect stuck work |
| Provider call | No database transaction spans an unbounded external call | Persist before I/O; persist result after I/O; model unknown outcomes explicitly |
| Callback evidence + deduplication | Duplicate callbacks cannot cause duplicate financial effects | Persist a stable callback identity/evidence and process idempotently |
| Canonical state + ledger posting | Financially significant state and required accounting effects cannot diverge silently | Use one transaction where feasible or a rigorously specified recoverable posting protocol |
| Refund amount bound + reservation | Concurrent refunds cannot exceed policy limits | Lock/version/reserve the available amount atomically before external submission |
| Payout approval + submission eligibility | A submission cannot bypass required approval or beneficiary controls | Bind approval to the exact operation fingerprint and policy context |
| Reconciliation decision + audit | Discrepancy closure is traceable and cannot erase evidence | Persist evidence references, decision, actor, timestamp and resulting corrections |
| Event creation + delivery work | Committed events are not lost when workers or networks fail | Separate event creation from repeated delivery attempts |

All monetary amounts must use the approved exact representation (for example integer minor units where currency rules permit), never binary floating point. Cross-currency calculations require explicit currency, rate source, precision and rounding policy. The final ledger and currency rules remain subject to DOC-42 and finance review.

## 12. Queues, workers and retry discipline

Background work may include provider submission, status enquiry, callback processing, outbox publication, merchant webhook delivery, reconciliation import, report generation and notifications. The eventual queue/worker technology is undecided.

Every asynchronous task needs a durable identity, tenant-safe context, operation/event reference, bounded attempt policy, timeout, retry classification, next-run/lease handling, and observable terminal or quarantined state. Workers should be idempotent and safe after crash/restart. A stale worker must not overwrite a newer state; use concurrency/version checks or equivalent fencing.

Retry policy must distinguish:

- **Safe read/retrieval:** status enquiries and report fetches may usually retry within provider limits.
- **Known pre-submission failure:** retry only when evidence establishes that no financial request was accepted and policy allows it.
- **Ambiguous submission:** do not blindly retry; reconcile using the original provider reference/idempotency facility, status enquiry, callbacks and support escalation.
- **Webhook delivery:** retry delivery independently of payment lifecycle, with bounded backoff and dead-letter/manual redelivery.
- **Permanent validation/authentication failure:** stop rapid retries, alert or quarantine, and require configuration correction.

Queue exhaustion, poison messages, provider outage, rate limiting, duplicate tasks, lease expiry, worker termination and partial downstream failure need documented recovery paths. A dead-letter queue is not a resolution by itself: it needs ownership, alerting, evidence retention and a safe replay procedure.

## 13. Security and tenant boundaries

- Authorize every user/API/worker action for the specific merchant, project, operation and capability. Do not rely on opaque IDs alone to prevent cross-tenant access.
- Provider credentials belong only in the approved server-side secret-management boundary. They must never be returned to merchants, embedded in frontend bundles, placed in URLs or written to logs.
- Merchant API credentials and webhook secrets must be scoped, revocable, rotated and stored using an approved secure design; reveal secrets only under an explicit safe lifecycle.
- Verify provider callbacks using provider-specific rules and prevent replay. Restrict ingress and egress according to the approved deployment and provider requirements.
- Record security-sensitive actions and privileged support access in tamper-resistant audit records with actor, scope, reason and correlation identifiers.
- Minimize personal/customer data in logs and callback evidence. Apply retention, redaction and access policies reviewed for Uganda and any future market.
- Separate production and test environments, credentials, provider references and callback destinations. Environment mismatches must fail closed.
- Protect high-risk actions with limits, risk checks and independent approval where policy requires it. Never let a support override silently manufacture a successful financial state.

## 14. Observability and incident response

Carry correlation identifiers across API requests, operations, provider attempts, callback records, queue tasks, ledger entries, webhook events and reconciliation cases. Avoid using personal data or secrets as correlation IDs.

Minimum proposed operational signals include request/error/latency rates; idempotency conflicts; counts and age of pending/unknown operations; provider timeouts and enquiry failures; callback signature failures and uncorrelated callbacks; queue age/retries/dead letters; webhook delivery success and backlog; ledger-posting failures; reconciliation lag/discrepancies; payout/refund exceptions; and privileged-action alerts.

Every critical alert needs an accountable owner, severity, runbook, escalation route and recovery objective. Numeric SLOs, retention, on-call coverage and incident communication obligations are open decisions. Recovery procedures must not resolve uncertainty by changing a financial state without evidence.

## 15. Deployment topology: deliberately unresolved

Logical responsibilities can be implemented as modules in one application or separated into services. Do not create microservices merely because this document lists responsibilities.

Before selecting topology, compare:

- Transactional consistency requirements for operation state, ledger and outbox.
- Expected throughput, provider limits, queue behavior and operational staffing.
- Tenant isolation, secret boundaries and independent scaling needs.
- Deployment, rollback, observability, disaster recovery and incident complexity.
- Database ownership and migration strategy, including whether any data store is shared.
- Cost and maintainability for an initial Uganda-first service.

Any topology must preserve the same externally observable financial invariants and recovery behavior. Database, queue, cache, deployment platform, outbox mechanism and service split remain open pending G2 review.

## 16. Decisions still required

1. Approve the API/authentication/session architecture and exact tenant-context propagation.
2. Approve operation and idempotency scopes, key retention, request fingerprinting and conflict behavior.
3. Select modular-monolith versus service topology based on a documented trade-off.
4. Select persistence, queue, outbox/CDC and worker lease/concurrency mechanisms.
5. Define provider-specific adapter contracts and written authorized capabilities for each operation.
6. Define exact state transitions, provider finality evidence and safe retry/enquiry rules.
7. Approve callback authentication, replay prevention, raw/minimal evidence retention and acknowledgement behavior per provider.
8. Approve merchant webhook signing, versioning, ordering, retry, redelivery and endpoint-disablement policy.
9. Approve ledger accounts, fee/tax treatment, posting triggers, rounding, refunds/reversals and finance controls.
10. Approve payout funding, beneficiary changes, approvals, limits and unknown-outcome recovery.
11. Define reconciliation sources, cadence, discrepancy ownership, resolution approvals and settlement evidence.
12. Define monitoring/SLOs, incident ownership, on-call support, backup/restore and disaster-recovery objectives.
13. Define data retention, privacy, audit access, support access and secrets-management approach.
14. Reconcile all decisions with DOC-16, DOC-17, DOC-40–DOC-49, DOC-55–DOC-56 and the 54 requirement IDs in DOC-31/DOC-34.

## 17. Proposed review and verification themes

The following are planning themes only; no tests have been created or run by this document.

- Tenant isolation and cross-tenant negative cases for every API, background worker, webhook, export and support action.
- Idempotency under duplicate, concurrent and mismatched requests.
- Crash points before submission, after submission, after provider acceptance, before state persistence, before ledger posting and before event publication.
- Delayed, duplicate, malformed, forged, replayed, out-of-order and uncorrelated callbacks.
- Provider timeouts, throttling, status enquiry, eventual finality and reconciliation recovery.
- Concurrent refunds, payout approvals, beneficiary changes, limits and duplicate-submission prevention.
- Balanced ledger invariants, immutable correction history, rounding and multi-currency boundaries.
- Outbox/queue redelivery, worker crash/restart, stale leases, poison tasks and dead-letter recovery.
- Merchant webhook retries, signing, secret rotation, endpoint failure and safe manual redelivery.
- Reconciliation import duplication, missing records, settlement mismatch, discrepancy ownership and evidence-based closure.
- Audit completeness, sensitive-data redaction, secret leakage prevention and privileged-access restrictions.
- Operational alerting, runbook usability, backup/restore and recovery objectives once approved.

Each eventual test must map to a stable requirement ID and record environment, commit, test data, result, date and evidence. Mock/provider-sandbox success alone does not prove contractual production permission or operational readiness.

## 18. Related documents

- DOC-16 — Data model and database integrity (planned physical integrity specification).
- DOC-17 — Architecture and service boundaries (existing index entry; reconcile its status and scope with this document).
- DOC-31 — Launch requirements and acceptance criteria.
- DOC-34 — Requirement-level traceability matrix.
- DOC-40 — G2 architecture and financial-safety baseline.
- DOC-41 — Transaction state machine and provider mapping.
- DOC-42 — Ledger and reconciliation design proposal.
- DOC-43 — Threat model and security controls.
- DOC-45 — Evidence register and acceptance-test planning standard.
- DOC-48 — Provider adapter contract and operation capability matrix.
- DOC-49 — Merchant API specification and developer integration guide.
- DOC-55 — Frontend API contract and data-binding matrix.
- DOC-56 — Backend domain model and data ownership matrix.

## 19. Gate and authorization statement

This document is a conceptual design proposal only. It does not approve a funds-flow model, provider integration, database schema, endpoint contract, deployment topology, live capability, payment-engine implementation, tests, provider outreach, credential creation, real transactions, pull-request merge or deployment.

**G1 remains NOT PASSED. G2 remains NOT APPROVED.** Continue with review, evidence gathering and decision recording. Any implementation, tests, provider contact, production credentials, financial activity, merge or deployment requires its own explicit authorization and all relevant provider, legal/compliance, security, finance and operational gates.

## 20. Tenant context across synchronous and asynchronous boundaries — proposed clarification

Apply one consistent tenant-binding rule across the data-flow diagrams:

- **Inbound merchant API/dashboard:** authenticate first; resolve tenant membership, project and environment from trusted server-side state; authorize the requested action against the target resource's stored owner. Do not accept caller-supplied tenant context as proof.
- **Callback ingress:** validate provider-specific authentication and schema; correlate against a persisted provider-attempt identifier/reference; derive the tenant through the stored attempt-to-operation relationship. Callback-supplied tenant or merchant fields are comparison data only, never routing authority. Unmatched/ambiguous events are durably quarantined for investigation.
- **Outbox, queues and scheduled jobs:** commit a stable internal operation/event identity with the source change; enqueue only a reference plus expected ownership/environment and correlation metadata. The worker reloads authoritative state and checks owner consistency, current permissions/capability, operation state and idempotency before side effects. A mismatch is a hard stop with an audit record, not an automatic rebind.
- **Merchant webhooks and exports:** select endpoint/subscription and payload from the authorized tenant-bound event, not from free-form job fields. Revalidate endpoint version and delivery eligibility. Exports must bind the query and artifact to the requester/tenant, enforce authorization again at download, and expire/revoke access as policy requires.
- **Support/admin operations:** use a separate privileged authorization decision, case/reason, target scope and actor audit. Read access does not imply mutation permission; support tooling cannot directly force a payment state or bypass the canonical transition/correction workflow.
- **Cache, object storage and read models:** tenant/environment/resource scope must be part of every relevant key and access policy. Derived data must retain source ownership and rebuild rules. Private evidence and exports must not use public URLs as their authorization mechanism.
- **Recovery and replay:** after restore or queue redrive, keep consequential workers paused until ownership, idempotency, provider-attempt state and pending-operation reconciliation checks are complete. Replayed work re-enters the same authorization and idempotency gates.

The above are proposed requirements for later architecture decisions and tests, not claims that the current system implements them. Database row-level security, specific queue products, storage paths and deployment topology remain unresolved. G1 remains NOT PASSED; G2 remains NOT APPROVED.
