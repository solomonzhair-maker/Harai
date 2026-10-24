# DOC-62 — HARAI Repository-Wide Remediation and Verification Plan

**Status:** Draft corrective-action plan from a read-only repository inventory; not an implementation authorization.  
**Review date:** 2026-10-24  
**Reviewed ref:** docs/julypay-discovery-and-requirements  
**Review boundary:** The reviewed tree contains README.md and 42 Markdown documents under docs/. It contains no application source, package manifests, lockfiles, tests, CI workflows, deployment configuration, database migrations, or .env.example files. This is therefore a repository/documentation audit and readiness gap assessment—not a source-code audit, security test, provider verification, or proof that software behavior works.

**Gate status:** G1 requirements approval NOT PASSED. G2 architecture/security approval NOT APPROVED. This document authorizes no implementation, tests, provider contact, credential generation, live financial activity, merge, or deployment.

## 1. Binding remediation rule

Follow the stages below in order. A later stage cannot be marked complete merely because a document describes it, a table maps it, or a proposed test ID exists.

Every corrective action must record a stable ID, exact file/deliverable, accountable role (never an invented person), dependencies, objective acceptance criteria, reviewer, evidence reference, gate impact, and status. Use VERIFIED only after the artifact is inspected and its criteria are actually met. For tests, preserve command, environment, date, result and relevant logs. This audit claims no tests were run.

## 2. Repository inventory

Present on the reviewed ref:
- README.md
- DOC-00 through DOC-03
- DOC-24 through DOC-61 (42 Markdown documents in docs/)

The numbering gap from DOC-03 to DOC-24 is real. DOC-00 still lists DOC-04 through DOC-23 as “Not started”, although later documents cover overlapping topics. The index must distinguish legacy planned IDs, superseded plans, current source-of-truth documents, and work genuinely not started. Do not create duplicate documents just to fill numbering gaps.

Absent from the reviewed ref:
- Application source and package/workspace configuration
- Dependency lockfile and pinned runtime/toolchain
- Tests and test harness
- CI workflow and automated quality/security checks
- Database schema and migrations
- Runtime/deployment configuration and environment templates

README.md describes a proposed stack (NestJS, TypeScript, PostgreSQL, Prisma, Redis, Next.js, pnpm workspaces and GitHub Actions) as “Initial architecture” and refers to provided .env.example files, but those implementation artifacts are absent. The README must describe the stack as proposed and the repository as documentation-only until those artifacts actually exist.

## 3. Findings register

| ID | Priority | Finding | Required correction | Acceptance evidence | Gate |
|---|---|---|---|---|---|
| RR-001 | P0 | No executable implementation or engineering foundation exists on the reviewed ref. | Keep this ref documentation-only until G1/G2 pass. After separate authorization, prepare a foundation PR with manifests, lockfile, source structure, migrations, test harness and CI. Do not invent these artifacts in this pass. | Tree inventory; later foundation PR and reproducible CI evidence only after authorization. | G3 blocked |
| RR-002 | P0 | README can imply an existing stack and environment templates that are absent. | State documentation-only status, label the stack proposed/not implemented, remove the claim that .env.example files are already provided, and link this plan and current gates. | README checked against actual tree; no false implementation claims. | Repository integrity |
| RR-003 | P0 | DOC-00 says DOC-04–DOC-23 are “Not started” despite later documents covering many of those subjects. | Reconcile every legacy row to current source documents; mark covered, superseded/retired, or genuinely not started. Preserve legacy IDs and cross-links. | DOC-00 contains an explicit mapping and one authority per topic. | G0/G1 |
| RR-004 | P0 | DOC-03 contains a stale instruction to keep foundation PR #1 open during discovery; current work is on the documentation branch/PR #2. | Replace PR-number-specific roadmap instructions with durable branch/review/gate policy. Keep current PR state in a dated status record, not permanent roadmap text. | DOC-03 and DOC-00 agree; no stale PR instruction. | Governance |
| RR-005 | P0 | DOC-32 opening scope/status is stale compared with its later sections: its reviewed-baseline list names only early documents and its opening emphasizes G0 while later material tracks G1/G2. | Update header and executive summary with exact ref/date, scope, audit limitations and current gates. Preserve historical findings; do not silently erase history. | DOC-32 opening summary agrees with its latest sections and DOC-00. | G0/G1 |
| RR-006 | P1 | DOC-47 scope omits newer DOC-50–DOC-61, which affect onboarding, UI/API boundaries, domain ownership and security assurance. | Add a dated supplemental review of DOC-50–DOC-61. Identify affected AR findings; do not claim the earlier review covered documents outside its original scope. | Supplemental review gives disposition per relevant document family; DOC-47 remains the authority for AR status. | G1/G2 |
| RR-007 | P0 | Requirement mappings and proposed dispositions are not owner decisions or verification. | Disposition all 54 DOC-31 IDs as accept/amend/defer/reject with rationale, owner, reviewer, date, design link and evidence dependency. Preserve 32 P0 IDs. Keep pending until decisions are recorded; never weaken P0 controls to pass a gate. | DOC-00, DOC-31, DOC-34, DOC-39, DOC-46 and DOC-47 agree on IDs, counts and statuses. | G1 |
| RR-008 | P0 | Overlapping documents can drift: DOC-31/34, DOC-32/35/47, DOC-40–49, DOC-56–59 and DOC-60/61. | Establish an authority map: DOC-00 owns index/decision state; DOC-31 normative requirements; DOC-34 requirement traceability; DOC-32 documentation-gap audit history; DOC-47 architecture findings AR-001–AR-021; DOC-43 threat IDs; DOC-45 evidence records; DOC-60/61 security evidence planning/reconciliation only. Other docs must defer and cross-link. | Authority map and material cross-links exist; conflicts are logged, not silently resolved. | G1/G2 |
| RR-009 | P0 | Security documentation does not demonstrate implementation/effectiveness; DOC-61 records unresolved threat-ID and acceptance-criteria mappings, and DOC-60 defaults to evidence absent. | Reconcile TH-01–TH-20 against SEC-E01–SEC-E21 and exact DOC-31 IDs; classify direct, partial, missing and inapplicable coverage with rationale. Never claim a control works without authorized evidence and independent review. | Complete mapping; evidence stays absent/blocked until actually produced. | G1/G2 |
| RR-010 | P0 | Provider entitlement, funds flow, legal/compliance, finance, settlement and production capability remain unresolved. | Preserve hard gates in DOC-28–DOC-30 and DOC-37–DOC-38. Record source/date/scope/owner/reviewer. Provider contact requires separate explicit authorization; portal access or public API docs do not prove multi-merchant permission. | Evidence-backed decision packet and qualified Uganda legal/compliance and finance review, or capability explicitly remains blocked. | G1/G2; live capability blocked |
| RR-011 | P0 | Technical, independent security, finance/reconciliation, operations and independent requirements reviewers remain unassigned unless formally recorded. | Keep names blank until appointed. Record authority, independence, scope and conflicts. Sole internal review is not independent sign-off. | Owner state agrees across DOC-00, DOC-34, DOC-46, DOC-47, DOC-60 and DOC-61. | G1/G2 |
| RR-012 | P1 | Document growth risks stale links, duplicate authority and status drift. | Add a manual change checklist for index, source-of-truth, IDs/counts, status/date/scope, cross-links, proposal-versus-decision labels and test/implementation claims. Consider automation only after an authorized engineering foundation exists. | Recorded checklist result for every changed document; links resolve or are marked planned. | Documentation quality |
| RR-013 | P1 | No CI or automated quality/security evidence exists on this ref. | Do not invent green checks. After G1/G2 and separate implementation authorization, add deterministic install, formatting/lint, typecheck/build, tests, migration checks, dependency scan and secret scan in stages. | Actual workflow files and run results/URLs; failures fixed or explicitly dispositioned. | G3+ |
| RR-014 | P0 | There is no approved implementation-ready baseline while G1/G2 remain open. | Do not begin payment-engine implementation, provider integration or live configuration from this plan. Close required decisions and reviews first; then seek separate G3 authorization. | DOC-00 records evidence-backed G1/G2 decisions and explicit G3 authorization. | G1/G2 |

## 4. Strict execution sequence

### Stage A — Repository truth and document authority

1. **A-01:** Fix README truthfulness (RR-002).
2. **A-02:** Reconcile DOC-00 index and authority map (RR-003, RR-008).
3. **A-03:** Remove stale PR-specific roadmap instructions from DOC-03 (RR-004).
4. **A-04:** Refresh DOC-32 scope, summary and gate status (RR-005).
5. **A-05:** Add dated supplemental DOC-47 review of DOC-50–DOC-61 (RR-006).
6. **A-06:** Run a manual consistency pass: all IDs exist; counts match; links resolve; proposals are not called approvals; no tests or implementation are claimed without evidence (RR-012).

**Stage A exit:** DOC-00, DOC-03, DOC-32 and DOC-47 agree on scope, authority, governance and gates. G1/G2 are not presumed passed.

### Stage B — Requirements and decisions

7. **B-01:** Product owner plus assigned requirements reviewer disposition every one of the 54 DOC-31 IDs. The proposed 51-retain/3-defer split in DOC-34 is only a recommendation until recorded as a decision.
8. **B-02:** Reconcile all 32 P0 requirements across DOC-31, DOC-34, DOC-39 and DOC-46.
9. **B-03:** Close or explicitly retain each DOC-47 architecture finding using source-backed evidence and accountable roles. Adding prose alone does not close a finding.
10. **B-04:** Complete DOC-43/DOC-60/DOC-61 security traceability against DOC-31 and DOC-34 (RR-009).
11. **B-05:** Resolve or explicitly retain external blockers (RR-010, RR-011): funds-flow model, provider terms/entitlement, qualified Uganda legal/compliance, finance/accounting, reconciliation/operations and independent security/requirements review. If evidence is unavailable, keep affected capability blocked.

**Stage B exit:** G1 may pass only when its actual criteria and accountable decisions are met. G2 is a separate gate. A traceability matrix is not implementation or test evidence.

### Stage C — Architecture and implementation authorization (not authorized yet)

12. **C-01:** Record selected funds-flow model and model-specific financial/accounting rules using provider and qualified reviewer evidence.
13. **C-02:** Obtain technical architecture and independent security review of tenancy, state/finality, idempotency, callbacks, money-out, ledger/reconciliation, restore/replay, secrets and operations.
14. **C-03:** Record approved architecture decisions, rejected options, failure scenarios, ownership and migration/rollback plan.
15. **C-04:** Obtain explicit recorded authorization for a G3 foundation branch/PR. Product-direction approval and this plan do not authorize coding, tests, provider contact, credentials, financial activity, merge or deployment.

**Stage C exit:** DOC-00 records G1 passed, G2 approved with evidence, and separate G3 authorization. If any gate is unmet, stop.

### Stage D — Foundation and verification (only after Stage C authorization)

16. Create the approved application structure, manifests/lockfile, pinned toolchain, placeholder-only environment templates, database schema/migration process, configuration validation and local development guide.
17. Add CI for deterministic install, format/lint, typecheck/build, unit tests, schema/migration checks, dependency scanning and secret scanning.
18. Start with a safe non-financial vertical slice (health/configuration and authentication/tenant-boundary scaffolding). Keep provider adapters and money movement disabled until their specific gates pass.
19. Add approved tests for tenant isolation, authorization/revocation, idempotency/concurrency, callback authenticity/replay, unknown outcomes, reconciliation integrity, webhook retry/deduplication and restore/replay.
20. Record actual commands, CI URLs, environment, dates, reviewers, failures and fixes. Planned test IDs are not proof of execution.
21. Keep work on the authorized feature branch and in reviewable PRs. No merge, deployment or live operation without separate applicable approval.

## 5. Mandatory document-change checklist

- [ ] One clear authority/purpose; links to normative sources.
- [ ] Accurate status, review date, ref and scope.
- [ ] Proposal, decision, external fact and verification evidence clearly distinguished.
- [ ] Requirement/decision/risk/finding/test IDs valid and consistent.
- [ ] 54 requirement IDs and 32 P0 subset preserved unless an approved baseline change is recorded.
- [ ] DOC-00, DOC-31, DOC-34, DOC-39, DOC-46 and DOC-47 reconciled where affected.
- [ ] DOC-43 threat IDs and DOC-60/61 mappings not silently altered or inferred.
- [ ] Cross-references point to existing files/sections or are explicitly planned.
- [ ] No owner, permission, test result, provider capability, legal conclusion, production approval or implementation is invented.
- [ ] Changed file re-fetched and exact content checked before claiming completion.

## 6. Current blocker snapshot

- G0 discovery/requirements preparation remains in progress.
- G1 requirements approval is **NOT PASSED**; G2 architecture/security approval is **NOT APPROVED**.
- All 54 requirement dispositions remain pending unless a later explicit accountable decision is recorded.
- Provider multi-merchant authority, production access, product entitlements, funds-flow model, settlement/account ownership, money-out funding authority and method-specific finality evidence are not established here.
- Named technical, independent security, finance/accounting, operations/reconciliation and independent requirements reviewers remain unassigned unless formally recorded.
- No executable code, tests, CI, migrations or runtime configuration were present on the reviewed ref; no software behavior was verified.

## 7. Completion and change-control rule

Update this plan only with dated, evidence-backed status changes. If a blocker remains, preserve it as BLOCKED and identify the exact decision/evidence needed. Never mark a stage complete because the plan exists. Never lower a safety requirement or remove a blocker solely to meet a deadline or make a gate appear passed.

**Final rule:** Correct repository truthfulness and document authority first, close owner/evidence decisions second, approve architecture third, and only then begin separately authorized engineering work.