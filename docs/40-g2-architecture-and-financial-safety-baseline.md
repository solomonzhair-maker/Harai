# DOC-40 — G2 Architecture and Financial-Safety Baseline (Proposal)

**Status:** Draft architecture proposal — not approved for implementation  
**Date:** 2026-10-19  
**Related documents:** DOC-00, DOC-02, DOC-03, DOC-28, DOC-30, DOC-31, DOC-34, DOC-38, DOC-39, DOC-41, DOC-42, DOC-43  
**Purpose:** Convert the current requirements baseline into an architecture that can be reviewed in a focused G2 decision. This proposal intentionally separates HARAI's internal orchestration and records from the external provider's money movement. It does not select a funds-flow model, authorize provider access, or authorize coding/payment-engine implementation.

## 1. Architecture principles

1. **Provider adapters isolate differences.** MTN MoMo, Airtel Money, and any later eligible card processor sit behind documented provider-specific adapters. No shared business rule may assume that provider callbacks, refunds, payout semantics, finality, or idempotency are identical.
2. **A request is not a payment result.** An accepted API request, redirect, timeout, callback receipt, or queued job is not proof that money moved. Final transaction status follows the approved state machine and provider evidence.
3. **Unknown is a first-class outcome.** If the outcome is ambiguous, HARAI records uncertainty and performs safe status enquiry/reconciliation; it must not blindly retry a potentially money-moving operation.
4. **Tenant identity is enforced server-side.** Tenant context is derived from authenticated membership/credential records, never trusted from an arbitrary request body. Every query, job, export, webhook and support operation enforces tenant scope.
5. **Financial records are append-only.** Correct errors with linked compensating entries and audited adjustments, not by silently editing or deleting financial history.
6. **External money flow remains a decision gate.** An internal ledger is a record of observed obligations and movements; its existence does not establish that HARAI is a custodian, settlement agent, bank, or holder of funds.
7. **Sensitive actions are least-privilege and auditable.** Payouts, refunds, beneficiary changes, credential rotation, manual corrections and merchant activation use explicit permissions and configurable approvals; independent approval is required where risk policy or law calls for it.
8. **No unsupported method is advertised or enabled.** Product, API, checkout and operations UI capability flags must agree with provider permission, country/currency eligibility and release evidence.

## 2. Proposed logical components

| Component | Responsibility | Safety boundary |
|---|---|---|
| Merchant/admin web application | Merchant setup, checkout configuration, transaction views, API keys, webhook configuration and support views | UI is not an authorization boundary; server checks every action |
| Public API edge | Authentication, tenant resolution, schema validation, rate limits, correlation IDs and stable errors | Trusted-proxy configuration is explicit; secrets and personal data are redacted from logs |
| Identity and access service | Users, sessions, tenant memberships, roles, scopes, step-up and approval records | Least privilege, revocation and separation-of-duties policy |
| Merchant eligibility service | Onboarding, verification status, restrictions, method eligibility and activation gates | Live activation blocked until evidence and approvals are present |
| Payment orchestration domain | Collection intents, provider attempts, canonical state transitions, idempotency and recovery | No direct client-controlled status mutation; unknown outcomes are preserved |
| Provider adapter boundary | Provider-specific request mapping, authentication, callbacks, enquiry, errors, refunds and money-out capabilities | Per-provider contract tests; no provider assumptions leak into shared domain logic |
| Event delivery service | Signed outbound webhooks, delivery attempts, retry scheduling, deduplication and replay controls | At-least-once delivery is expected; consumers must tolerate duplicates |
| Ledger and reconciliation domain | Immutable postings, fee calculations when approved, provider evidence matching, discrepancies and compensating entries | Double-entry or equivalent invariant requires independent finance review before approval |
| Money-out control domain | Beneficiary validation, payout initiation, funding/limit checks, approval, status enquiry and return handling | Disabled unless provider, legal, funds-flow and operational gates pass |
| Risk and case management | Limits, suspicious-activity flags, disputes/complaints, review queues and case history | Restricted evidence access and auditable decisions |
| Background workers | Provider status checks, webhook retries, reconciliation imports, notifications and recovery jobs | Jobs are idempotent, tenant-scoped, bounded and safe under redelivery |
| Operations and observability | Metrics, alerting, runbooks, incident records, backup/restore evidence and safe diagnostics | No secrets in logs; support access is time-limited and audited |

These are logical boundaries, not a commitment to deploy each component as a separate microservice. A modular monolith with isolated modules and worker processes is a candidate; the final deployment topology must be chosen after requirements, capacity and operational constraints are reviewed.

## 3. Canonical transaction lifecycle proposal

Provider-specific states must map to a shared internal lifecycle without erasing provider details. The following is a starting proposal for review, not a final provider contract.

| Internal state | Meaning | Permitted next outcomes (subject to provider flow) | Prohibited interpretation |
|---|---|---|---|
| CREATED | HARAI validated and recorded an intent | REQUIRES_ACTION, SUBMITTING, EXPIRED, CANCELLED | Not payable or successful merely because created |
| REQUIRES_ACTION | Payer or merchant action is needed | SUBMITTING, EXPIRED, CANCELLED, FAILED where justified | No success inference from a browser redirect |
| SUBMITTING | A provider operation may be in flight | PENDING, SUCCEEDED, FAILED, UNKNOWN | A timeout is not a failure |
| PENDING | Provider indicates processing or outcome not yet final | SUCCEEDED, FAILED, EXPIRED only with authoritative evidence, UNKNOWN | No premature success/failure |
| UNKNOWN | HARAI cannot safely establish the result yet | PENDING, SUCCEEDED, FAILED after enquiry/evidence; manual reconciliation case | No blind second money-moving attempt |
| SUCCEEDED | Provider-confirmed success under the method-specific finality rule | Refund/return/reversal recorded as separate linked operations if supported | Never silently rewritten to FAILED |
| FAILED | Authoritative failure under the method-specific rule | A new attempt only under approved idempotency/retry rules; later correction is a separate event if evidence changes | No retry solely because the client did not receive a response |
| EXPIRED / CANCELLED | No new provider submission should start for this intent | Existing in-flight attempt still reconciled if submission may already have occurred | Does not erase a provider operation already in flight |

Additional rules:
- Preserve provider status, provider reference, raw response classification (redacted), event time, receipt time and correlation identifiers for each attempt.
- Reject forbidden state transitions; record the rejected event for investigation.
- Duplicate and out-of-order callbacks are expected and must not create duplicate financial effects.
- A refund, reversal, return or chargeback is a separate linked financial operation with its own state and ledger effect, not a destructive edit of the original collection.
- Finality and timeout windows are provider-specific and remain blocked until documented and contractually applicable.

## 4. Idempotency and safe retries

For any API operation that can create or move money:
1. Require an idempotency key scoped to the authenticated tenant/merchant + environment + operation class, with endpoint-compatibility rules defined in one canonical contract. Treat actor/credential as audit attributes, not automatic uniqueness boundaries, unless an approved architecture decision documents a specific exception.
2. Persist a canonical request hash and the original result; the same key with the same request returns the original operation, while a conflicting payload returns a stable conflict error.
3. Enforce uniqueness atomically at the database boundary, not only in application memory.
4. Persist the intent and an outbox command in one database transaction before dispatching provider work.
5. Record provider request identifiers and any provider idempotency facility separately from HARAI's key.
6. If a provider call times out after dispatch may have occurred, transition to UNKNOWN and enquire/reconcile before any repeat submission.
7. Worker retries, queue redelivery and process restart must not create duplicate provider operations or duplicate ledger postings.

Provider idempotency support and retention windows are not assumed; adapter contracts must state what is known, unknown and safe for each method.

## 5. Callback and outbound webhook design

### Inbound provider callbacks
- Accept only configured HTTPS endpoints and the provider's documented authentication/signature mechanism.
- Verify authenticity before changing state; validate timestamp/replay windows when supported.
- Store a deduplicated receipt and correlation metadata before asynchronous processing.
- Acknowledge promptly only after durable receipt; handle duplicate and out-of-order deliveries idempotently.
- Never trust callback payload alone if the provider's documented flow requires a status enquiry or reconciliation.
- Use a scheduled recovery/enquiry path because callback delivery may be one-shot, delayed or unavailable.
- Do not log callback secrets, signatures that could enable replay, or unnecessary personal data.

### Outbound merchant webhooks
- Sign a canonical payload with a per-endpoint secret, version the signature scheme and allow controlled secret rotation.
- Deliver from a durable outbox; record attempt count, response class, timestamps and next retry.
- Expect at-least-once delivery; publish stable event IDs and require consumers to deduplicate.
- Use bounded exponential backoff, an operational dead-letter/replay path and a visible delivery history.
- Protect against SSRF: restrict destinations to approved public HTTPS endpoints, reject private/link-local/reserved targets, revalidate DNS/redirect behavior and enforce egress policy.
- A webhook delivery failure must not reverse the underlying payment state.

## 6. Ledger and reconciliation proposal

Subject to independent finance review, model financial effects as immutable, balanced postings or an equivalently controlled accounting representation. Each posting must reference the originating operation and carry currency, amount in integer minor units (or an explicitly approved exact decimal representation), posting time, source, and correlation IDs.

Required invariants:
- Every posted financial event is attributable to one canonical operation and has unique posting identity.
- Amounts and currency precision are validated against an explicit currency policy; floating-point arithmetic is prohibited for financial calculations.
- Duplicate callbacks, retries or imports cannot create duplicate postings.
- Fees are applied only from approved, effective-dated schedules; fee payer, tax, rounding and refunds are explicit.
- A correction is a compensating posting with reason, actor, approver where required, and links to the original entry.
- Reconciliation compares provider transaction evidence, settlement evidence where available, and HARAI's operation/ledger records; missing or conflicting records enter an owned discrepancy queue.
- No reconciliation process silently changes a transaction to make totals match.
- Daily reconciliation is a candidate operating rule, but exact frequency, source reports and settlement cadence must be confirmed per provider/model.

The ledger records HARAI's understanding of financial events. It does not by itself determine who legally owns or controls funds. That question is governed by the approved funds-flow model and qualified review.

## 7. Minimum logical data entities

These are conceptual entities; not an approved database schema.

- Tenant, User, Membership, Role, Permission, Session and Approval.
- MerchantProfile, VerificationCase, EvidenceReference, MerchantMethodEligibility and ActivationDecision.
- ApiCredentialMetadata, CredentialScope, IpAllowlistEntry and CredentialRotationEvent (never persist a retrievable plaintext secret).
- PaymentIntent, ProviderAttempt, ProviderEventReceipt, IdempotencyRecord and StateTransition.
- RefundOrReversalOperation linked to its original transaction and provider-specific capability.
- BeneficiaryReference and BeneficiaryChangeCase with restricted access and change history.
- LedgerAccount (only if supported by the selected accounting model), LedgerTransaction, LedgerEntry, FeeScheduleVersion and SettlementReference.
- ReconciliationRun, ReconciliationItem, DiscrepancyCase and ResolutionEvent.
- WebhookEndpoint, WebhookSecretVersionMetadata, OutboundEvent, DeliveryAttempt and ReplayRecord.
- SupportCase, DisputeCase, AuditEvent, RiskDecision and IncidentRecord.
- OutboxMessage, JobExecution and DeadLetterRecord for durable asynchronous work.

Use database constraints for uniqueness and referential integrity, explicit tenant scoping, encrypted sensitive fields where required, and migrations with forward/rollback strategy. Final entity definitions depend on the chosen funds-flow model and technology decision.

## 8. Security and tenancy controls

- Every tenant-owned record has an enforceable tenant boundary; privileged cross-tenant support is exceptional, time-limited, reason-coded and audited.
- Background jobs carry an explicit tenant/operation context and re-check authorization/state at execution time.
- Database constraints and negative tests supplement application-level authorization.
- Credentials are shown once where appropriate, stored as non-retrievable secrets or hashes according to their purpose, scoped, revocable and rotatable.
- IP allowlisting must use trusted network metadata and must include a secure lockout recovery path.
- Outbound callbacks/webhooks have SSRF and egress protections.
- Secrets and personal data are minimized in logs, traces, error messages, support exports and test fixtures.
- Security-sensitive configuration changes have before/after audit records and alerting.
- No private verification documents or live provider secrets are cached in offline PWA storage.
- Threat modelling must cover tenant isolation, IDOR, credential theft, replay, race conditions, provider spoofing, SSRF, insider access, data leakage and recovery abuse.

## 9. Deployment and reliability shape (candidate)

Candidate deployment shape: a modular API application, merchant/admin web application, background worker process, PostgreSQL as authoritative state store, durable queue/outbox pattern, and managed secret storage. Redis may be considered for non-authoritative caching, rate limiting or ephemeral coordination, but must not become the only record of idempotency, financial state or queued money-moving work.

Required design evidence before implementation approval:
- environment separation for local/test/sandbox/staging/production;
- database transaction and migration strategy;
- durable outbox and job recovery semantics;
- structured metrics for provider latency, pending/unknown age, callback verification failures, queue depth, webhook delivery and reconciliation discrepancies;
- backup schedule, restore exercises, RTO/RPO and incident escalation;
- deployment rollback/forward-fix and compatibility rules;
- access-control and secret-rotation procedures.

Numeric availability, latency, throughput, RTO, RPO and retention targets remain open until capacity, provider dependencies and operational ownership are assessed.

## 10. Funds-flow decision remains separate

DOC-28 must continue comparing:
1. **Provider-managed funds:** provider/eligible partner receives or controls funds under its approved model; HARAI provides permitted orchestration and records.
2. **Direct provider integration:** HARAI contracts for and integrates provider APIs within explicitly authorized roles and legal/contractual boundaries.
3. **Hybrid model:** only where each rail's responsibilities, accounts, settlement, reconciliation and legal treatment are separately documented.

No model is selected by this architecture proposal. Before choosing, obtain evidence for account ownership/control, payer collection path, source of payout funds, merchant settlement, fees/taxes, reserves, refunds/returns, settlement cadence, provider permission and qualified Ugandan legal/compliance assessment. If evidence is unavailable, mark the affected capability blocked rather than assuming the architecture can overcome it.

## 11. G2 review checklist

G2 should not pass until accountable reviewers have recorded decisions on:
- modular monolith versus service decomposition and candidate technology stack;
- canonical state machine and per-provider state mapping (DOC-41);
- tenant isolation and privileged support boundaries, including the threat register in DOC-43;
- idempotency scope, storage and provider-specific retry rules;
- ledger/accounting representation, reconciliation invariants and correction policy (DOC-42);
- inbound callback authentication, outbound webhook retries and SSRF/egress protection;
- money-out, beneficiary, refund/reversal and approval controls;
- data classification, retention, deletion and privacy boundaries;
- provider adapter contracts and the still-open funds-flow model;
- reliability targets, backup/restore, operational ownership and incident response;
- test strategy covering concurrency, failure, recovery, cross-tenant denial and reconciliation, using DOC-41–DOC-43 as proposed scenario sources.

A qualified independent security reviewer and finance/reconciliation reviewer must review their respective domains. The author's internal review is not a substitute for independent sign-off.

## 12. Supporting design artifacts

- DOC-41 expands the proposed state machine and maps current MTN/Airtel evidence to internal handling rules. Provider finality, retry guarantees and production capability remain unconfirmed.
- DOC-42 details immutable financial records, posting invariants, reconciliation layers and discrepancy handling. Account semantics require finance review and depend on the selected funds-flow model.
- DOC-43 records initial trust boundaries, 20 threat scenarios, proposed controls and required validation. It is not a penetration test or independent security sign-off.
- DOC-28 now presents Model A, Model B and conditional Model C side by side without scoring unknowns.

## 13. Current disposition

This proposal is ready for architecture review, not implementation. G1 remains not passed until the requirements baseline receives accountable disposition. G2 remains not approved until the design decisions and independent reviews above are recorded. No provider contact, onboarding, live credentials, real transactions, payment-engine coding, merge or deployment is authorized by DOC-40.
