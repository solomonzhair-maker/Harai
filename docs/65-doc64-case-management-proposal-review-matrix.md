# DOC-65 — DOC-64 Case-Management Proposal Review Matrix

**Status:** Internal reviewer assessment of a draft proposal; not specialist approval, implementation evidence, or requirement verification.  
**Review date:** 2026-10-24  
**Branch:** docs/julypay-discovery-and-requirements  
**Reviewed sources:** DOC-31 FIN-004 and SUP-005; DOC-34; DOC-42 §5E; DOC-47 AR-022; DOC-63; DOC-64; DOC-30 where compliance/funds-flow questions are relevant.  
**Scope:** Compare DOC-64's proposed design against normative requirements and identify gaps, evidence dependencies and required reviewers. Documentation-only review; no tests or implementation.

## 1. Decision summary

DOC-64 is a material improvement over the previously documented gap: it proposes a coherent case lifecycle, transaction/evidence linkage, ownership, deadline provenance, escalation, communications, tenant-scoped access, resolution/reopening, financial-operation separation and recovery criteria. It addresses the shape of FIN-004 and SUP-005 more directly than reconciliation exception handling.

However, it remains a proposal. Its fifteen CASE criteria are not executed tests, and the review does not establish that the proposed lifecycle is complete for the actual operating model, legally sufficient, supported by provider capabilities, or ready for implementation. The correct disposition is:

- FIN-004 and SUP-005 remain **Accept — intended launch requirements baseline**.
- Their design coverage may be changed from **PRELIMINARY MISSING** to **PRELIMINARY PARTIAL — draft proposal exists; material decisions and reviews remain open**, if the crosswalk is updated consistently.
- No requirement is verified or closed by this review.
- AR-022 remains **OPEN — draft design proposed; review/evidence pending**. Its original statement that no complete design was confirmed is still accurate, but its status text should acknowledge DOC-64.
- G1 remains **NOT PASSED**; G2 remains **NOT APPROVED**.
- No tests, implementation, provider outreach, credentials, live financial activity, merge or deployment is authorized.

## 2. Normative requirement comparison

| Requirement | Normative obligation in DOC-31 | DOC-64 coverage | Reviewer assessment / remaining gap |
|---|---|---|---|
| FIN-004 | A case links to the original transaction and evidence, records owner, deadlines, status, communications and outcome, and cannot silently modify financial records. Provider dispute/complaint obligations and support policy are dependencies. | §§3–8, §§12–14; CASE-001, 003, 005, 007–012. | **Partial.** All named concepts have a proposed treatment. Actual provider dispute types/authority, support policy, accountable owners, deadline rules, approved resolution taxonomy and implementation evidence are absent. |
| SUP-005 | Cases record source, transaction reference, minimized merchant/customer context, owner, deadlines, evidence references, status, communication history and outcome; escalation and retention rules are configurable; tests cover reassignment, overdue cases, access restrictions and closure. | §§3–8, §§10–13; CASE-001–015. | **Partial.** Main fields and controls are proposed. Reassignment is mentioned through ownership history but no explicit reassignment acceptance criterion; overdue handling depends on verified deadline and route rules; retention values/policy are unapproved; test IDs/results are not evidence. |
| MER-004 dependency | Suspension/closure must not orphan pending transactions, disputes or settlements. | §8 and CASE-014. | **Partial.** Cross-link is useful, but policy must define post-suspension access, owner reassignment, retention, communications and responsibility for open cases. |
| OUT-005 dependency | Sensitive evidence/access/audit obligations must align with security and data controls. | §§6 and 8; CASE-007/012. | **Partial.** Good baseline, but classification, encryption, redaction, retention, deletion, legal hold, export and privileged-access policy need approved specifics and security/privacy review. |
| REC-002 dependency | Case records must not replace reconciliation discrepancy evidence or mutate financial records. | §§1 and 7; CASE-009/010. | **Directionally aligned.** DOC-64 correctly separates a complaint case from reconciliation and financial operations. Exact operation link contract and accounting treatment remain unresolved. |
| SEC-002 dependency | Authorization, tenant isolation and sensitive evidence access must be enforced. | §8; CASE-002/007/012. | **Partial.** Server-side enforcement is stated, but concrete role/permission matrix, elevated-access approval/expiry, audit integrity, and negative-test evidence remain open. |
| SUP-010 dependency | Restore/replay safety and evidence of recovery must be respected. | §9 and CASE-015. | **Partial.** Recovery invariant is proposed, but authoritative restore/replay procedure, owner, reconciliation evidence and resume authorization must align with existing approved recovery rules. |

## 3. Criterion-by-criterion review of DOC-64

| DOC-64 section / criteria | Assessment | Required follow-up before design approval |
|---|---|---|
| §§1–2 authority, scope, non-goals | **Adequate boundary proposal.** Explicitly denies inferred provider authority, invented deadlines and automatic financial action. | Preserve these restrictions in any later design. Confirm terminology against the selected operating/funds-flow model after that decision is evidenced. |
| §3 intake; CASE-001 | **Partial.** Unique IDs, provenance and unlinked state are sound. | Define approved intake channels, source-authentication confidence, identity-verification steps, acknowledgement/receipt evidence and triage ownership. Do not claim provider-originated automation without interface and entitlement evidence. |
| §3 category taxonomy | **Open decision.** Categories are explicitly provisional. | Approve categories and applicability by provider, payment method, merchant type and jurisdiction; identify unsupported categories and safe referral route. |
| §4 lifecycle and transitions; CASE-004/011/013 | **Partial.** Proposed states and append-only history cover common paths. | Resolve transition completeness for withdrawal/invalid complaint, duplicate discovered after work begins, rejected/appealed resolution, missing complainant response, owner reassignment, reopened/closed cases and unresolved financial uncertainty. Keep payment and case statuses distinct. |
| §5.1 ownership; CASE-003 | **Partial.** Role descriptions are useful, but no named operating coverage exists. | Assign accountable people/rotations and backups; define assignment/reassignment authority, unassigned-case alerting, acknowledgement and continuity during suspension/termination. Names and authority must be recorded in controlled evidence, not guessed in this public proposal. |
| §5.2 deadline registry; CASE-005 | **Safety-aligned but blocked.** Does not invent deadlines. | Obtain attributable provider/processor and qualified Uganda legal/compliance sources; approve trigger, timezone, business-day, pause/resume and supersession semantics. Define internal targets separately. Unknown rule must remain visibly unverified and be escalated. |
| §5.3 escalation; CASE-006 | **Partial.** Trigger concepts are reasonable; destination and timing are not defined. | Approve role/team destinations, fallback route, acknowledgement timeout and escalation ownership. A logged routing attempt must not be represented as a successful handoff without evidence. |
| §6 evidence and communications; CASE-007/008 | **Partial.** Provenance and message-state distinctions are good. | Define verification standards by source, allowed evidence types, customer/merchant identity checks, language/templates, proof of delivery, redaction and data minimization. Do not store secrets or sensitive authentication data in case records. |
| §7 resolution and operation separation; CASE-009/010/011 | **Strong safety direction; incomplete policy.** The proposal prevents silent financial mutations and requires separate operation references. | Finance/accounting and provider reviewers must confirm eligible outcomes, authority, accounting treatment, finality/unknown states, approval separation and linkage semantics for each supported operation. A case resolution cannot imply a refund/reversal succeeded. |
| §8 access, isolation and audit; CASE-002/012 | **Partial.** Explicit server-side tenant checks and restricted evidence are appropriate. | Review against the canonical IAM/tenant controls; specify case-level permissions, support access justification/expiry, export controls, audit retention/integrity, access reviews and privacy rights. Require negative tests only after separate test authorization. |
| §9 logical entities | **Conceptual only.** No physical schema is claimed. | Architecture/security/privacy review must decide consistency boundaries, event immutability, deletion/legal hold, encryption, indexing, idempotency, attachment storage and transactional behavior. Do not treat this as a migration specification. |
| §10 notifications/reporting | **Partial.** Operational views are sketched. | Define recipients, delivery state, sensitive-content minimization, retry/duplicate behavior, alert acknowledgement, access leakage checks and report/export authorization. |
| §11 open decisions | **Appropriate but not closure evidence.** | Convert each dependency to a decision/evidence record with owner, source/version, reviewer and status when evidence exists. Do not invent names or provider destinations. |
| §12 CASE-001–015 | **Proposed acceptance criteria only.** | Map each criterion to future test specifications and evidence requirements. In particular add/trace explicit reassignment coverage for SUP-005, overdue behavior, access restrictions, closure, communications, retention and suspension. No tests have been run. |
| §13 review workflow | **Directionally correct.** | Obtain the listed specialist reviews and record their actual outcomes; do not treat the internal review as independent sign-off. |
| §14 disposition | **Correct.** FIN-004/SUP-005 and AR-022 remain open at the appropriate levels. | Update DOC-63/DOC-34/DOC-47 references consistently so they acknowledge the draft without overstating approval or closure. |

## 4. Cross-document consistency findings

1. **DOC-31 is normative.** FIN-004 and SUP-005 wording remains unchanged; DOC-64 does not supersede or dilute it.
2. **DOC-34 is stale in its case-gap narrative.** It still says no complete case lifecycle is confirmed. This remains true as an approval/evidence statement, but should say DOC-64 now provides a draft proposal and that approval/evidence remain outstanding.
3. **DOC-47 AR-022 needs a status-description update, not closure.** Its risk statement remains valid. Replace the status phrase “Open — design missing” with “Open — draft proposal exists; specialist review/evidence pending” and cite DOC-64/DOC-65.
4. **DOC-63 FIN-004/SUP-005 rows need reconciliation.** Their PRELIMINARY MISSING label is now outdated because a specific draft design exists. Change only to PRELIMINARY PARTIAL — DOC-64 draft proposal exists; decisions/reviews/evidence pending; keep execution evidence NOT CREATED, dependencies BLOCKED, and no verification/closure.
5. **DOC-42 §5E remains a reconciliation-exception design, not a complaint-case workflow.** DOC-64 correctly treats it as a dependency and does not conflate the two.
6. **DOC-30 reinforces the legal/funds-flow boundary.** The case design cannot determine regulatory classification, responsibility, retention or complaint duties without qualified review and actual funds-flow/provider evidence.
7. **DOC-00 and DOC-62 should index/record this review** without changing the 54 requirement IDs, 51 Accept / 3 Defer split, 32 P0 subset, funds-flow model, or G1/G2 gates.

## 5. Required closure evidence and reviewers

| Work item | Evidence needed | Required accountable review |
|---|---|---|
| Case taxonomy and lifecycle | Approved category/transition matrix; edge-case decisions; mapping to provider-specific states where supported | Product + support/operations + technical architecture |
| Provider dispute/chargeback obligations | Attributable current product-specific written terms/specification; entitlement and channel evidence | Provider operations + qualified compliance/legal |
| Legal/regulatory complaint duties | Written review of actual Uganda activities, applicable notices, deadlines, retention and escalation | Qualified Uganda legal/compliance adviser |
| Deadline registry | Source/version, calculation semantics, effective date, owner, reviewer and supersession history for each rule | Provider operations + qualified legal/compliance |
| Financial outcomes | Approved outcome taxonomy, operation linkage, authorization/approval rules and accounting treatment | Finance/accounting + provider operations + architecture |
| Evidence, privacy and audit | Approved data classification, identity verification, access/export policy, retention/deletion/legal hold, audit integrity | Security + privacy + qualified compliance |
| Staffing and routing | Named accountable owner/backup, real destinations, acknowledgement/fallback behavior and authority | Support/operations owner; actual appointments must be evidenced |
| Recovery and suspension | Consistency with approved restore/replay and merchant suspension/closure controls | Operations + technical architecture + security |
| Verification plan | Criterion-to-test-spec mapping, synthetic fixtures and evidence retention plan; execution only after separate authorization | Test/quality owner + relevant independent reviewer |
| Final disposition | Updated crosswalk and architecture finding supported by the evidence above | Accountable reviewer and any required independent/specialist reviewers |

## 6. Review outcome and boundaries

**Outcome: draft proposal accepted as a useful design input, not approved as the final design.** DOC-64 covers the major requirement concepts, but open decisions and evidence dependencies prevent a complete design/acceptance claim.

The following remain unchanged:
- 54 unique requirement IDs; 51 Accept and 3 Defer (EXT-001–EXT-003).
- All 32 P0 requirements remain in scope; none is declared verified here.
- FIN-004 and SUP-005 remain accepted, preliminary partial after crosswalk reconciliation, not verified or closed.
- AR-022 remains open.
- G1 NOT PASSED; G2 NOT APPROVED.
- No tests, implementation, provider outreach, credential use, live financial activity, merge or deployment was performed or authorized.

**Next action:** update the cross-document status/reference text and record this review in DOC-62. Then continue the broader evidence/architecture closure work; do not begin implementation based on this proposal alone.
