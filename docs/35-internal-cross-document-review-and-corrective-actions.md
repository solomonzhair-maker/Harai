# DOC-35 — Internal Cross-Document Review and Corrective-Action Register

**Status:** Draft internal review by ChatGPT; not external certification  
**Review date:** 2026-10-19  
**Documents reviewed:** DOC-00, DOC-02, DOC-03, DOC-28, DOC-29, DOC-30, DOC-31, DOC-32, DOC-33 and DOC-34  
**Purpose:** Check whether the current discovery and requirements documents tell one consistent story, record corrective edits, and distinguish work we can continue from decisions/evidence that only the product owner or qualified external parties can provide.

> This is a document review, not a formal independent audit, legal opinion, provider approval, penetration test or finance certification. I can conduct further critical reviews, but I will not describe my own review as independent external sign-off.

## 1. Executive outcome

The documents are sufficiently aligned to continue requirements preparation, but **G1 is not passed** and the repository is not ready for payment-engine implementation authorization.

The current product direction remains: Uganda-first, multi-merchant, comprehensive launch ambition, collections plus money-out, and refunds/reversals where the specific provider flow supports them. Provider-managed funds and direct integrations remain candidates to compare; no funds-flow model has been selected.

The principal blockers are substantive, not merely editorial:
- provider eligibility and written permission for the intended multi-merchant model;
- account ownership, funds control/custody responsibilities, settlement and money-out funding;
- provider-specific payment, payout, refund, reversal, return and reconciliation semantics;
- qualified Ugandan legal/compliance review of the actual proposed service and funds flow;
- approved product decisions, accountable named owners, and concrete requirement-to-test/evidence mapping.

## 2. Cross-document consistency findings

| ID | Finding | Severity | Disposition / next action |
|---|---|---|---|
| CR-001 | DOC-02, DOC-03, DOC-31 and DOC-33 consistently describe a comprehensive launch ambition while keeping provider-dependent operations gated | Positive control | Preserve this distinction in future edits; never convert ambition into a claim of provider support |
| CR-002 | DOC-03 Phase 9 heading used “Planned money-out ... and additional methods,” which could be read as making refunds/money-out broadly optional despite the intended launch direction | Editorial ambiguity | Corrected Phase 9 heading to distinguish money-out/refunds from conditional additional methods; added explicit provider-specific refund/reversal limitation |
| CR-003 | DOC-00 had a stale review date and repeated the G0 status in two separate paragraphs, making the current gate summary less clear | Editorial/status hygiene | Correct review date and consolidate current gate summary; record completion only after the index update succeeds |
| CR-004 | DOC-31 has 43 unique requirement IDs; DOC-33 cross-review and DOC-34 tracking matrix cover those 43 IDs | Coverage check | Matrix coverage is confirmed; owner, test and evidence values are still proposals and remain unverified |
| CR-005 | DOC-34's proposed test IDs are not actual test cases or execution results | Evidence integrity | Keep them explicitly labelled proposed; after G1/G2 and implementation authorization, map them to actual test files, runs, commit SHAs and results |
| CR-006 | Role-based responsibility is proposed, but the product owner has not confirmed named people, availability or independence | Governance blocker | Keep roles unassigned until confirmed; the working team can continue with ChatGPT review without inventing appointments |
| CR-007 | DOC-28/DOC-29 public evidence and inquiry drafts cannot prove contractual eligibility, current commercial terms or production capability | External evidence blocker | Provider outreach requires explicit owner authorization; capture written responses and their limitations |
| CR-008 | DOC-30 correctly frames Ugandan regulatory questions as a checklist rather than a legal determination | External professional dependency | Prepare accurate diagrams and questions; do not state a legal conclusion without qualified review |
| CR-009 | G1 requirements approval, G2 architecture approval, implementation authorization, provider onboarding and production launch are separate decisions | Critical control | Preserve separate recorded gates; no document or test plan silently grants implementation or live-payment authority |
| CR-010 | The product-owner decision register remains open for merchant eligibility, currency, pricing, fees/taxes, risk limits, recovery objectives and launch exclusions | Product-owner decisions | Prepare recommendations and ask the owner only where a decision materially changes the baseline |

## 3. Corrective edits performed

- Updated DOC-03 Phase 9 heading to: “Money-out, refunds/reversals and conditional additional methods (provider/legal gates apply).”
- Added a Phase 9.2 statement that refunds/reversals belong in intended launch requirements where the specific provider flow supports them, without implying universal support.
- DOC-33 records the cross-review of the 43 DOC-31 requirement IDs.
- DOC-34 provides the requirement-level tracking view, with all role assignments and test identifiers explicitly marked as proposals.
- DOC-00 gate summary and review-date cleanup are intended corrective actions; verify the updated file revision before treating them as complete.

## 4. Work ChatGPT can continue without new owner decisions

1. Review and improve documentation for consistency, completeness and unambiguous gates.
2. Prepare proposed architecture, threat model, domain/state-machine, data-model, ledger/reconciliation and API contract documents, explicitly labelled draft until G1 decisions are recorded.
3. Develop requirement-to-design and requirement-to-test plans using synthetic scenarios and clearly proposed identifiers.
4. Inspect repository files and CI evidence read-only, distinguish actual results from assumptions, and report exact gaps.
5. Draft provider inquiries and counsel briefing materials without sending them.
6. Prepare implementation prompts and phased execution checklists, but do not initiate payment-engine implementation until the user explicitly authorizes it after requirements/architecture review.

## 5. Actions that remain gated

- **Owner decision:** launch merchant allowlist/exclusions, initial currencies, fee/pricing principles, risk limits, service/recovery objectives and approval policy.
- **Explicit authorization:** contacting providers, submitting business applications, sharing documents externally, or requesting commercial terms.
- **Provider evidence:** aggregator/multi-merchant permission, collection/disbursement product access, fees, limits, settlement, refunds/reversals, callbacks, reporting and support.
- **Qualified Ugandan legal/compliance review:** regulatory classification, licensing/permissions, account/custody roles, merchant due diligence, privacy, tax and contracts.
- **Implementation authorization:** code changes to the payment engine, provider credentials, live financial actions, production deployment and PR merges remain separate from this document review.
- **External assurance:** a genuine independent security assessment or legal/financial certification cannot be replaced by the same assistant reviewing its own work.

## 6. Decision approach and defaults for future drafting

To keep progress moving without silently making consequential choices:
- use conservative proposed defaults in drafts, mark them **Proposed**, and record alternatives;
- for safety-critical unknowns, default to **disabled/blocked until evidenced**;
- avoid inventing prices, provider capabilities, numeric limits, legal conclusions or people;
- use synthetic data and non-production examples in plans and tests;
- when a choice materially changes launch scope, funds flow, contractual obligations, risk appetite or customer commitments, present the recommendation and ask the product owner directly;
- do not ask the owner to re-approve routine editorial work or low-risk documentation improvements already within the agreed scope.

## 7. Current gate and next sequence

**G0 — Discovery: in progress. G1 — Requirements approval: not passed.** The next useful work is to complete the owner decision baseline and prepare draft G2 architecture artefacts in parallel where assumptions can be kept explicit. Any architecture element that depends on unresolved custody, account ownership, provider behavior or legal permissions must remain a decision point rather than a fabricated design fact.

No provider has been contacted by this review. No legal approval, provider permission, independent external audit, implementation, merge or deployment is claimed.
