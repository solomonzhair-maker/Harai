# DOC-42 — Ledger and Reconciliation Design Proposal

**Status:** Draft proposal — finance/reconciliation review required; not an approved accounting model  
**Date:** 2026-10-19  
**Related:** DOC-28, DOC-31, DOC-34, DOC-38, DOC-40, DOC-41, DOC-43  
**Purpose:** Define financial-record invariants and reconciliation workflows independently of the still-unselected funds-flow model. The ledger is an internal record of observed events; it does not decide legal ownership/custody of funds.

## 1. Core design decision proposed

Use immutable, balanced postings for financial events, with operation records and reconciliation evidence kept separate. A posted entry is never edited or deleted to make totals appear correct. Corrections use linked compensating entries with reason, actor, approval where required, and evidence.

This is a proposed accounting representation, not a claim that HARAI may hold customer balances or control funds. The final chart of accounts and legal/accounting treatment depend on the selected model and qualified professional review.

## 2. Separate records and responsibilities

| Record | Purpose | Mutability rule |
|---|---|---|
| PaymentIntent / MoneyOutOperation | Business intent and lifecycle | State changes only through audited transition service |
| ProviderAttempt | Each submission/status enquiry with provider reference and mapping version | Append attempts/events; do not reuse identifiers |
| FinancialEvent | Normalized source event (success, fee, refund, settlement, return, adjustment) | Immutable source facts; corrections are linked |
| LedgerTransaction | Atomic group of entries representing one financial effect | Immutable after posting |
| LedgerEntry | One debit/credit line with account, currency and amount | Immutable after posting |
| FeeScheduleVersion | Approved fee configuration with effective dates and approval | New version for changes; never rewrite history |
| SettlementReference | Provider/partner settlement batch and destination evidence | Preserve source and import identity |
| ReconciliationRun / Item | Comparison of internal records to provider/settlement evidence | Append runs and findings; preserve input-file hashes/metadata |
| DiscrepancyCase | Missing, duplicate, mismatched or late financial evidence | Status changes audited; closure requires reason and evidence |
| CorrectionCase | Approved resolution path for a financial record error | Separate approval and compensating posting |
| AuditEvent | Actor, action, reason, before/after reference and correlation ID | Append-only with restricted access |

Do not store full card PAN, CVV, provider secrets or unnecessary personal information in financial event descriptions or reconciliation files. Where source documents contain sensitive data, store only a protected reference and minimum necessary metadata.

## 3. Financial representation rules

1. Store monetary amounts as integer minor units with an explicit currency/precision registry, or use a separately approved exact-decimal representation. Never use binary floating-point for money.
2. Every ledger transaction balances within a single currency unless an explicitly approved foreign-exchange transaction model is defined; never silently net unlike currencies.
3. Each posting has a unique stable ID and references its business operation, source event, currency, amount, effective/observed times and correlation identifiers.
4. Enforce uniqueness and idempotency at the database boundary. Duplicate callbacks, import replays, worker retries or manual reprocessing must not post the same financial effect twice.
5. Do not post a collection success merely because a request was accepted or a merchant-facing webhook was delivered. Use the finality rule approved for that provider product.
6. Record gross amount, fee, tax, net settlement and fee payer as distinct values where applicable and supported by approved commercial terms. No assumed fee rate or tax treatment.
7. Partial refunds are bounded by the original operation's remaining eligible refundable amount; concurrent refunds require atomic reservation/checking so total approved refunds cannot exceed the permitted amount.
8. A reversal, refund, chargeback, payout return or manual adjustment is a distinct linked event; original postings remain intact.
9. A manual correction requires a discrepancy/correction case, reason, evidence, initiator and approver where policy requires separation of duties.
10. Ledger postings and reconciliation status are separate: marking an item reconciled must not mutate the original payment result or ledger entry.

## 4. Illustrative posting patterns — conceptual only

These patterns show invariants, not a final chart of accounts or an assumption that HARAI holds funds.

| Event | Conceptual record | Required evidence | Important caveat |
|---|---|---|---|
| Collection success | Balanced posting group for the observed collection obligation/event | Authoritative provider evidence under approved finality rule | Accounts depend on funds-flow and accounting model |
| Provider fee | Separate fee record/posting where the fee is known and contractually supported | Effective-dated approved fee schedule or provider statement | Do not infer fee from a public pricing page or estimate |
| Settlement | Link provider/partner settlement batch to relevant operations and settlement destination | Provider settlement report, bank/partner evidence where applicable | Settlement is not inferred solely from collection success |
| Refund | New linked financial event and posting group | Provider-confirmed refund status and original transaction link | Refund request acceptance is not necessarily refund completion |
| Reversal/return/chargeback | New linked event with provider evidence and case reference | Authoritative provider/bank/partner evidence | Do not erase the original transaction |
| Correction | Compensating entry linked to original posting and correction case | Documented discrepancy, evidence and approval | No direct mutation of posted entry |

A double-entry implementation needs a finance reviewer to approve account semantics and posting examples for each supported funds-flow model before implementation. If the selected model does not make HARAI the owner of particular funds, the ledger must represent only the obligations and events appropriate to HARAI's actual role.

## 5. Reconciliation layers

### Layer A — Operation-to-provider transaction
Match HARAI operation ID, provider reference, merchant reference, amount, currency, status and timestamps against provider transaction evidence. Preserve unmatched and ambiguous records rather than dropping them.

### Layer B — Provider transaction-to-settlement
Where settlement reports are available, map transaction-level gross/fee/net amounts to settlement batches, settlement destination and value date. Do not assume a provider API's transaction summary is a complete settlement statement.

### Layer C — Internal ledger-to-obligation
Verify that each accepted financial event has exactly the expected posting set and that every posting has a source operation/event. Check balance and uniqueness invariants.

### Layer D — Fees and net amounts
Recalculate only using approved effective-dated fee schedules and documented rounding/tax rules; compare to provider evidence. Differences become discrepancies, not silent adjustments.

### Layer E — Exceptions and case resolution
Create cases for missing provider reference, missing settlement, duplicate, amount/currency mismatch, unexpected fee, stale pending/unknown state, refund mismatch, duplicate posting, or provider/internal status conflict. Assign an owner and due time only after a real accountable owner and service target are appointed.

## 6. Matching and idempotency rules

- Prefer stable provider transaction IDs and settlement line IDs; merchant references alone may be truncated, non-unique or merchant-controlled.
- A match key is not a substitute for verifying amount, currency, direction, operation type and relevant timestamps.
- Store source report metadata and integrity hash; do not keep unnecessary raw personal data.
- Reimport of the same source artifact is idempotent; altered source content under the same source identity must be flagged.
- Never auto-match on amount alone.
- Use explicit match states: MATCHED, PARTIAL_MATCH, UNMATCHED_INTERNAL, UNMATCHED_PROVIDER, AMOUNT_MISMATCH, CURRENCY_MISMATCH, DUPLICATE, STATUS_CONFLICT, SETTLEMENT_MISSING and NEEDS_REVIEW.
- Auto-resolution is allowed only for deterministic, pre-approved rules. Any material financial correction or unresolved conflicting evidence requires an accountable case workflow.

## 7. Proposed data constraints

- Proposed idempotency scope: tenant + environment + operation class + idempotency key, with endpoint compatibility rules documented explicitly. Treat actor/credential as audit attributes, not automatically as uniqueness boundaries: otherwise the same tenant operation retried through a rotated or alternate credential could bypass deduplication. The final scope, key-retention window, request-fingerprint rules, conflict response and restore/replay horizon remain architecture decisions and must be consistent with DOC-49 and provider-specific idempotency limits.
- Unique provider event receipt key scoped by provider/account/product/environment where provider event identity is available; otherwise use a versioned, documented deduplication fingerprint over authenticated source identity and normalized immutable event fields. Potential fingerprint collisions or missing correlation go to review, not silent suppression.
- Unique ledger posting identity per normalized financial source event + posting purpose, enforced at the database boundary. A retry, callback, reconciliation import, compensation worker or restore replay must resolve to the same posting identity when it represents the same intended effect.
- Check constraints: positive/non-negative amount as appropriate per operation, supported currency/precision, allowed direction/type, balanced posting group and valid reference links.
- Database transactions atomically persist the financial event, posting group, outbox event and idempotency result where they form one business commit. Where external systems prevent one transaction, use durable outbox/inbox records and explicit recovery states; do not claim distributed atomicity.
- Do not use Redis/cache as the authoritative source for idempotency or financial posting uniqueness.
- Schema changes require reviewed migrations, backfill plan, rollback/forward-fix strategy and reconciliation checks.

### 7.1 Cross-layer identity and replay contract — proposed, not approved

The following is a consolidated proposal to reconcile with DOC-41, DOC-48 and DOC-49; it is not an approved architecture decision or evidence of implementation.

- Merchant idempotency is scoped to authenticated tenant/merchant + environment + operation class + key, with an explicit endpoint-compatibility class. Actor/credential remains audit context, not a uniqueness boundary by default.
- Canonicalize and fingerprint the semantically relevant request fields using a versioned algorithm. Same key + equivalent canonical request returns the existing logical operation; same key + different canonical request returns a stable conflict and cannot dispatch.
- Reserve the key atomically before any external side effect. Database uniqueness is authoritative under concurrency; cache/queue locks alone are insufficient.
- Keep logical operation IDs, provider-attempt IDs, provider-native idempotency keys/references, callback receipt IDs, ledger posting IDs, reconciliation source identities and outbound event IDs distinct but linked. Apply uniqueness at each boundary using the strongest documented stable identity available.
- If provider event identity is unavailable, use a versioned collision-aware fingerprint. If report/source identity repeats with identical content, treat it as a no-op; if the same identity has changed content, preserve both hashes and open a discrepancy.
- Postings are unique per normalized source event and posting purpose. Refunds/returns and payouts are separate linked operations, not mutation or reuse of the original collection's key/posting identity.
- Retention periods, tombstone lifetime, provider-reference expiry, client retry horizon and post-restore replay horizon must be decided together. Do not expire the only uniqueness evidence while a retry, provider callback, settlement report or restore could legitimately reintroduce the same operation.
- After restore, pause consequential dispatch/replay until durable operation, provider-attempt, callback receipt, ledger, outbox and reconciliation identities are checked and unresolved records reconciled. Do not infer that a missing queue item means a provider request was never sent.
- Stable outbound event IDs survive retry/replay. Transport retries must not trigger a second financial effect.

**Open decisions:** canonicalization details/version migration; endpoint compatibility rules; exact retention/tombstone periods; provider-native reference scope and retention per rail; callback fingerprint fields and collision handling; import identity/hash policy; ledger source/posting-purpose schema; restore acceptance evidence and resume authority. AR-005 remains OPEN until accountable owners approve the complete contract and authorized verification later demonstrates the required behavior.



### 7.2 Product-owner-approved internal defaults and outstanding owner decisions (2026-10-24)

The product owner approved the internal design defaults in this section on 2026-10-24. This approval establishes the intended HARAI design direction for subsequent technical review; it does not substitute for technical architecture approval, finance/accounting decisions, provider evidence, legal/compliance review, independent review, implementation, or verification. It does not authorize implementation.

**Approved internal design defaults**

1. **Merchant request identity.** The uniqueness boundary is authenticated tenant/merchant + environment + operation class + idempotency key. Actor and credential IDs are audit context, not scope dimensions by default. A key reservation belongs to one logical operation and is committed before any provider side effect.
2. **Operation classes and endpoint compatibility.** Collection creation, refund/return, reversal/void, and money-out are distinct classes with distinct operation IDs and authorization. Routes may share a compatibility class only when they create the same kind of logical operation and use the same semantic request fields. A route or API-version change must not silently bypass an existing reservation. If compatibility cannot be established deterministically, return a conflict or require retrieval of the existing operation; do not dispatch.
3. **Request fingerprint.** Persist the fingerprint algorithm/version with the reservation. Canonicalize only validated, semantically material fields after applying documented defaults; include operation class, compatible resource/route class, amount, currency, payer/beneficiary identity references and other fields that change the intended financial effect. Exclude secrets, authentication material, request IDs, transport headers and volatile timestamps. Normalize representation only where the API contract says values are semantically equivalent. A same-key request is a replay only if its canonical semantic request matches under the stored version; otherwise return stable `409 IDEMPOTENCY_CONFLICT` and do not dispatch.
4. **Version migration.** Never reinterpret an existing reservation solely using a newly deployed canonicalizer. Store canonicalization version and canonical digest (and, where needed for safe migration, a protected canonical-field representation). A new version must define how old reservations are compared, how long dual-read compatibility is needed, and how rollback works before rollout. If comparison is uncertain, fail closed and route to retrieval/review rather than creating a new operation.
5. **Concurrency and provider attempts.** Database uniqueness plus a transaction/atomic reservation is authoritative. Concurrent losers return the durable winner. A retry after possible dispatch reuses the same logical operation and follows provider-specific safe-retry evidence; it does not automatically create a fresh provider attempt. Provider-native idempotency keys and references are separate identifiers and must be stable across safe retries when the provider contract supports that behavior.
6. **Inbound event identity.** Prefer the provider's documented event ID, scoped by provider account/product/environment and any documented namespace. Without a reliable event ID, use a versioned fingerprint over authenticated source identity, the linked provider attempt/reference and normalized immutable event facts. If correlation is missing, two events could plausibly be distinct, or the fingerprint collides with conflicting facts, retain both evidence records and open review; do not silently suppress one.
7. **Ledger identity.** Enforce uniqueness for the normalized source event + posting purpose + relevant ledger/accounting domain. The source identity must be stable across callback, enquiry, import, worker retry and replay paths. Corrections, fees, refunds, returns and reversals use distinct linked purposes/operations; do not overwrite the original posting or rely on amount/time alone.
8. **Reconciliation import identity.** Prefer a provider-defined report/file ID and scope it by provider account/product/environment/report type/period where those dimensions are available. Record a content hash. Same source identity + same content is a no-op; same source identity + changed content is a discrepancy with both versions preserved. If no provider source ID exists, the import owner must approve a deterministic composite identity before automation; filename, amount or timestamp alone is insufficient.
9. **Outbound event identity.** Assign one durable event ID to the business event and preserve it across delivery retries and replay. Keep delivery-attempt IDs separate. A replay envelope may change only under an explicit versioned contract and must still point to the original event; transport delivery must never repeat a financial posting.
10. **Retention and tombstones.** Do not set an arbitrary universal duration in this draft. The architecture owner must approve a coordinated retention schedule that covers the longest supported merchant retry, provider idempotency/reference and callback replay, reconciliation re-import, dispute/correction and backup-restore replay horizons, plus any applicable legal/accounting retention. If full records must be purged, retain a minimal protected tombstone/hash only where lawful and sufficient to prevent duplicate effects. If a safe horizon cannot be established for a capability, that capability remains blocked or requires a controlled manual recovery path.
11. **Restore and replay.** After restore, pause consequential dispatch, callback reprocessing, import replay and outbound replay. Validate operation reservations, attempts and provider references, event receipts, postings, outbox events, import identities, ledger balance/integrity and unresolved UNKNOWN cases. Only an explicitly authorized Operations owner, with Technical owner verification and Finance/reconciliation approval for financial integrity, may resume consequential work under a recorded restore checklist. A missing queue item is not proof that dispatch did not occur.

**Exact decisions and evidence still required**

| Decision | Accountable role | Exact question to resolve | Blocker if unresolved |
|---|---|---|---|
| API compatibility classes and canonical fields | Product/API owner + technical architecture owner | Which exact endpoint/API-version pairs create the same operation class, and what validated fields alter the financial meaning? | Cross-endpoint retry cannot be safely classified; money-moving API stays gated |
| Canonicalization version migration | Technical architecture owner | What versioned canonical representation, storage fields, dual-read window and rollback rule preserve existing reservations across releases? | No safe schema/API rollout |
| Merchant retry horizon | Product/API owner + operations owner | What retry horizon is promised to integrators, including response-loss and support-recovery behavior? | Cannot set reservation/tombstone lifetime |
| Provider keys, references and events | Provider integration owner | For each enabled provider/product/API version/operation/environment, what are the documented uniqueness scope, retry semantics, event identity and retention limits? | Affected adapter remains disabled for unsafe retry/automatic deduplication |
| Ledger source and posting purpose | Finance/accounting reviewer | What immutable source identity and posting-purpose taxonomy apply to collection, fee, refund, return, reversal, payout and correction? | No live posting or automated correction |
| Reconciliation source identity | Reconciliation/operations owner + finance reviewer | Which reports/feeds are authoritative, what stable IDs exist, and how are amended files/versioned periods represented? | Import is manual/review-only; no automatic posting from ambiguous imports |
| Retention and tombstones | Technical owner + privacy/compliance adviser + finance reviewer | What periods and minimal retained identifiers are lawful and sufficient across retry, provider, accounting, dispute and restore horizons? | Do not expire uniqueness evidence or enable affected automated replay |
| Restore resume authority | Operations owner + technical owner + finance reviewer | Who is the named authorizer and what evidence checklist is mandatory before each class of consequential work resumes? | Consequential workers/replay stay paused after restore |

**Closure rule:** Product-owner approval records the intended internal design direction only. AR-005 and AR-021 remain OPEN until accountable technical/finance/operations/privacy roles are assigned, their outstanding decisions are recorded in DOC-00/DOC-46, the canonical contract is reconciled across DOC-40/41/42/48/49, provider-specific unknowns are evidenced or explicitly blocked, and separately authorized implementation and verification later provide evidence. Documentation consistency or product-owner approval alone is not closure.

## 8. Reconciliation cadence and operational controls

The provider documentation reviewed so far recommends daily reconciliation for MTN, while Airtel documentation provides transaction/service-charge fields but does not establish a complete settlement cadence in the inspected pages. This is evidence for designing a daily reconciliation capability, not enough to set a production SLA or assert that daily is contractually sufficient.

Proposed operational model:
- frequent status recovery for pending/unknown transactions within documented provider quotas;
- scheduled transaction-level reconciliation at a cadence approved for each provider/funds-flow model;
- settlement reconciliation when complete settlement evidence is available;
- daily exception review as a candidate control, subject to staffing and contract terms;
- periodic review of unmatched items and aging, with thresholds set by accountable operations/finance owners;
- controlled rerun, source import, correction and case closure, all auditable.

Exact cadence, age thresholds, report sources, settlement cutoffs, RTO/RPO and case SLAs remain unapproved.

## 9. Required metrics and alerts

Candidate metrics: pending/unknown count and age by provider/product; callback authentication failures; duplicate event rate; unmatched provider events; unmatched internal operations; settlement missing/mismatch amount; duplicate posting attempts; refund aging; payout/return exceptions; reconciliation-run freshness; import failure; case backlog and aging.

Alerts need accountable responders, thresholds, runbooks and escalation paths. Do not define production thresholds from guesses. Log correlation IDs and record IDs, not secrets, full credentials or unnecessary customer details.

## 10. Minimum acceptance scenarios (proposed, not existing tests)

| Test ID | Scenario | Expected invariant |
|---|---|---|
| LEDGER-001 | Every financial posting group | Balanced within currency and valid account semantics |
| LEDGER-002 | Duplicate event/import/worker retry | No duplicate posting |
| LEDGER-003 | Two concurrent partial refunds | Total approved refund remains within allowed refundable balance |
| LEDGER-004 | Manual correction | Original remains immutable; compensating entry links to approved case |
| RECON-001 | Provider success missing internally | Owned discrepancy created; no invented success |
| RECON-002 | Internal success absent from provider evidence | Hold/flag for review; no silent rewrite |
| RECON-003 | Amount/currency/fee mismatch | Explicit discrepancy state and evidence retained |
| RECON-004 | Settlement line missing or duplicated | Settlement discrepancy, not auto-adjustment |
| RECON-005 | Re-import same report | Idempotent result; source identity and hash checked |
| RECON-006 | Cross-tenant reconciliation access | Deny access and avoid cross-tenant data leakage |
| RECON-007 | Unknown/pending aging | Escalation/case at approved threshold once thresholds are set |
| RECON-008 | Correction and case closure | Required evidence, reason, authorization and audit chain preserved |

These IDs are proposed planning references, not existing or executed tests.

## 11. Decisions required before implementation

- Qualified finance/accounting reviewer approves the ledger model, account semantics, fees/tax/rounding treatment and illustrative postings.
- Product owner and qualified reviewers approve the selected funds-flow model and the obligations HARAI actually records.
- Provider/partner supplies sample transaction and settlement reports, stable references, fees, timing, return/refund behavior and discrepancy process.
- Named finance/reconciliation and operations owners are appointed; thresholds and case SLAs are set by accountable owners.
- Engineering approves database constraints, transaction boundaries, idempotency, migration and repair procedures.
- Independent review validates no silent balance changes, no duplicate effects, no cross-tenant leakage and safe recovery.

## 12. Current disposition

DOC-42 is a design proposal, not an approved accounting policy or production ledger specification. It does not select a funds-flow model, imply custody, confirm provider permission, authorize money movement, or claim implementation/tests. G1 and G2 remain open.


## 13. Money-out control and evidence sequence — proposed, not approved

This sequence is a control-design proposal for review against each provider/product and the eventual approved funds-flow model. It does not authorize a payout or assume HARAI owns or holds the funds.

1. **Resolve eligibility before creating a dispatchable operation.** Verify the tenant/merchant, environment, requested operation and currency against server-controlled capability configuration. Use the provider account/subscription status, configured credentials, onboarding records, applicable terms and documented/observed API capability as the normal evidence of what is enabled. Check the precise environment and operation scope where those sources distinguish them; seek explicit written confirmation when the scope is unclear, the provider requires it, or a material multi-merchant/contractual entitlement is not established. Legal/compliance approval, funding arrangement, applicable limits and operational readiness remain separate checks. If a required capability or safety prerequisite is missing, expired or contradictory, fail closed without provider dispatch.
2. **Resolve funding and balance authority.** Identify the legally and contractually valid funding source and the system that is authoritative for available funds. A provider balance/status endpoint is evidence only within its documented meaning and freshness; it is not by itself permission to pay or proof of funds availability at dispatch. If prefunding, reserve, hold or top-up is required, its ownership, lifecycle, expiry, release and reconciliation must be approved before implementation.
3. **Verify beneficiary and destination.** Bind the payout to a verified beneficiary record and destination version. Apply the approved step-up, dual-control, cooling-off and change-notification rules to sensitive creation/change actions. Re-evaluate a pending payout after a destination change; do not silently redirect an already-approved or in-flight operation. Keep raw identity/payment details out of logs and ordinary audit descriptions.
4. **Authorize the exact operation.** Evaluate amount, currency, purpose/category where required, velocity and aggregate limits, risk flags, required approvers and separation-of-duties constraints. Approval must bind to the immutable operation details and beneficiary version; material changes invalidate approval and require re-evaluation. Initiator/approver identities, decision, reason, policy version and evidence references are auditable. No approval may be inferred from a role label or provider response.
5. **Reserve idempotently and establish the dispatch boundary.** Atomically persist the money-out operation, canonical idempotency reservation, approval/funding references, attempt and outbox command before dispatch. Enforce uniqueness and concurrency at the database boundary. The worker dispatches only a currently eligible, durably claimed operation whose authorization and capability evidence remain valid under the approved freshness policy.
6. **Preserve dispatch certainty and recover ambiguity.** Record whether the request is proven not sent, accepted with a provider reference, or may have been sent with an ambiguous outcome. A timeout, crash or missing callback after possible dispatch becomes UNKNOWN; query/reconcile using the provider's documented safe method before any repeat submission. Never use a new client key as a shortcut around an unresolved prior attempt.
7. **Record outcome without confusing acceptance with completion.** Keep request acceptance, provider processing, final payout result, settlement and return as distinct evidence/status concepts. Apply only the provider/product-specific approved finality rule. A late success, conflicting callback/enquiry/settlement result, or return after apparent completion creates a linked exception/case and preserves the original evidence; it does not silently overwrite history or trigger an unapproved automatic second payout/refund.
8. **Post only the approved financial effect.** The accounting treatment must be explicitly approved for the selected funds-flow model and HARAI's legal role. Persist the normalized source event, balanced posting group (if applicable), idempotency result and outbox effect atomically where they form one business commit. Do not treat a requested/accepted payout as settled; do not fabricate customer or merchant balances from a provider's reported balance.
9. **Reconcile and resolve exceptions.** Match operation and provider references, direction, beneficiary reference, amount, currency, fees, final status, return and settlement evidence. Preserve source identity/hash and provenance. Missing, duplicate, stale, unmatched or conflicting records go to an owned discrepancy case; corrections require linked evidence, reason and approval under the approved policy. Case ownership and deadlines remain unset until accountable owners approve them.
10. **Disable safely when prerequisites fail.** Capability configuration must be fail-closed when permission, funding, beneficiary verification, approval, provider evidence or reconciliation readiness is absent or stale. Disablement prevents new dispatch but does not erase or suppress recovery/reconciliation for already-dispatched attempts.

### Money-out approval/evidence checklist

| Control | Minimum evidence required before the relevant gate can pass | Current disposition |
|---|---|---|
| Provider/product capability | Provider account/subscription state, configured credential scope, applicable terms and capability evidence; explicit written confirmation when required or ambiguity remains | Current entitlement evidence is incomplete for money-out; verify before enabling |
| Funds-flow and funding | Approved funds-flow diagram, source-of-funds and prefunding/reserve treatment, funding authority and failure handling | Open; model not selected |
| Balance authority | Provider/partner contract explaining balance semantics, freshness, holds and limits, or another approved authoritative source | Open |
| Beneficiary lifecycle | Verification standard, destination versioning, change protections, pending-payout behavior and audit requirements | Open |
| Authorization and limits | Approved limits/risk policy, approval matrix, separation of duties and exact-operation approval binding | Open |
| Idempotency and recovery | Canonical key/fingerprint/retention contract, provider references, ambiguous-outcome recovery and return handling | Open; AR-005 also remains open |
| Ledger and reconciliation | Finance-approved posting examples, fees, settlement/return matching, discrepancy ownership and repair controls | Open; G2-03 remains open |
| Operational readiness | Named owners, runbooks, alert thresholds, incident escalation, backup/restore and authorized verification evidence | Open |

No row in this table is a claim that evidence has been obtained. This proposed sequence must be reviewed by the accountable product, technical, provider-integration, finance/accounting, security, legal/compliance and operations reviewers before implementation authorization. The controls and their ordering may require provider-specific adaptation, but no adaptation may silently remove a required hard gate.
