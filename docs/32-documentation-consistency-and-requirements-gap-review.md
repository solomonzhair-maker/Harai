# DOC-32 — Documentation Consistency and Requirements Gap Review

**Status:** Draft audit record — documentation review only  
**Reviewed baseline:** DOC-00, DOC-02, DOC-03, DOC-28, DOC-29, DOC-30, DOC-31, DOC-37 and DOC-38 on the documentation branch  
**Purpose:** Identify contradictions and missing decision/evidence links before the requirements baseline is presented for approval. This audit is not a legal opinion, provider confirmation, architecture approval or implementation authorization.

## 1. Executive result

The documentation is directionally consistent on the central product intent: Uganda first, multiple merchants over time, a comprehensive launch package, collections plus a planned money-out workstream, refunds/reversals where supported, and an unresolved comparison between provider-managed funds and direct provider integrations.

**Gate status remains G0 — discovery and requirements in progress.** The documents do not yet constitute an approved, fully traceable requirements baseline. Provider eligibility, the legal/compliance position, funds flow, account ownership, funding source for money-out, currencies, settlement expectations, commercial viability and accountable review owners remain unresolved.

## 2. Consistency findings

| ID | Finding | Status | Required treatment |
|---|---|---|---|
| GAP-001 | Money-out is stated as intended launch scope while provider support and legal permission remain unknown | Consistent if carefully worded | Preserve intended product scope, but keep every unsupported rail blocked from live use |
| GAP-002 | Provider-managed funds vs direct integrations remains an open comparison | Consistent | Do not score unknown evidence or select a model until DOC-28 hard gates are met |
| GAP-003 | DOC-02 has a broad capability catalogue; DOC-31 turns selected controls into stable requirement IDs and acceptance criteria | Partially complete | Map every approved in-scope capability to a requirement ID; add IDs for missing support, network, API and reliability controls |
| GAP-004 | DOC-03 is a phase roadmap, not a dated schedule | Consistent and intentional | Do not invent durations, delivery dates, costs or staffing assumptions without capacity and dependency estimates |
| GAP-005 | Technology stack and foundation PR remain proposals pending requirements/architecture approval | Consistent | Keep DEC-007 proposed; keep PR #1 unmerged under DEC-015 |
| GAP-006 | DOC-29 contains a provider inquiry template but external responses are not present | External blocker | Owner must authorize submission; then record actual written responses and evidence dates |
| GAP-007 | DOC-30 lists legal/compliance questions but contains no legal conclusion | External blocker | Obtain qualified Ugandan review of the actual candidate funds flows; do not treat the checklist as legal advice |
| GAP-008 | DOC-31 acceptance criteria are a draft and do not yet have named implementation/test owners or linked test IDs | Open | Add accountable owners, test IDs, reviewer and evidence as the project moves through G1/G2 |
| GAP-009 | Merchant segments, currencies, pricing, settlement expectations, limits, SLO/recovery targets and retention remain open | Product/operational decisions needed | Assign each question to an accountable owner and record the approved answer or explicit deferral |
| GAP-010 | Public-source capability evidence does not establish HARAI's own eligibility, production access or contractual permission | Hard gate | Obtain written provider and contractual evidence; keep affected capabilities blocked |
| GAP-011 | Payouts, refunds, reversals and dispute handling depend on provider-specific lifecycle and accounting behavior | Hard gate | Document method-specific state transitions, ledger effects, finality and reconciliation before implementation approval |
| GAP-012 | Public repository is not an appropriate place for confidential provider agreements, credentials or personal onboarding evidence | Security rule | Keep sensitive evidence in controlled storage and commit only a redacted summary/reference |
| GAP-013 | Owner-assisted read-only portal inspections now establish limited account states and relevant documented APIs, but not production access, written aggregator permission, funds flow or commercial terms | Partially resolved discovery gap; hard gate remains | DOC-37 records the detailed observations; DOC-38 compares them; DOC-28 incorporates them without scoring unknowns. Obtain written evidence only after explicit owner authorization for provider outreach and keep legal/compliance review outstanding |

## 3. Missing or under-specified requirement areas

The following areas needed explicit requirement IDs and testable acceptance criteria, not merely a mention in prose. DOC-39 and the DOC-31 additions SUP-001–SUP-010 now cover these principal gaps; the additions remain Draft/In review pending accountable disposition:

1. Merchant verification evidence access, re-verification and expiry; suspension and closure behavior.
2. Payment-link/session expiry, cancellation, revocation and concurrent usage-limit enforcement.
3. API abuse controls and safe rate-limiting behavior for status/recovery requests.
4. Beneficiary changes and protections against destination substitution.
5. Transaction state machine, allowed transitions, terminality and provider-specific exceptions.
6. Dispute/complaint case ownership, deadlines, evidence and outcome tracking.
7. API versioning/deprecation, stable errors, pagination and correlation IDs.
8. CORS/CSRF and trusted-proxy/IP-allowlist behavior where applicable.
9. Measurable reliability, alerting, backup/restore and recovery objectives, set only after owner review.
10. Accessibility and responsive acceptance criteria for critical checkout and dashboard journeys.

These items should be added to DOC-31 and mapped to the corresponding phases of DOC-03. Their addition does not imply that every optional capability is automatically approved for launch.

## 4. Requirement traceability rule

Before G1 can be declared complete, each proposed launch capability must have:
- a stable requirement ID and priority;
- an accountable product owner and proposed technical owner;
- acceptance criteria and a planned test/evidence reference;
- a linked decision or external dependency;
- an explicit state: Draft, In review, Approved, Blocked, Implemented, Verified, Deferred or Rejected;
- a release gate where provider permission, law, security, finance or operational readiness is required.

A requirement cannot be marked Approved based only on its presence in a document, and cannot be marked Verified without evidence. The product owner and relevant independent reviewers must explicitly approve the baseline.

## 5. Open decision register additions

Track these questions in DOC-00 until explicitly answered:
- Exact launch merchant categories and prohibited/restricted categories.
- Launch currency/currencies, settlement expectations, account ownership and reconciliation sources.
- Provider/intermediary contracting route and written multi-merchant permission.
- Source of funds, prefunding requirements and failure/return handling for money-out.
- Refund/reversal support, dispute lifecycle and financial corrections per provider.
- Customer pricing, provider costs, taxes, limits, reserves and fee payer.
- Account roles, dual control, merchant verification policy and support access.
- Service objectives, pending-age thresholds, incident response and recovery targets.
- Data retention/deletion/hold policy and accountable data-protection roles.
- Named product, technical, security, finance, compliance and independent reviewers.

## 6. Recommended sequence

1. Finish and review DOC-31 requirement IDs and acceptance criteria.
2. Obtain product-owner review of the proposed launch baseline and unresolved decisions.
3. Submit DOC-29 only after the owner authorizes provider outreach; capture responses without guessing.
4. Obtain qualified Ugandan legal/compliance review of the actual candidate funds-flow diagrams.
5. Resolve G1, then draft and review architecture, state-machine, data-model, ledger and threat-model documents for G2.
6. Only after explicit requirements and architecture approval consider authorizing implementation work.

## 7. Audit conclusion

The documentation is coherent enough to continue discovery but **not ready to pass G1**. The portal-inspection gap is now partially closed by DOC-37/DOC-38 and the updated DOC-28, but the principal blockers remain missing written provider evidence, product-owner decisions and qualified professional review—not a reason to reduce the user's comprehensive product ambition. Keep the ambition explicit while refusing to represent unsupported capabilities as available.

No provider has been contacted by this audit. No production integration, live credential, financial action, merge or deployment is authorized.


## 8. Supplemental requirement coverage update (2026-10-19)

The ten gaps listed in Section 3 are now represented by SUP-001–SUP-010 in DOC-31. DOC-34 maps all 54 unique requirement IDs and DOC-03 includes corresponding roadmap tasks. This resolves the gap at the documentation-definition and initial traceability level only. It does not close acceptance, numeric-threshold, named-owner, test-execution, provider, legal/compliance or G1 approval gates.


## 9. Targeted P0 traceability audit (2026-10-24)

**Scope:** documentation-only cross-check of the 32 P0 requirement rows in DOC-31 against DOC-34 and the companion closure rows in DOC-46. This is a bounded consistency audit, not an independent security review, requirements approval, test run, or gate decision.

**Coverage result:** all 32 P0 IDs appear in DOC-31, DOC-34 and DOC-46, with the same set of identifiers: GOV-001, TEN-001, MER-002, IAM-001, IAM-002, MER-003, COL-003, COL-004, COL-005, COL-008, OUT-002, OUT-005, FIN-001, FIN-002, FIN-003, REC-001, REC-002, FIN-005, SEC-001, SEC-002, OPS-001, OPS-002, SEC-003, OPS-003, SUP-001, SUP-002, SUP-003, SUP-004, SUP-006, SUP-007, SUP-008 and SUP-010. DOC-31 and DOC-34 each continue to represent 54 unique requirement IDs overall.

**Confirmed traceability correction:** the DOC-34 SUP-002 dependency list previously cited COL-007 and the state-machine design but omitted COL-001/COL-002, although the DOC-31 SUP-002 planning references include those collection-creation/checkout requirements. DOC-34 now lists COL-001/COL-002 as well as COL-007 and the state-machine design. This aligns traceability references; it does not approve a design or change any requirement's status.

**Per-row review:** DOC-34 supplies a proposed role (not a named person), proposed test/evidence work, dependencies and a blocking/gate note for each P0 row. DOC-46 supplies a companion closure row with role-level accountability, control/closure expectation, dependencies/blocking context and status. Those are planning records only. The audit did not treat proposed tests as existing or executed, role labels as appointments, or documented controls as implemented.

**Remaining closure limitations:**
- All 32 P0 requirements remain Draft — not verified; none passes by virtue of traceability coverage.
- Named accountable people and genuinely independent reviewers remain unassigned.
- Provider permissions/status semantics, the Uganda legal/compliance position, the funds-flow model, finance/accounting acceptance, numeric operational targets and other owner decisions remain unresolved where their source documents say so.
- Required evidence is not yet complete in DOC-45; summary dispositions are not substitutes for dated, located, owned and reviewed source artifacts.
- The 32-row count/ID comparison and the SUP-002 reference correction do not constitute the full closure of DOC-47 architecture findings AR-001–AR-021. AR-021 remains open; AR-019 remains closed only for indexing/cross-linking.

**Gate result:** G1 remains NOT PASSED. G2 remains NOT APPROVED. No tests, implementation, provider contact, production credentials, live financial activity, merge or deployment were performed or authorized by this audit.


## 10. Targeted architecture consistency audit — idempotency scope (2026-10-24)

**Scope:** documentation-only comparison of the canonical idempotency-scope statements in DOC-40, DOC-42, DOC-48 and DOC-49, with the corresponding AR-021/G2 closure records.

**Confirmed inconsistency found and corrected:** DOC-40 §4 previously required the idempotency key to be scoped to tenant + credential/actor + operation class. DOC-42 §6 and DOC-49 §7 instead propose authenticated tenant/merchant + environment + operation class, with endpoint-compatibility rules, and treat credential/actor as audit attributes rather than automatic uniqueness boundaries. DOC-40 has now been corrected to align with DOC-42/DOC-49 and to require one canonical contract. This prevents credential rotation or use of an alternate credential for the same tenant operation from being described as an automatic way to create a separate idempotency scope.

**Cross-document follow-through:** DOC-47 AR-021 now records the DOC-40 inconsistency and its documentary correction; DOC-46 G2-01 and DOC-44's review summary also reflect the correction. The correction is a consistency repair, not an approved architecture decision.

**Still unresolved:** DOC-48 and the wider architecture set still need one approved cross-layer contract for API requests, provider attempts, callbacks/events, ledger postings, reconciliation imports and restore/replay. Canonical request fingerprinting, same-key/different-payload behavior, atomic reservation/concurrency, retention horizon, provider-specific constraints, reference uniqueness and restore/replay safety need accountable decisions and eventual authorized verification. AR-021 remains OPEN.

**Gate result:** G1 remains NOT PASSED; G2 remains NOT APPROVED. No tests, implementation, provider contact, credentials, live financial activity, merge or deployment were performed or authorized by this audit batch.


## 11. Product-decision propagation and G1 consistency review (2026-10-24)

**Scope:** documentation-only comparison of DOC-36 approved product defaults against the master decision register (DOC-00), product scope (DOC-02), launch requirements (DOC-31), G1 review package (DOC-33) and requirement traceability (DOC-34).

**Result:** the inspected sources consistently record Uganda-first/UGX-first direction, both MTN MoMo and Airtel Money, cards through an eligible processor/acquirer, collections and money-out as intended scope, provider-supported refunds/reversals, and no funds-flow model selected yet. DOC-31 and DOC-34 continue to contain 54 unique requirement IDs; the 32 P0 mappings remain traceability/planning records, not accepted or verified controls. No conflicting product decision or evidence claim was found in this bounded pass.

**Clarification applied:** DOC-33 previously described MTN/Airtel as candidates and bank/card methods as subject to verification without stating in that passage that both mobile-money providers and cards are already approved product intent. DOC-33 now separates intended method scope from provider/processor eligibility, contractual permission, integration status and production readiness. The clarification does not change requirement priorities or mark a capability approved for use.

**Open gates:** G1 remains NOT PASSED. Detailed requirement dispositions, written provider/processor evidence, qualified Ugandan legal/compliance review, funds-flow selection, finance/accounting review and accountable reviewer evidence remain open. G2 remains NOT APPROVED. No tests, implementation, provider contact, credentials, live financial activity, merge or deployment were performed or authorized by this audit batch.


## 12. Transaction lifecycle and cancellation/recovery consistency pass (2026-10-24)

**Scope:** documentation-only comparison of DOC-40's canonical lifecycle, DOC-41's state machine and §5A transition guards, DOC-48's adapter dispatch-certainty/evidence-precedence rules, and DOC-49's merchant-facing status and cancellation proposal.

**Result:** no new confirmed contradiction was identified in this bounded pass. The documents consistently distinguish intent state from provider-attempt state, preserve UNKNOWN when dispatch may have occurred, require provider-specific evidence before finality, retain in-flight attempts for reconciliation after cancellation/expiry, and prohibit blind retries or automatic refunds after ambiguous outcomes. DOC-41 §5A and DOC-48 §4.1 provide compatible proposed rules for dispatch uncertainty, late success and conflicting evidence; DOC-49 correctly leaves cancellation semantics unapproved.

**Remaining design/evidence gaps:** AR-004 and AR-006 remain OPEN. The provider/product/operation-specific precedence and finality rules are not contractually established; the accountable owners have not approved the transition/dispatch model; cancellation-versus-dispatch races, worker crash windows, late success, conflicting callback/enquiry/settlement evidence and post-restore recovery have only planned scenario coverage. No test artifacts or execution evidence were checked or claimed in this pass. Do not close these findings on the basis of prose consistency alone.

**Gate result:** G1 remains NOT PASSED; G2 remains NOT APPROVED. Provider permission, qualified legal/compliance and finance review, named operational ownership and later authorized verification remain separate gates. No tests, implementation, provider contact, credentials, live financial activity, merge or deployment were performed or authorized by this audit batch.


## 13. Money-out safety-control sequence review (2026-10-24)

**Scope:** documentation-only review of DOC-47 AR-008 against DOC-41, DOC-42, DOC-46 and DOC-48, focused on funding/balance authority, beneficiary lifecycle, approvals, dispatch uncertainty, ledger effects and reconciliation.

**Finding:** existing sources consistently identify money-out as a separate, disabled-by-default capability, and no contradictory authorization claim was found. However, the safety controls were distributed across state-machine, adapter and ledger documents without one end-to-end control sequence and evidence checklist in the ledger proposal. DOC-42 now contains a proposed sequence covering eligibility and permission gates, funding/balance authority, beneficiary versioning, exact-operation approval, atomic idempotency/dispatch boundary, UNKNOWN recovery, finality, approved ledger effects, reconciliation and safe disablement.

**Status of addition:** proposed design only. It does not choose the funds-flow model, determine who owns or holds funds, assert a provider balance endpoint is authoritative, appoint approvers, establish payout limits, confirm provider permission or approve an accounting treatment. The evidence checklist explicitly leaves those items open. AR-008 remains OPEN; AR-005 remains OPEN for the canonical cross-layer idempotency contract; G2-02 and G2-03 remain open/blocked.

**Next required decisions/evidence:** written provider/product/operation/environment permission; approved source-of-funds and prefunding/reserve model; authoritative balance semantics; beneficiary verification and change policy; amount/velocity limits and separation-of-duties approvals; provider-specific finality, idempotency, return and ambiguous-outcome rules; finance-approved ledger/settlement treatment; named reconciliation/operations owners and authorized verification evidence.

**Provider-evidence proportionality clarification (2026-10-24):** The money-out proposal previously emphasized written provider permission as if it were the universal evidence format for each product/environment. DOC-42 §13 and DOC-48 now use provider account/subscription status, configured credential scope, applicable terms and reliable capability evidence as the normal way to establish enabled operations. Explicit written confirmation is reserved for unclear scope, cases where the provider/contract requires it, or material entitlements such as a multi-merchant gateway role that ordinary credentials do not establish. This does not treat credentials as blanket permission for every product, environment or merchant model, and it does not change the current evidence state: money-out remains blocked until its applicable capability and safety prerequisites are verified.

**Gate result:** G1 remains NOT PASSED and G2 remains NOT APPROVED. No tests, implementation, provider contact, credentials, live financial activity, merge or deployment were performed or authorized by this audit batch.
