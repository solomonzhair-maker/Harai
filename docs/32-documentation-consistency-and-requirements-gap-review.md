# DOC-32 — Documentation Consistency and Requirements Gap Review

**Status:** Draft audit record — documentation review only  
**Reviewed baseline:** DOC-00, DOC-02, DOC-03, DOC-28, DOC-29, DOC-30 and DOC-31 on the documentation branch  
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

## 3. Missing or under-specified requirement areas

The following areas need explicit requirement IDs and testable acceptance criteria, not merely a mention in prose:

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

The documentation is coherent enough to continue discovery but **not ready to pass G1**. The principal blockers are missing external evidence and decisions, not a reason to reduce the user's comprehensive product ambition. Keep the ambition explicit while refusing to represent unsupported capabilities as available.

No provider has been contacted by this audit. No production integration, live credential, financial action, merge or deployment is authorized.
