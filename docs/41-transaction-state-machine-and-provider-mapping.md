# DOC-41 — Transaction State Machine and Provider Mapping

**Status:** Draft design proposal — provider-specific finality and transitions unapproved  
**Initial date:** 2026-10-19  
**Review update:** 2026-10-23  
**Related:** DOC-31, DOC-34, DOC-38, DOC-40, DOC-42, DOC-43  
**Purpose:** Turn the generic lifecycle in DOC-40 into a reviewable state model and explicitly distinguish observed provider documentation from HARAI's proposed internal states. This is not a provider contract or authorization to implement.

## 1. Non-negotiable rules

1. HARAI owns its internal operation state; provider status is preserved separately and mapped through a versioned adapter rule.
2. An API acceptance, HTTP 2xx, redirect, callback receipt, queue completion or timeout is not itself proof of payment success or failure.
3. Every potentially money-moving operation has one durable HARAI operation ID, an idempotency record and one or more separately recorded provider attempts.
4. Unknown/ambiguous outcomes remain unresolved until authoritative status enquiry, provider evidence or an owned reconciliation decision resolves them.
5. Duplicate, delayed and out-of-order events must not create duplicate operations or ledger effects.
6. Refunds, reversals, returns, disputes and chargebacks are separate linked operations; they do not erase or rewrite the original collection.
7. Provider-specific status strings and payload metadata are retained with appropriate redaction and retention controls. Mapping rules are versioned and auditable.
8. Only a reviewed transition service can change state. Browser clients, merchant webhooks and generic support tools cannot directly set a transaction to SUCCEEDED.

## 2. Proposed internal state vocabulary

| State | Meaning | Entry evidence | Allowed next state(s) | Prohibited shortcut |
|---|---|---|---|---|
| CREATED | Intent validated and durably recorded; no provider submission is known to have begun | Request validation, tenant authorization, idempotency and database commit | REQUIRES_ACTION, SUBMITTING, CANCELLED, EXPIRED | Do not represent as paid |
| REQUIRES_ACTION | Payer/merchant action is required before provider submission or completion | Explicit checkout/provider contract response | SUBMITTING, CANCELLED, EXPIRED, FAILED only if authoritative | A browser redirect alone cannot set success |
| SUBMITTING | Dispatch may be in progress or may have reached provider | Durable attempt/outbox record | PENDING, SUCCEEDED, FAILED, UNKNOWN | Do not assume a timeout means no request was received |
| PENDING | Provider indicates processing or the documented flow is not final | Authenticated callback, status enquiry or documented response mapping | SUCCEEDED, FAILED, UNKNOWN; EXPIRED only where provider confirms final expiry | Do not infer finality from elapsed time alone |
| UNKNOWN | Outcome cannot safely be established yet | Timeout after possible dispatch, ambiguous provider result, conflicting evidence | PENDING, SUCCEEDED, FAILED only after authoritative evidence; manual review case | No blind resubmission of a potentially money-moving operation |
| SUCCEEDED | Method-specific success finality criteria are satisfied | Authoritative provider evidence plus approved mapping rule | Separate refund/return/reversal/dispute operation; correction workflow only by linked evidence | Never silently overwrite with FAILED |
| FAILED | Method-specific final failure criteria are satisfied | Authoritative provider evidence plus approved mapping rule | New attempt only under approved retry/idempotency policy; correction case if later evidence conflicts | No retry solely because caller missed a response |
| CANCELLED | HARAI has prevented a new submission or cancelled before dispatch according to approved semantics | Audited cancellation decision and proof no in-flight operation remains, where required | Terminal for new submission; existing attempts still reconciled | Cancellation must not hide an already-submitted attempt |
| EXPIRED | Intent/session expiry has been enforced | Expiry policy and check of any in-flight attempt | Terminal for new submission; existing attempts still reconciled | Expiry does not prove a provider operation failed |

The table is a proposed internal vocabulary. Implementation must define whether intent state and provider-attempt state are separate aggregates. Preferred proposal: keep **PaymentIntent** and **ProviderAttempt** distinct so a failed attempt does not erase the lifecycle of the overall intent, and a new attempt cannot obscure an unresolved earlier attempt.

## 3. Separate operation families

| Family | Linkage rule | Independent lifecycle? | Financial effect rule |
|---|---|---|---|
| Collection | One intent may have multiple controlled attempts, but never overlapping unsafe attempts | Yes | Post only after approved success/finality rule |
| Refund | Must reference the original successful collection and a provider-supported refund capability | Yes | Separate linked debit/credit entries according to the approved ledger model; partial/full amounts bounded by remaining refundable amount |
| Reversal/return | Must reference the original operation and documented provider return/reversal event | Yes | Separate compensating financial event; never delete original postings |
| Payout/disbursement | Separate money-out operation with beneficiary, funding source, limits, approvals and provider reference | Yes | Do not initiate until all legal, provider, funds-flow and operational gates pass |
| Beneficiary change | Separate audited security-sensitive case, not a field overwrite | Yes | Pending payouts re-evaluated under approved rules; destination change cannot silently redirect an in-flight payout |
| Dispute/chargeback/complaint | Case linked to transaction and relevant evidence | Yes | Financial effect only on authoritative event and approved policy |

## 4. Provider evidence and adapter mapping — current baseline

This table distinguishes current documented/observed facts from internal behavior proposed for HARAI. It does not claim that HARAI has production access.

| Topic | MTN MoMo evidence recorded in DOC-38 | Airtel Uganda evidence recorded in DOC-38 | HARAI mapping implication |
|---|---|---|---|
| Collection lifecycle | Request-to-Pay is asynchronous; status enquiry is documented; callbacks are reported as one-shot without retry | Collection v2.0 documents initiation and transaction enquiry; documented states include success, failed, ambiguous, in-progress and expired | Model accepted/in-progress/ambiguous separately; use enquiry and reconciliation for recovery |
| Money-out | Disbursement docs describe Transfer/Deposit, status/balance and refund functions; no Disbursement subscription was visible in the inspected account | Disbursement v2.0 describes B2B/B2C and direct merchant transfers; errors include insufficient funds, limits, duplicate IDs and timeout/ambiguous outcomes | Money-out adapter remains capability-disabled until account/contract approval; ambiguous outcomes use enquiry/reconciliation, never blind retry |
| Refund/reversal | Disbursement refund endpoints documented; collection invoice cancellation visible; generic collection reversal not identified in inspected pages | Collection full refund and Merchant Collection refund documented; separate Reverse Interops documentation exists | Do not infer identical refund/reversal semantics; document method, direction, window, partial/full behavior and accounting effect per product |
| Callback security | HTTPS and registered hosts required in published guidance; callback may be sent once without retry | Callback configuration documented; authenticated callback variant uses HMAC-SHA256 | Implement only after provider-specific signature verification, replay handling and durable receipt rules are confirmed |
| Idempotency/finality | Provider-specific behavior and retention need written confirmation | Duplicate IDs documented as an error; exact idempotency guarantees/retention need confirmation | Keep HARAI idempotency separate from provider reference; never infer universal provider idempotency |
| Production/aggregation eligibility | Active Collections and Collection Widget subscriptions were visible in Test; no Disbursement subscription or visible production approval | Dashboard showed zero live applications and three archived; no production approval visible | All production capability flags remain off until written approval and evidence |

## 5. Event-to-state decision rules

- **Authenticated success event:** validate authenticity, deduplicate, persist receipt, validate provider reference and amount/currency where available, then apply the approved mapping. If the event conflicts with current evidence, hold for enquiry/reconciliation rather than force a transition.
- **Authenticated failure event:** only set FAILED if the provider contract identifies the event as final for that operation. Retry rules are operation- and provider-specific.
- **Ambiguous/in-progress response:** keep UNKNOWN or PENDING as appropriate; schedule bounded status enquiry with backoff and provider quota awareness.
- **Timeout after dispatch:** mark UNKNOWN and reconcile. Do not assume provider did not receive the request.
- **Duplicate event:** record duplicate receipt/metrics as appropriate; do not repeat transition side effects or ledger posting.
- **Out-of-order event:** compare provider event identity/time and current state; preserve the event, reject invalid regression or request authoritative status enquiry.
- **Contradictory evidence:** freeze automated financial side effects for that operation, open a discrepancy case and require accountable resolution.
- **Expiry/cancellation racing with dispatch:** serialize decisions at the operation boundary; if provider dispatch may have occurred, continue recovery and do not represent cancellation as proof of no payment.
- **Unknown outcome beyond target age:** alert and create/retain an owned case; the numeric target remains a product/operations decision.


## 5A. Transition guards, precedence and late-event handling — proposed canonical rules

The following rules are proposed to make the state table implementable without treating intent status as proof that every provider attempt has stopped. They remain design proposals until the accountable architecture decision is recorded and provider-specific evidence is available.

1. **Separate intent and attempt state.** Keep the customer-facing PaymentIntent lifecycle distinct from each ProviderAttempt lifecycle. An intent may be cancelled or expired for *new initiation* while a previously dispatched or possibly dispatched attempt remains PENDING/UNKNOWN and subject to recovery. Do not use one terminal intent flag to suppress reconciliation of an attempt.
2. **Establish a durable dispatch boundary.** Before network dispatch, atomically persist the operation, idempotency reservation and attempt/outbox record. A worker may dispatch only from an eligible, durably claimed attempt. If cancellation wins before the dispatch claim, mark the attempt as not dispatched and prevent sending. If dispatch may already have begun, record the race as uncertain and reconcile; do not assert that cancellation prevented payment.
3. **Serialize competing decisions.** State transitions must use an atomic compare-and-set/version guard or equivalent transactional serialization. Concurrent cancel, expiry, dispatch, callback, enquiry and retry decisions must not independently create multiple provider submissions or financial postings.
4. **Late success after cancel/expiry.** Preserve the intent's cancellation/expiry history and record the authoritative success against the provider attempt. Open a late-success/discrepancy case and notify the responsible operational path. Do not silently rewrite the intent history, discard the success, or issue an automatic refund unless a separately approved, provider-supported policy explicitly authorizes that action.
5. **Conflicting evidence.** Authenticate and persist each event before applying it. Deduplicate using provider event identity where available; otherwise use a documented collision-aware fingerprint. When callback, status enquiry and settlement evidence conflict, retain all source evidence, stop automatic financial side effects for the disputed operation, and route to an owned reconciliation case. No single source wins universally: precedence must be specified per provider product, operation and lifecycle phase.
6. **Terminal states are not erasers.** A later event must not destructively regress a final state. If later authoritative evidence proves the prior mapping was wrong or a later reversal/return occurred, record a new linked event/operation or correction case under the approved financial policy.
7. **Retry eligibility is attempt-specific.** A new provider attempt is permitted only when the prior attempt is authoritatively final/non-submitted or when the provider's documented idempotency/retry contract explicitly makes the retry safe. A timeout, worker crash, missing callback or merchant retry is not sufficient evidence.
8. **Tenant context survives asynchronous work.** Persist immutable tenant/merchant ownership on the operation and attempt; derive it at initial authorization and carry it through outbox messages, callbacks, enquiry, reconciliation, exports and support cases. A provider reference or callback field alone must never select a tenant without a verified binding to the stored attempt.

### 5B. Cross-layer deduplication and replay — proposed, not approved

- Treat the merchant idempotency reservation as the entry-point identity for one logical operation, not as a universal identifier for provider attempts, callbacks, ledger postings or webhooks. Persist explicit links among these identities.
- Same key + same canonical request resolves to the existing operation. Same key + materially different canonical request is a conflict and cannot cause dispatch. The canonical fingerprint rules and compatibility classes must be shared with DOC-49 and DOC-42.
- Use database-enforced uniqueness/transactional reservation for concurrent requests and posting effects. A queue lock, cache entry or worker-local state is not authoritative.
- Deduplicate provider events by provider-issued identity when available; otherwise use a versioned fingerprint and preserve ambiguous/colliding evidence for reconciliation. Out-of-order or duplicate receipt must not produce a second financial side effect.
- Restore/replay must preserve the original operation, attempt, event, posting, import and outbound-event identities. A missing queue record is not proof that dispatch did not occur. Pause consequential work until durable records are validated and outstanding UNKNOWN operations are reconciled under an approved restore procedure.

### Required scenario coverage (planned, not executed)

Extend the future state/contract test plan to cover: cancel before dispatch claim; cancel concurrent with claim; worker crash after durable claim but before/after network send; timeout followed by late success; expiry followed by successful provider enquiry; duplicate/out-of-order callback; callback conflicting with enquiry or settlement; credential rotation with same tenant idempotency key; cross-tenant callback/reference substitution; restore while attempts/jobs are pending; and one authorized manual correction with full audit history. Each scenario must assert both persisted state and whether any ledger posting or outbound merchant event is permitted.


## 6. Minimum acceptance scenarios (proposed IDs, not existing tests)

| Test ID | Scenario | Expected invariant |
|---|---|---|
| STATE-001 | Every defined allowed transition | Transition accepted only when its preconditions and evidence are met |
| STATE-002 | Every representative forbidden transition | No illegal transition or hidden state regression |
| STATE-003 | Timeout after provider dispatch | UNKNOWN, status enquiry/reconciliation; no blind second submission |
| STATE-004 | Duplicate callback / duplicate poll result | At most one state side effect and one financial posting per source event |
| STATE-005 | Out-of-order callback | Preserve evidence; do not regress terminal success to pending/failed |
| STATE-006 | Conflicting callback and enquiry | Discrepancy case; no automatic financial adjustment |
| STATE-007 | Cancellation/expiry racing with submission | No new unsafe attempt; in-flight operation remains reconciled |
| STATE-008 | Partial and full refund bounds | Refund cannot exceed approved remaining refundable amount |
| STATE-009 | Provider adapter mapping version change | Old event can be interpreted with recorded mapping version; audit trail preserved |
| STATE-010 | Cross-tenant transaction ID substitution | Access denied without disclosing whether another tenant's transaction exists |

These IDs are proposed test-planning references. They are not evidence that code or test artefacts exist.

## 7. Decisions still required

- Approve the internal state names and whether intent/attempt state are separate after accountable technical review.
- Obtain each provider product's authoritative finality, expiry, callback, status-enquiry and retry/idempotency contract.
- Define provider-specific timeouts, polling budgets, pending-age alert thresholds and escalation owners.
- Confirm partial refund, reversal, return, dispute and chargeback capabilities and financial effects.
- Define evidence required for manual resolution and who can approve corrections.
- Keep money-out disabled until its separate permission, funds-flow, funding, compliance and operations gates pass.

## 8. Current disposition

DOC-41 is a design proposal for review. It does not claim provider contract confirmation, implementation, executed tests, G1/G2 approval, or production eligibility. Provider unknowns remain explicitly unconfirmed.


## 9. Provider adapter contract cross-reference

DOC-48 defines the proposed provider-neutral adapter boundary, capability evidence vocabulary, normalized result, retry taxonomy and conformance-test plan. DOC-41 remains the canonical state-machine proposal; provider-specific finality and transition rules must be evidenced per product/version before implementation. DOC-48 does not replace provider authorization or G1/G2 approval.
