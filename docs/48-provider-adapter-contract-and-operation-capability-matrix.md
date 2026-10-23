# DOC-48 — Provider Adapter Contract and Operation Capability Matrix

**Status:** Draft architecture contract — not approved for implementation or live use  
**Date:** 2026-10-23  
**Related documents:** DOC-00, DOC-28, DOC-31, DOC-34, DOC-37, DOC-38, DOC-40–DOC-47, DOC-49  
**Purpose:** Define the provider-neutral boundary HARAI expects and record which MTN MoMo Uganda and Airtel Uganda capabilities are documented, observed, unknown or blocked.

> Public API documentation and read-only portal observations are not contractual authorization. This is a design proposal, not a provider contract or claim of runtime verification. No provider settings, credentials, API calls, applications or transactions were used to produce this document.

## 1. Decision summary

DOC-49 proposes the merchant-facing API contract that will call into the internal operation boundary described here. Its endpoint, authentication, idempotency and webhook proposals must be reconciled with this adapter contract during G1/G2 review; neither document authorizes implementation or provider dispatch.

1. HARAI should expose a provider-neutral operation contract to the core while preserving provider-native statuses, response codes, identifiers, callback payloads and evidence.
2. Capabilities must be recorded per provider, product/version, operation, merchant relationship, environment and currency—not as a provider-wide boolean.
3. Request acceptance, HTTP status, redirect, callback receipt or timeout is not universal proof of financial success or failure.
4. Unknown outcomes remain unresolved until authoritative enquiry, verified evidence or an explicitly authorized reconciliation decision resolves them.
5. The adapter reports provider facts. The core owns canonical state transitions, tenant authorization, fees, accounting, ledger posting and release policy.
6. Unsupported or unverified capabilities fail closed. A UI toggle or successful test request cannot grant permission.
7. All interfaces and tests below are proposed; they are not implemented code or executed tests.

## 2. Capability evidence vocabulary

| Value | Meaning | Permitted conclusion |
|---|---|---|
| DOCUMENTED | Public documentation describes an operation or behavior | Documentation exists; HARAI access and permission remain unproven |
| OBSERVED-TEST | Authenticated portal explicitly shows test access/subscription | Test access was observed only within the recorded scope |
| OBSERVED-ACCOUNT | Read-only account state was observed | Only the visible state at that time is established |
| WRITTEN-CONFIRMED | Current written provider terms confirm HARAI's role, product, environment and operation | May be considered subject to all other gates |
| UNKNOWN | Evidence is absent, incomplete, ambiguous or stale | Must not be assumed supported |
| BLOCKED | A prerequisite or approval is absent, or the feature is deliberately disabled | Do not expose or execute the capability |
| NOT-APPLICABLE | Operation does not apply to this specific product/flow | Record why; do not silently substitute another operation |

Every capability record needs its source, evidence class under DOC-45, capture/review date, product/version, environment, account/merchant scope, reviewer and limitations. DOCUMENTED is not equivalent to WRITTEN-CONFIRMED.

## 3. Proposed provider-neutral contract

### 3.1 Capability descriptor

Each adapter/product configuration must have a reviewed, versioned descriptor containing:

- Provider code, exact product/API family and version.
- Environment: test or production, explicit and never inferred from credentials alone.
- Allowed operation types: collection, status enquiry, refund, reversal/return, money-out, balance enquiry and reconciliation data as applicable.
- Merchant relationship model: direct merchant, sub-merchant, aggregator or other documented arrangement.
- Supported currencies and limits per operation and merchant relationship.
- Initiation-response meanings, native status values, finality and mapping-rule version.
- Callback authentication, key version, replay protection, delivery/retry behavior and registration prerequisites.
- Provider idempotency/reference scope and retention where documented; otherwise explicitly UNKNOWN.
- Status-enquiry support, identifiers, rate limits and authoritative fields.
- Fee, funding, settlement and reconciliation evidence references.
- Enabled flag, which remains false until permission and operational prerequisites are satisfied.
- Evidence references and the person/date approving the descriptor.

The adapter must not enable a capability just because an endpoint is public or test credentials work.

### 3.2 Common operation envelope

The core persists its operation before any external side effect. The proposed envelope includes:

- Durable HARAI operation ID.
- Tenant and merchant account derived from authenticated server-side context, not trusted from arbitrary request fields.
- Explicit operation type.
- Amount as a positive integer in minor units and an allowlisted currency; no binary floating-point money.
- Validated merchant reference.
- Idempotency key for money-moving merchant API operations, with centrally defined uniqueness and conflict behavior.
- Non-secret correlation ID.
- Opaque payer/beneficiary references only where applicable and authorized.
- Allowlisted, non-sensitive metadata only.

The adapter must not accept raw tenant identity or a merchant-supplied provider account identifier as authority to route funds.

### 3.3 Logical adapter methods

| Method | Purpose | Safety requirement |
|---|---|---|
| describe capabilities | Return approved descriptor for product/environment/merchant relationship | Must not infer or elevate permissions |
| initiate collection | Start a collection attempt | Persist attempt before dispatch; preserve provider identifiers and response evidence |
| initiate money-out | Start an approved payout/disbursement | Reject unless funding, beneficiary, permission, limits and approvals are satisfied |
| enquire status | Query provider status where supported | Return native status and evidence separately from canonical mapping |
| request refund | Request supported refund linked to original operation | Atomically enforce refundable amount and product capability |
| request reversal/return | Request distinct supported correction/return | Never alias to refund without documented semantic equivalence |
| verify callback | Authenticate provider callback and validate basic schema | Verify raw bytes where required; enforce context and replay controls |
| parse verified callback | Normalize a verified callback into a provider event | Parsing is not proof of finality; events must be deduplicable |
| fetch reconciliation data | Retrieve approved reports/statements where available | Bounded, auditable and read-only |
| map native status | Map native status for operation type and descriptor version | Deterministic and versioned; preserve native status and mapping version |

Unsupported operations return a typed unsupported-capability result before network dispatch. The adapter must not fabricate support with a generic fallback.

### 3.4 Normalized result

A method result should include, as applicable:

- Outcome: ACCEPTED, REJECTED, PENDING, UNKNOWN, UNSUPPORTED_CAPABILITY or CONFIGURATION_BLOCKED.
- Provider, product, environment, operation type, adapter version and capability-descriptor version.
- Provider attempt ID and native references.
- Native status and provider response/error code.
- Canonical-state candidate and mapping-rule version; the core validates transitions.
- Retry advice: SAFE_TO_RETRY, DO_NOT_RETRY or RECONCILE_FIRST only when supported by the provider contract. Ambiguity is not safe to retry.
- Documented retry timing if available, correlation ID, timestamps and secure evidence reference.
- Redacted diagnostics suitable for logs/support.

A bare success boolean is insufficient for a money-moving operation. The adapter must not write ledger entries or decide business authorization.

## 4. State and transition boundary

| Provider observation | Required core behavior |
|---|---|
| Request accepted or HTTP success | Map according to verified product contract; never automatic financial success |
| Provider says in progress | Keep pending and use allowed enquiry/reconciliation |
| Timeout after dispatch may have occurred | Mark UNKNOWN; reconcile before another money-moving submission unless written contract proves safe retry |
| Callback authentication fails | Reject as untrusted; no financial transition |
| Verified callback reports success | Validate context, event identity, amount/currency/reference and transition; apply idempotently |
| Duplicate/out-of-order callback | Deduplicate and retain evidence; no repeated side effects |
| Enquiry conflicts with callback | Preserve both facts and follow approved precedence or open a discrepancy; do not choose the favorable status silently |
| Expiry/failure reported while attempt may be in flight | Apply only verified provider-specific finality; otherwise reconcile |
| Late success after cancellation/expiry | Governed discrepancy/late-success path; no destructive overwrite or automatic blind refund |
| Refund or payout outcome is ambiguous | Maintain separate linked operation and reconcile; dispatch is not completion |

Provider-specific mapping requires evidence for the exact product and version. The adapter reports facts; the core validates transitions.


### 4.1 Correlation, tenant binding and evidence precedence

A callback must be correlated to a previously persisted provider attempt through a provider/product-specific identifier or a separately verified binding. Do not trust a tenant ID, merchant reference, amount, status or operation ID supplied only by the callback to establish ownership. Resolve the callback to the stored attempt, then validate the expected provider/product, environment, merchant relationship, amount/currency and allowed state transition. If correlation is ambiguous, preserve a sanitized receipt and route it for investigation; do not guess the tenant or apply a financial effect.

For each provider product and operation, the approved contract must state which evidence is authoritative at each phase (initiation, pending recovery, final status and settlement), whether status enquiry can supersede an earlier callback, how event timestamps/sequence are interpreted, and what to do when evidence conflicts. There is no global callback-over-enquiry or enquiry-over-callback rule. Preserve all authenticated source evidence with receipt time, provider-reported time, event/reference identifiers, mapping version and redaction metadata. Settlement data may be authoritative for settlement matching without necessarily being the authority for real-time customer-facing payment state.

Dispatch and cancellation must follow DOC-41 §5A: the adapter must expose whether a request was definitely not sent, was accepted with a provider reference, or may have been sent with an ambiguous outcome. If the adapter cannot prove “not sent,” the core must preserve UNKNOWN and recover using documented provider-safe methods. The adapter does not independently cancel an in-flight payment, issue an automatic refund, choose a tenant, or post to the ledger.

## 5. Operation capability matrix — evidence snapshot

The following reflects evidence recorded in DOC-37/DOC-38 and the provider review for 2026-10-19. It is not a live status check as of this document's date and must be refreshed before implementation decisions.

| Capability | MTN MoMo Uganda | Airtel Uganda | HARAI disposition |
|---|---|---|---|
| Collection initiation | Collections and Collection Widget subscriptions observed active in Test; Request-to-Pay documented | Collection API v2.0 documents initiation; dashboard showed 0 live and 3 archived applications | Design candidate only; live access, role and entitlement unconfirmed |
| Collection status enquiry | Documented; asynchronous request and recovery guidance | Collection API v2.0 documents transaction enquiry and ambiguous/in-progress outcomes | Recovery path required; exact identifiers, limits and finality need evidence |
| Collection callbacks | HTTPS/registered-host guidance; prior review indicates callback sent once without retry | Configurable callbacks; docs describe HMAC-SHA256 authentication | Provider-specific verification/delivery contract; do not assume same retry policy |
| Collection widget | Active test subscription observed | Equivalent capability not confirmed in reviewed evidence | MTN test observation only |
| Refund | Disbursement refund variants/status documented; generic collection reversal not identified in reviewed pages | Collection API v2.0 documents full refund | Exact flow, partial support, eligibility, timing, funding and accounting remain unconfirmed |
| Reversal, return, dispute | Exact capability and semantics not established for every flow | Reverse interops documentation exists; applicability to each product/merchant needs verification | Separate operation family; blocked until evidenced |
| Money-out/disbursement | Transfer/deposit and balance/status documentation exists; no Disbursement subscription visible in reviewed portal | Disbursement API v2.0 documents B2B/B2C/direct transfers; no production activation visible | Blocked pending written permission, funding, beneficiary, limits and settlement evidence |
| Balance enquiry | Documentation exists; no live call made | Account API documentation exists; no live call made | Not a substitute for ledger or settlement reconciliation |
| Settlement/reconciliation | Daily reconciliation recommended; actual schedule, reports and terms not established | Transactions Summary exposes transaction/service-charge fields; full settlement terms not established | Sources, cadence, fees and owner unresolved |
| Multi-merchant/aggregator role | Aggregator guidance documented; written HARAI permission not visible | Merchant Collection documents relationship IDs; written HARAI permission not visible | External hard blocker; public docs do not establish authorization |
| Production approval | No production approval/live subscription visible in reviewed portal | No production approval visible; dashboard showed 0 live and 3 archived applications | Blocked pending current evidence and written authorization |
| Fees, limits, funding, settlement | Complete commercial terms not visible in reviewed evidence | Complete fee/settlement/funding terms not visible in reviewed evidence | UNKNOWN; never hard-code guessed values |

“Not identified in reviewed pages” means the review did not establish the capability; it does not prove the provider lacks it.

## 6. Required behavior by operation family

### 6.1 Collections

- Create the HARAI operation and idempotency record before dispatch.
- Record each provider attempt separately, including dispatch certainty.
- Preserve native response/status and map only with a versioned rule for the exact product.
- On timeout after possible dispatch, use UNKNOWN and reconcile; do not resubmit merely because a caller retries.
- Authenticate callbacks and bind them to expected provider account/product/environment; match identifiers, amount and currency where available.
- Successful financial posting requires the approved funds-flow and accounting model and must occur idempotently.

### 6.2 Status enquiry and reconciliation

- Use the most authoritative supported enquiry and preserve source evidence under controlled access.
- Respect provider limits and documented enquiry windows. If limits are unknown, do not invent a polling cadence.
- Record enquiries and evidence used to resolve state.
- An inconclusive enquiry is not success or failure.
- Reconciliation may open a discrepancy or request authorized review; it must not silently rewrite facts or post a balancing plug.

### 6.3 Refunds, reversals and returns

- Each is a separate linked operation with its own idempotency key, provider attempts, status, fees and audit trail.
- Validate remaining refundable/returnable amount atomically.
- Distinguish full/partial refund only where the exact product supports it.
- Accepted refund request is not completed refund.
- Do not treat reversal, return, dispute/chargeback and refund as synonyms without documented semantics and approved accounting mapping.

### 6.4 Money-out

Before enabling money-out, obtain written product permission and document source of funds, balance authority, prefunding/top-up if applicable, beneficiary verification/change controls, limits and approvals, returned payouts, provider idempotency, timeout recovery, fees, settlement, accounting, reconciliation and incident ownership. Refuse dispatch when a required prerequisite is missing or stale. A provider balance response does not authorize a payout.

### 6.5 HARAI outbound merchant webhooks

These are distinct from inbound provider callbacks. Proposed design: durable outbox, signed payloads, stable event IDs, documented bounded retries/backoff, replay controls and per-tenant destination validation. Delivery success only establishes transport-level success; consumers must deduplicate. This is not an implemented delivery guarantee.

## 7. Error taxonomy and retry policy

| Internal category | Meaning | Default handling |
|---|---|---|
| VALIDATION_FAILED | Invalid amount, currency, reference or schema | Do not dispatch |
| AUTHENTICATION_FAILED | Invalid credentials or callback authentication | No blind retry; use security/credential incident path |
| PERMISSION_BLOCKED | Product, merchant or environment permission not established | Fail closed |
| UNSUPPORTED_CAPABILITY | Descriptor does not support operation | Do not dispatch |
| LIMIT_OR_FUNDS_REJECTED | Explicit provider limit/insufficient-funds rejection | No automatic repeat; reconcile and apply approved policy |
| DUPLICATE_REFERENCE | Provider duplicate or HARAI idempotency conflict | Resolve existing operation; no second effect |
| DEFINITIVE_PROVIDER_REJECTION | Contractually definitive rejection | Apply versioned product mapping |
| TRANSIENT_PRE_DISPATCH_FAILURE | Evidence proves request was not dispatched | Retry only under approved bounded policy |
| TIMEOUT_AFTER_POSSIBLE_DISPATCH | Operation may have occurred | UNKNOWN; reconcile first |
| RATE_LIMITED | Provider/API rate limit | Follow documented guidance; otherwise controlled backoff and alert |
| MALFORMED_OR_UNVERIFIED_CALLBACK | Failed authentication, schema or context validation | No state transition; retain redacted evidence |
| PROVIDER_CONTRACT_MISMATCH | Unknown status/schema or unexpected behavior | Quarantine result, alert and preserve evidence; do not guess |

Retry decisions must consider operation type, dispatch certainty, provider contract, idempotency support, current state and policy version. An adapter must not independently retry an ambiguous money-moving operation.

## 8. Security and data handling

- Store credentials in approved secret management, scoped per product and environment; never in merchant records, logs, fixtures or repository examples.
- Verify signatures against exact raw request bytes where required; enforce documented timestamp/replay controls and key rotation. Do not invent provider signature schemes.
- Use verified HTTPS/TLS; validate outbound destinations against SSRF and unsafe redirects.
- Never route a callback to a tenant based solely on unverified payload fields. Resolve provider account/product and operation references through trusted configuration and persisted records.
- Restrict access to raw payloads and provider responses; redact secrets and minimize personal data.
- Strictly validate provider status strings and callback fields. Preserve unknown values for review without unsafe transitions.
- Independent security review and negative tests are required before production; this document is not that review.

## 9. Adapter conformance test plan

The following IDs are planning references only. No tests are claimed to exist or have run.

| Test ID | Scenario | Expected property |
|---|---|---|
| ADAPTER-001 | Unsupported operation submitted | Rejected before external dispatch |
| ADAPTER-002 | Same idempotency key submitted concurrently | One logical operation; no duplicate effect |
| ADAPTER-003 | Timeout after possible dispatch | UNKNOWN; no blind resubmission |
| ADAPTER-004 | Unknown native status | Preserve native value; do not guess terminal state |
| ADAPTER-005 | Duplicate callback | One logical effect and no duplicate posting |
| ADAPTER-006 | Invalid signature/account context | No state or ledger change |
| ADAPTER-007 | Callback arrives before initiation response persistence | Safely correlate or quarantine without loss/double-post |
| ADAPTER-008 | Late success after cancellation/expiry | Governed exception; no destructive overwrite |
| ADAPTER-009 | Enquiry and callback disagree | Preserve both; approved precedence or discrepancy |
| ADAPTER-010 | Provider reference collision across tenants | No cross-tenant disclosure or association |
| ADAPTER-011 | Concurrent refund boundary checks | Atomic amount bounds and distinct linked operations |
| ADAPTER-012 | Money-out with missing/stale permission or beneficiary approval | Fail closed before dispatch |
| ADAPTER-013 | Rate limit or provider outage | Bounded documented backoff; no request storm |
| ADAPTER-014 | Provider status/schema changes | Version mismatch detected and safely quarantined |
| ADAPTER-015 | Worker crashes after dispatch before response persistence | Reconcile existing attempt; no blind resubmission |
| ADAPTER-016 | Reconciliation import replay | Deduplicate and avoid duplicate ledger effects |
| ADAPTER-017 | Merchant webhook retry | Stable event ID/signature and bounded replay |
| ADAPTER-018 | Product/environment descriptor mismatch | Dispatch blocked; no test/production mixing |

Executed tests must link commit/build, environment, command/run ID, result, timestamp and evidence artifact under DOC-45. A table row is not test evidence.

## 10. Evidence and decisions required before implementation

1. Written permission for HARAI's precise multi-merchant/gateway role, product, environment and permitted money flows.
2. Current product/version contracts for statuses/finality, callback authentication/delivery, idempotency/reference rules, enquiry behavior, rate limits and refunds/returns.
3. Per-provider capability descriptors reviewed by a named accountable integration owner.
4. Funds-flow decision and qualified finance/accounting review of postings, refunds, fees, settlement, payouts and discrepancies.
5. Qualified Uganda legal/compliance review of the operating model and applicable obligations.
6. Named independent security reviewer and approved tenant, callback, secrets, data-retention and edge-trust designs.
7. Approved operational targets, reconciliation sources/ownership, incident handling and recovery procedures.
8. G1/G2 criteria met with evidence; separate explicit authorization before implementation.
9. Future adapter conformance tests executed and independently reviewed before production capability is enabled.

## 11. Open decisions

| ID | Question | Closure evidence | Status |
|---|---|---|---|
| AD-01 | Which provider products and merchant relationship may HARAI use? | Written permission and qualified legal/compliance review | Open — external blocker |
| AD-02 | Which funds-flow model applies and who holds/settles funds? | Product decision plus qualified finance/legal review | Open |
| AD-03 | Which statuses are final and what evidence takes precedence? | Versioned product contract matrix | Open |
| AD-04 | What idempotency guarantees and reference-retention periods apply? | Provider contract plus approved HARAI policy | Open |
| AD-05 | Which refunds, returns, disputes and money-out operations are authorized? | Capability matrix, written permission and accounting treatment | Open |
| AD-06 | What polling, callback retry, webhook retry and reconciliation cadence is safe? | Published limits/contracts plus operations approval | Open |
| AD-07 | What does each callback authenticate and how are keys registered/rotated? | Product-specific documentation and secure account evidence | Open |
| AD-08 | Which settlement sources are authoritative and who owns discrepancies? | Provider/contract evidence and named reconciliation owner | Open |
| AD-09 | Which card processor/acquirer and card-data boundary apply? | Processor selection, written terms and security review | Open — card capability blocked |
| AD-10 | How often must capability evidence be refreshed? | Named integration owner and approved evidence policy | Open |

## 12. Current disposition

DOC-48 defines a proposed adapter boundary and an evidence-aware capability matrix. It does not certify provider availability or permission and does not select the funds-flow model. Provider-dependent live capabilities remain blocked until the relevant written authority, contract, security, finance, legal/compliance and operational prerequisites are satisfied.

**G0 remains in progress. G1 is NOT PASSED. G2 is NOT APPROVED.** No provider contact, onboarding, credentials, API calls, payment-engine implementation, merge, deployment or live financial activity was performed or authorized by this document.
