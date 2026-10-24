# DOC-49 — Merchant API Specification and Developer Integration Guide

**Status:** Draft proposal — API/security decisions and G1/G2 approval remain open  
**Related documents:** DOC-00, DOC-03, DOC-12, DOC-13, DOC-14, DOC-31, DOC-34, DOC-41, DOC-43, DOC-45, DOC-46, DOC-48  
**Audience:** Merchant developers, HARAI product/technical/security reviewers  
**Purpose:** Define a stable proposed contract for merchants integrating their websites and applications with HARAI. This is a specification proposal, not an implemented API or a claim of provider authorization.

> **Safety and authority boundary:** Examples use fake values. Never place real credentials, personal data, provider secrets, or transaction data in this document. The API contract does not grant HARAI permission to aggregate or process payments for other merchants. G0 remains in progress, G1 is NOT PASSED, and G2 is NOT APPROVED. This document does not authorize payment-engine implementation, provider contact, live credentials, real transactions, merge, or deployment.

## 1. Scope and non-goals

### In scope
- Merchant-server authentication and tenant-scoped authorization.
- Creating and retrieving payment intents.
- Idempotent requests and safe handling of timeouts/unknown outcomes.
- Consistent request/response schemas, errors, pagination and versioning.
- Proposed HARAI-to-merchant webhook events and verification.
- Low-friction developer onboarding, direct production-access requests and optional sandbox integration/conformance-test planning.

### Out of scope pending separate decisions
- Final funds-flow model, settlement/custody responsibilities, provider permissions, pricing or fees.
- Final merchant verification/KYC rules and merchant eligibility categories.
- A selected hosted-checkout implementation or card processor/acquirer.
- A finalized credential/authentication mechanism, numeric rate limits or webhook retry schedule.
- Production API URLs, live credentials, production enablement, or a claim that any endpoint currently exists.

## 2. Environments, base paths and versioning

The following are **placeholders**, not actual HARAI domains:

| Environment | Proposed base URL | Intended use |
|---|---|---|
| Sandbox | `https://sandbox-api.example.invalid` | Simulated/test integrations only |
| Production | `https://api.example.invalid` | Reserved until production domain, controls and release gate are approved |

The reserved `.invalid` domains deliberately prevent accidental use. Replace them only through an approved environment/domain decision; do not publish them as working endpoints.

- The proposed version prefix is `/v1`.
- A versioned contract must define request schemas, response schemas, errors and webhook event versions.
- Additive optional fields may be considered backward-compatible only after a formal compatibility policy is approved. Removing/renaming fields or changing meanings requires a new version or explicit migration plan.
- Unknown response fields should be ignored by clients; clients must not assume unknown status values are terminal.
- Send a non-secret `X-Request-Id` when supported. HARAI should return its own request/correlation identifier in every response.
- Production and sandbox credentials, provider accounts, data stores, webhook secrets and routing must be isolated. Sandbox credentials must never work in production, or vice versa.

## 3. Authentication and merchant authorization

### 3.1 Proposed baseline (decision required)

A server-to-server credential model with scoped, revocable merchant credentials is a candidate baseline, but the final choice requires the DOC-43 security review and an explicit architecture decision. Do not treat this proposal as approved authentication design.

Candidate options to compare include:
- Random, high-entropy API keys with a public key identifier and secret component; store only a suitable verifier/hash where feasible, display the secret once, support rotation/revocation, and scope every key to a merchant and environment.
- OAuth 2.0 client credentials with short-lived access tokens if operational complexity and client support justify it.
- Mutual TLS for selected higher-trust integrations as an optional future control, not a replacement for authorization.

No credential values belong in the repository, client-side JavaScript, mobile-app bundles, URLs, analytics, error messages or logs. Merchant secrets must be sent only over verified HTTPS and stored in the merchant's server-side secret store. A browser/mobile client must never receive a merchant secret. If a browser needs to initiate checkout, the merchant backend should create the payment intent and return only a narrowly scoped, short-lived checkout reference/token under a separately approved checkout contract.

### 3.2 Authorization rules

- Resolve merchant and tenant identity from the authenticated credential/session, not from an untrusted request body.
- Every object lookup must enforce tenant ownership. Knowing or guessing another merchant's ID, payment ID, reference or event ID must not grant access.
- Use least-privilege scopes, such as proposed `payment_intents:write`, `payment_intents:read`, `refunds:write`, and `webhooks:read`; names and granularity require review.
- Refunds, money-out, credential changes, beneficiary changes and other sensitive actions need separate permissions and any required approvals.
- IP allowlisting may be defense-in-depth, never the only authentication control. Define trusted proxy behavior before relying on client IP; reject spoofed forwarding headers. Provide a secure recovery path for merchants with changing IP addresses.
- Revocation, rotation, compromise response, owner identity verification and audit events must be specified before live access.

## 4. Proposed resource model

### 4.1 Payment intent

A payment intent is HARAI's durable record of the merchant's request to collect a specified amount. It is not proof that a provider accepted, completed or settled the payment.

Proposed fields:

| Field | Meaning |
|---|---|
| `id` | Opaque HARAI payment-intent identifier |
| `merchant_reference` | Merchant-supplied business reference, unique only within a defined merchant scope |
| `amount_minor` | Positive integer in the currency's approved minor-unit convention; never a floating-point number |
| `currency` | ISO currency code; UGX is the initial product direction, but allowlist and exponent rules require approval |
| `status` | Canonical status from the approved state model; current mapping proposal is DOC-41 |
| `description` | Optional, bounded, non-sensitive description |
| `metadata` | Optional allowlisted key/value data; never use it for secrets or sensitive personal data |
| `checkout_url` | Optional only if an approved hosted-checkout product is selected; not guaranteed by this draft |
| `created_at`, `updated_at` | ISO 8601 timestamps in UTC |
| `expires_at` | Optional expiry only when supported by the approved lifecycle |
| `request_id` | Non-secret correlation identifier |

The exact schema, reference constraints, currency rules, expiry behavior and checkout behavior are open decisions. Do not accept a provider account ID, tenant ID, fee override, final status, or settlement instruction from arbitrary merchant input.

### 4.2 Proposed status contract

The current internal state-machine proposal in DOC-41 includes `CREATED`, `REQUIRES_ACTION`, `SUBMITTING`, `PENDING`, `UNKNOWN`, `SUCCEEDED`, `FAILED`, `CANCELLED` and `EXPIRED`. These remain draft states pending review.

- `PENDING` and `UNKNOWN` are not successful payment.
- An HTTP 2xx response means the API request was accepted according to the endpoint contract, not that money was collected.
- Provider acceptance, callback receipt, timeout, browser redirect or merchant return URL must not alone be treated as proof of success.
- Merchant systems should use authenticated status retrieval and verified signed events, then apply their own idempotent fulfillment logic.
- The finality and transition rules must match the approved state machine and provider evidence.

## 5. Proposed endpoints

All paths below are **proposed, unimplemented contract examples**.

| Method and path | Purpose | Candidate scope |
|---|---|---|
| `POST /v1/payment-intents` | Create a payment intent | `payment_intents:write` |
| `GET /v1/payment-intents/{payment_intent_id}` | Retrieve one intent owned by the authenticated merchant | `payment_intents:read` |
| `GET /v1/payment-intents` | List the authenticated merchant's intents with bounded filters/pagination | `payment_intents:read` |
| `POST /v1/payment-intents/{payment_intent_id}/cancel` | Request cancellation where supported and still valid | Candidate separate scope; exact semantics open |
| `POST /v1/refunds` | Request a refund linked to an original payment, only where authorized/supported | `refunds:write` |
| `GET /v1/refunds/{refund_id}` | Retrieve a refund operation | Candidate `refunds:read` |
| `GET /v1/webhook-endpoints` | View registered merchant webhook endpoint metadata | Candidate `webhooks:read` |

Do not infer that cancellation, refunds, list filters, or endpoint names are supported by a live service. Refunds and money-out need separate operation models and permissions; they must not be hidden side effects of payment-intent creation.

**Cancellation semantics — proposed, not approved:** the merchant cancellation endpoint means HARAI will stop accepting a new initiation for that intent when the cancellation guard wins before dispatch. It does not, by itself, mean a provider-side operation has been cancelled or that funds cannot move. If dispatch is already claimed or may have reached the provider, retain the attempt as PENDING/UNKNOWN and continue status recovery/reconciliation. A provider-side cancellation, void or reversal is a distinct provider-specific capability and must be represented as a separate request/result with its own evidence; do not return a terminal “payment stopped” result until the applicable contract's confirmation criteria are met. If cancellation races with dispatch, report the intent cancellation decision separately from the unresolved attempt outcome.

## 6. Create payment intent: proposed contract

### Request

```http
POST /v1/payment-intents
Authorization: Bearer <MERCHANT_SERVER_CREDENTIAL>
Content-Type: application/json
Idempotency-Key: order-2026-000184-attempt-1
X-Request-Id: req-example-7f31
```

```json
{
  "merchant_reference": "order-2026-000184",
  "amount_minor": 25000,
  "currency": "UGX",
  "description": "Order 184",
  "metadata": {
    "order_reference": "184"
  },
  "return_url": "https://merchant.example.invalid/payment-return"
}
```

This example is illustrative. The final schema must decide whether `return_url` is accepted, how destinations are registered/allowlisted, whether checkout is hosted, and whether metadata is permitted. Never trust a return URL for payment confirmation. Arbitrary redirects can create phishing/open-redirect risk and require explicit validation.

### Response (illustrative only)

```http
HTTP/1.1 202 Accepted
Content-Type: application/json
X-Request-Id: req-example-7f31
```

```json
{
  "id": "pi_example_01J0000000000000000000000",
  "merchant_reference": "order-2026-000184",
  "amount_minor": 25000,
  "currency": "UGX",
  "status": "REQUIRES_ACTION",
  "checkout_url": null,
  "created_at": "2026-10-23T10:00:00Z",
  "updated_at": "2026-10-23T10:00:00Z",
  "request_id": "req-example-7f31"
}
```

The status and HTTP code above demonstrate a possible accepted-for-processing response only; the final contract must decide whether create returns 201 or 202 and which statuses may be returned. Never return `SUCCEEDED` just because a request was created or dispatched. The fake identifier is not a real payment.

## 7. Idempotency, concurrency and timeouts

Idempotency is mandatory for payment-creating and other money-moving requests.

- The client sends an `Idempotency-Key` for each logical operation. Proposed scope is authenticated tenant/merchant + environment + operation class, with endpoint compatibility rules explicit and consistent across API routes. Credential rotation or use of another credential for the same tenant must not silently bypass deduplication. Exact scope, canonical request fingerprint, retention window, provider-reference retention and post-restore replay horizon require one approved cross-document contract with DOC-42 and DOC-48.
- The server atomically reserves the key with a canonical request fingerprint before any external side effect. The fingerprint uses a versioned canonical representation of the operation class, compatible endpoint/resource class and semantically relevant business fields (including amount, currency and beneficiary where applicable); exclude transport-only fields, volatile timestamps and secrets. Canonicalization/version changes must not silently make an old key appear new.
- Repeating the same key and equivalent canonical request returns the original logical operation/result; it must not create a second operation.
- Reusing a key with a materially different canonical request returns a stable `409 IDEMPOTENCY_CONFLICT`, preserves the original reservation and operation, and must not dispatch.
- Concurrent duplicate requests converge on one durable operation through database-enforced uniqueness/transactional reservation. A losing request reads the durable winner rather than starting another provider attempt; cache or queue locks alone are insufficient.
- Collection, refund/reversal/return and money-out are distinct operation classes with separate logical operation IDs and authorization rules. A compensating operation links to its original but does not reuse its idempotency reservation as though it were the same operation.
- Every provider attempt belongs to one logical operation. Provider-native keys/references are stored separately from the merchant key. Enforce provider-reference uniqueness within the documented provider/account/product/environment scope; ambiguous collisions are quarantined rather than attached by guesswork. Provider key/reference guarantees and retention are adapter-specific and may remain UNKNOWN.
- Deduplicate callbacks by provider event identity scoped to provider/account/product/environment when available. Otherwise use a documented, versioned fingerprint over authenticated source identity and normalized immutable fields; ambiguous collisions require review, not silent suppression. Duplicate receipts cannot repeat state transitions, ledger postings or outbound business effects.
- Enforce unique ledger posting identity per normalized financial source event + posting purpose. Reconciliation imports use stable source/report identity plus content hash: same identity and content is a no-op; changed content under the same identity creates a discrepancy. Amount or timestamp alone is not a safe uniqueness key.
- Outbound merchant-event retries/replays preserve stable event identity and immutable business content (or use an explicitly versioned replay envelope). Delivery attempts cannot create a second financial effect; receivers must deduplicate.
- If the response is lost, the client retries with the same key or retrieves the operation; it must not create a fresh key merely because the outcome is unknown. If a provider request may have been dispatched and its outcome is ambiguous, preserve `UNKNOWN` and reconcile under DOC-41/DOC-48. Never blindly repeat the provider operation.
- Idempotency retention must exceed the supported client retry window and be consistent with provider reference/event retention. Exact durations, tombstone lifetime and post-restore replay horizon remain open; do not expire the only uniqueness evidence while retries, callbacks, settlement reports or restore could reintroduce the same operation.
- After restore, pause consequential workers and replay until operation/idempotency records, provider attempts/references, event receipts, ledger postings, outbox records and import identities are cross-checked and unresolved items reconciled. Rebuild derived queues only from durable records; a missing queue item is not proof that provider dispatch never occurred. Resume requires an approved restore procedure and authorization.
- Idempotency does not replace tenant authorization, provider-specific retry rules, database uniqueness or ledger uniqueness.



### 7.1 Cross-endpoint replay and fingerprint decision rules — proposed, not approved

DOC-42 §7.2 is the consolidated decision package; this API document must not define a separate idempotency policy.

- **Compatibility rule:** API routes or versions share a key reservation only when an explicitly documented compatibility class says they represent the same logical operation and semantic fields. A changed route/version must not create a second operation for the same tenant request. If the server cannot safely resolve compatibility, it must not dispatch a second operation; return a stable conflict or direct the client to retrieve the existing operation.
- **Fingerprint rule:** store the canonicalization version with the reservation. Compare retries using the stored version and an approved migration/dual-read rule; never silently treat a key as new because canonicalization changed during deployment. Include validated financial meaning (operation class, amount, currency and applicable payer/beneficiary references); exclude secrets and transport-only fields. The exact field list and version migration remain owner decisions.
- **Credential rotation:** a credential change does not create a new uniqueness scope for the same authenticated tenant/merchant and operation class. Authorization is still evaluated on every request; idempotency is not an authorization bypass.
- **Response-loss rule:** return the existing logical operation for an equivalent retry. If the operation is unresolved, expose its current safe state and retrieval path; do not create a new provider attempt merely because the caller did not receive the first response.
- **Retention rule:** no universal duration is fixed here. The approved retry promise, provider key/reference/event horizons, reconciliation import window, dispute/correction needs and restore/replay horizon must be reconciled before records or tombstones can expire.
- **Recovery rule:** after restore, the API must not make an unresolved operation appear new because a derived queue or cache is missing. Consequential work remains paused until DOC-42 §7.2's restore checks and authorization are satisfied.

**Unresolved owner decisions:** Product/API owner and technical architecture owner must approve route compatibility and semantic fingerprint fields; technical architecture owner must approve canonicalization migration; provider integration owner must evidence provider-specific key/reference/event guarantees; Operations and Finance/reconciliation must approve retry, import and restore horizons. These remain blockers to G2 and are not implementation or test claims.

## 8. Error response contract

Proposed error envelope:

```json
{
  "error": {
    "code": "IDEMPOTENCY_CONFLICT",
    "message": "The idempotency key was already used with a different request.",
    "request_id": "req-example-7f31",
    "retryable": false,
    "details": {}
  }
}
```

Messages must be safe for merchant display and must not reveal provider secrets, internal stack traces, raw provider payloads or another tenant's data. `details` must be bounded, schema-controlled and non-sensitive.

| HTTP status | Proposed use | Client guidance |
|---|---|---|
| `400` | Malformed JSON or invalid request shape | Correct the request; do not retry unchanged |
| `401` | Missing/invalid authentication | Check server-side credential; no repeated rapid retries |
| `403` | Authenticated but unauthorized or capability blocked | Do not retry until access/permission changes |
| `404` | Resource absent or not visible to this tenant | Do not reveal whether another tenant owns it |
| `409` | Idempotency conflict or incompatible resource state | Retrieve the existing operation or resolve the conflict |
| `422` | Valid shape but invalid business values | Correct input |
| `429` | HARAI rate limit | Honor `Retry-After` if supplied; use bounded backoff |
| `500` | Unexpected internal error | Retry only according to endpoint/idempotency rules |
| `502` / `503` / `504` | Upstream or service availability issue | Outcome may be unknown; retrieve/reconcile before creating a new operation |

Candidate stable error codes include `VALIDATION_ERROR`, `AUTHENTICATION_FAILED`, `PERMISSION_DENIED`, `MERCHANT_SUSPENDED`, `CAPABILITY_BLOCKED`, `IDEMPOTENCY_CONFLICT`, `RESOURCE_STATE_CONFLICT`, `RATE_LIMITED`, `PROVIDER_OUTCOME_UNKNOWN`, `UPSTREAM_UNAVAILABLE` and `INTERNAL_ERROR`. Final codes and retry semantics require API/security review. Do not expose raw provider errors as the public API contract.

## 9. Status retrieval, filtering and pagination

- A single-resource lookup must enforce merchant ownership before returning any data.
- List endpoints must be bounded and paginated. Cursor pagination is a candidate for stable ordering; final scheme is open.
- Proposed filters: status, created-time range, merchant reference and currency, only after privacy, indexing and query-cost review.
- Validate maximum page size, date ranges and filter combinations. Do not permit unbounded exports through ordinary list endpoints.
- Return stable ordering and an opaque next-page cursor rather than exposing database offsets or internal query details.
- A status read must distinguish current canonical state from timestamps, provider references and any disclosed uncertainty.
- If the authoritative state is unresolved, return `UNKNOWN`/pending according to the approved contract rather than guessing.

## 10. HARAI outbound webhooks

HARAI merchant webhooks are separate from provider-to-HARAI callbacks. This section proposes a contract; it does not claim delivery is implemented.

### 10.1 Proposed event envelope

```json
{
  "id": "evt_example_01J0000000000000000000000",
  "type": "payment.status_updated",
  "api_version": "v1",
  "created_at": "2026-10-23T10:01:00Z",
  "merchant_id": "mer_example",
  "data": {
    "payment_intent_id": "pi_example_01J0000000000000000000000",
    "merchant_reference": "order-2026-000184",
    "amount_minor": 25000,
    "currency": "UGX",
    "status": "SUCCEEDED"
  }
}
```

All identifiers and values are fictional. Final event names, payload schemas, status exposure and event versioning must align with approved state and privacy policies. A success event may be emitted only after the canonical state transition is verified and committed.

### 10.2 Signature proposal

A candidate signing scheme is HMAC-SHA256 over a precisely defined timestamp and the exact raw request body, using a per-endpoint secret. This is a **proposed HARAI outbound webhook scheme**, not a claim about MTN/Airtel callback signing.

A possible header shape is:

```http
Harai-Webhook-Id: evt_example_01J0000000000000000000000
Harai-Webhook-Timestamp: 1792759260
Harai-Webhook-Signature: v1=<hex-signature-placeholder>
```

The final canonical signing string, encoding, digest format, timestamp tolerance, key identifier/rotation scheme and header names must be approved before implementation. Receivers should:
1. Read the exact raw body bytes.
2. Verify the signature with a constant-time comparison.
3. Reject invalid signatures and timestamps outside the approved tolerance.
4. Deduplicate by event ID durably.
5. Return a success response only after durable acceptance, then process asynchronously where appropriate.

Never include webhook secrets in event payloads or URLs. Provide a controlled secret-rotation procedure and permit overlapping active key versions only under an explicit policy.

### 10.3 Delivery and recovery proposal

- Persist events to a durable outbox in the same transaction as the relevant state change, where architecture permits.
- Deliver asynchronously with a stable event ID and bounded retry/backoff.
- Define retry intervals, maximum age, delivery timeout, response success codes, endpoint disablement and alerting before launch; numeric values remain open.
- Treat receiver 2xx as transport-level acceptance only. The receiver must deduplicate because delivery may be repeated.
- Provide delivery history and a secure manual replay feature only after permission and audit controls are designed.
- Replay must resend the same event ID and immutable event content, or create a separately identified replay envelope under an explicit contract. It must never cause a second financial side effect.
- Validate endpoint ownership and HTTPS; block loopback, private, link-local and cloud metadata destinations as applicable; defend against DNS rebinding and unsafe redirects. Revalidate destination safety on each delivery.
- Do not send full payer/beneficiary details, secrets, provider raw payloads or unnecessary personal data in events.
- Define webhook events for refund status separately if refunds are approved. Money-out events require separate permission and scope.
- Event names such as `payment.status_updated` are draft examples, not approved event taxonomy.

## 11. Production-first developer onboarding; optional sandbox

HARAI should let legitimate developers register, create projects, access API documentation and request production access without being forced through sandbox integration first. Sandbox is optional, useful for developers who want to test, and must not be represented as a prerequisite for production review.

Proposed onboarding sequence:
1. Create a developer workspace and project with proportionate account-abuse controls; provide API documentation and integration requirements.
2. Let the business begin verification and request production access directly, without a mandatory sandbox merchant or sandbox test completion.
3. Offer optional sandbox credentials through the approved secure display/rotation flow for developers who choose to test.
4. Explain API schemas, error semantics, idempotency, transaction states, webhook verification, timeout recovery and reconciliation requirements.
5. Validate HTTPS webhook ownership and destination safety before sending events in whichever environment is used.
6. Where sandbox is used, clearly mark simulated outcomes and isolate them from real money, production reports and live ledger entries. Record any tests actually run with environment, build, timestamp and evidence under DOC-45.
7. Assess production access using business verification, provider/processor permission, legal/compliance, commercial, finance, security and operations evidence. Sandbox completion is not a substitute for, or universal prerequisite to, those gates.
8. Issue separate production credentials only after required approvals; document key custody, rotation, incident response and revocation. Never place production secrets in client-side code, chat, tickets or source control.

Sandbox success does not prove provider production access, aggregator permission, settlement readiness or live-money capability; skipping sandbox proves none of these either. Do not fabricate provider responses as if they came from a real provider. Simulation records must be clearly distinguishable from real provider records.

## 12. Rate limits, privacy and operational controls

- Apply per-merchant, per-key and global limits to request rates, concurrent operations, status enquiries, list/export workloads and webhook configuration.
- Numeric quotas, burst rules, cost controls and exception policy are open decisions; do not hard-code guessed limits into documentation as approved.
- Use bounded request/body sizes, strict schema validation, timeouts and controlled upstream concurrency.
- Return `429` with a documented `Retry-After` policy where applicable; protect recovery and status enquiries from being starved by bulk/list traffic.
- Maintain audit records for credential creation/revocation, webhook configuration, refunds, money-out permissions and other sensitive changes.
- Define retention/deletion rules, access controls and redaction for payment metadata, API logs, webhook payloads and support diagnostics.
- Never put credentials, full payment secrets, authentication headers, or unnecessary personal data into logs.
- Keep card-data handling out of HARAI unless a qualified processor/acquirer is selected and a reviewed compliance/security boundary is approved. Do not collect or store raw card numbers or security codes under this proposal.
- Define service-level objectives, incident communications, API deprecation, status-page and support obligations in the operational/release documents.

## 13. Proposed conformance-test catalogue

These are planning IDs only. No tests have been implemented or run by this document.

| Test ID | Scenario | Expected property |
|---|---|---|
| MERCHANT-API-001 | Valid create request | One durable intent; response is not misrepresented as successful collection |
| MERCHANT-API-002 | Missing/invalid credential | Denied without leaking tenant/resource data |
| MERCHANT-API-003 | Merchant A reads Merchant B's intent | Denied with no cross-tenant disclosure |
| MERCHANT-API-004 | Same idempotency key and same payload concurrently | One logical operation |
| MERCHANT-API-005 | Same key reused with different payload | Stable conflict; no second dispatch |
| MERCHANT-API-006 | Client timeout after request submission | Retry/retrieval resolves the original operation safely |
| MERCHANT-API-007 | Invalid amount, currency or oversized metadata | Rejected before dispatch |
| MERCHANT-API-008 | Unknown provider/native status | No guessed terminal state |
| MERCHANT-API-009 | Forged or stale webhook signature | Receiver example rejects it; no event accepted |
| MERCHANT-API-010 | Duplicate webhook event | One durable business effect |
| MERCHANT-API-011 | Webhook endpoint points to private/metadata address | Registration or delivery blocked |
| MERCHANT-API-012 | Credential revoked or rotated | Old key denied according to approved overlap policy |
| MERCHANT-API-013 | Rate limit exceeded | Predictable 429/backoff without request storm |
| MERCHANT-API-014 | List query too broad or cursor tampered | Bounded query and safe validation |
| MERCHANT-API-015 | Refund attempted without permission/capability | Rejected before dispatch |
| MERCHANT-API-016 | Sandbox credential used in production | Rejected; environments remain isolated |
| MERCHANT-API-017 | Error path includes raw provider payload/secret | Sensitive data redacted |
| MERCHANT-API-018 | Webhook delivery retry/replay | Stable identity, bounded attempts, no duplicate financial side effect |

Evidence for executed tests must include the actual test file/case, commit/build, environment, command or run ID, timestamp, result and evidence artifact. A planned test ID is not execution evidence.

## 14. Open decisions and closure evidence

| Decision | Required decision/evidence | Status |
|---|---|---|
| API credential model | Threat-model review and explicit choice among scoped API keys/OAuth/other approach | Open |
| Merchant identity and onboarding | Approved tenant, organization verification, role and suspension model | Open |
| API schema and endpoint semantics | Reviewed OpenAPI/schema proposal; error/version compatibility policy | Open |
| Hosted checkout/session | Product decision, threat review, return URL allowlist and session lifecycle | Open |
| Currency/amount/reference rules | Finance/provider evidence and exact minor-unit conventions | Open |
| Idempotency | Scope, atomicity, retention, conflict behavior and provider interaction policy | Open |
| Webhook signing | Canonical bytes/string, headers, timestamp, key rotation and test vectors | Open |
| Webhook delivery | Retry schedule, timeout, retention, endpoint disablement and replay authorization | Open |
| Rate limits | Capacity analysis, provider quotas and approved per-merchant/global thresholds | Open |
| Refund/money-out API | Provider permission, operation lifecycle, finance/accounting and authorization controls | Open / externally gated |
| Card processing | Eligible processor/acquirer, contract and reviewed card-data boundary | Open / externally gated |
| Production domains and operations | Approved DNS/TLS, secret management, SLOs, runbooks and release evidence | Open |



### 14.1 Consolidated API/event contract rules — product-owner-approved direction, detailed decisions open

The product owner has approved the following internal contract direction on 2026-10-24. This records product intent; it is not a final OpenAPI document, technical architecture approval, provider capability statement or implementation authorization.

1. **Stable public contract:** publish versioned request/response schemas and machine-readable error codes. Keep public business resources separate from database models and provider-native payloads. Every response/error should carry a non-secret correlation identifier where appropriate.
2. **Truthful lifecycle semantics:** HTTP acceptance is not proof of collection or payout completion. Expose `PENDING`/`UNKNOWN` distinctly from terminal success/failure. A final state is permitted only under the approved state machine and method-specific evidence rules in DOC-41/DOC-48. Never infer success from a redirect, initiation acknowledgement, timeout or callback receipt alone.
3. **Consistent errors:** use the error envelope in §8 as the common proposed shape across merchant API endpoints. Error codes must be stable, bounded and non-sensitive; document retryability and client remediation. Authentication/authorization, validation, idempotency conflict, business-state conflict, rate limiting, provider rejection, unknown outcome and internal failure remain distinct categories. Exact code catalogue and HTTP mapping require technical/security review.
4. **Bounded pagination:** list APIs must use a documented, opaque cursor and deterministic stable ordering; enforce page-size, time-range and filter limits. Cursor integrity, expiry, ordering fields and behavior when records change during traversal require technical review. Ordinary list endpoints must not become unbounded export APIs.
5. **Versioning:** public API version and event schema version are explicit. Additive fields, enum evolution, deprecation, unknown-value handling, rolling deployment and rollback must be documented. Event schema evolution is independent of public API evolution where necessary. Persisted idempotency/fingerprint versions and queued task versions must remain interpretable during supported deployments and recovery.
6. **Outbound event identity and delivery:** one durable event ID identifies the business event across retries; delivery attempts have separate IDs. Events are emitted only after the authoritative state transition commits. Signing must cover a precisely specified representation of the timestamp and raw body; final algorithm, headers, timestamp tolerance and rotation rules need security review. Delivery is at-least-once unless an approved contract states otherwise; consumers deduplicate. Retry cadence, maximum age, timeout, disablement, alerting and manual replay authorization remain open.
7. **Replay and privacy:** manual replay is a privileged, audited action and preserves event identity/content under the approved contract. Never replay a financial operation merely by replaying a notification. Minimize event payload data and redact secrets, raw provider bodies and unnecessary personal information.
8. **Authority boundary:** DOC-49 is the proposed merchant-facing contract source; DOC-41 owns lifecycle semantics, DOC-42 ledger/reconciliation semantics, DOC-48 provider capability/evidence, DOC-43 security requirements and DOC-58 internal operation-contract planning. Where these disagree, do not silently choose a local behavior: record a contradiction and resolve it before implementation.

**Decisions still required:** exact route/schema catalogue, error-code mapping, cursor fields/limits, API and event compatibility policy, webhook signature test vectors/rotation, delivery retry numbers, replay authority, and requirement-level traceability. Product-owner approval does not substitute for technical/API, security, finance, operations or independent review. No tests are claimed as run.

## 15. Related-document traceability

- **DOC-31:** normative launch requirements and acceptance criteria; this document does not replace them.
- **DOC-34:** requirement-level traceability matrix; proposed tests here must later map to approved requirement IDs and actual evidence.
- **DOC-41:** canonical transaction lifecycle and provider mapping proposal.
- **DOC-43:** threat model and security controls requiring independent review.
- **DOC-45:** evidence classes and test evidence requirements.
- **DOC-46:** G2 gap closure and role-level ownership matrix.
- **DOC-48:** provider adapter contract and capability evidence boundary.
- **DOC-12/13/14:** existing planned topics for API, credential/IP controls and webhooks; reconcile these documents against this proposal before G1/G2 approval to avoid conflicting contracts.

## 16. Current disposition

DOC-49 proposes a merchant-facing API and developer integration contract for future review. It adopts a production-first onboarding direction: sandbox is optional and is not a universal prerequisite to requesting production access. Endpoint names, authentication, schemas, webhook behavior, rate limits and lifecycle details are not yet approved or implemented. No live API is claimed to exist. Provider authority, funds-flow, legal/compliance, finance, independent security and operational evidence remain separate hard gates.

**G0 remains IN PROGRESS. G1 is NOT PASSED. G2 is NOT APPROVED.** No tests are claimed as run. This document does not authorize provider contact, onboarding, implementation, merge, deployment or live financial activity.
