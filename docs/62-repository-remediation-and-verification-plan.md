# DOC-62 — HARAI Repository-Wide Remediation and Verification Plan

**Status:** Draft corrective-action plan from a read-only repository inventory; not an implementation authorization.  
**Review date:** 2026-10-24  
**Reviewed ref:** docs/julypay-discovery-and-requirements  
**Review boundary:** Read-only inventory covered the three active working refs: main, feat/initial-foundation, and docs/julypay-discovery-and-requirements. main contains README.md and one DOC-47 file; the documentation branch contains README.md and 42 Markdown documents under docs/; the foundation branch contains a root package.json, pnpm-workspace.yaml, turbo.json, .gitignore, .env.example, app/package placeholder READMEs and two short docs. Across these refs there is no actual application source, no workspace-level package.json files, no lockfile, no tests, no CI workflow, no database schema/migrations, and no deployment configuration. This is a cross-branch repository inventory and readiness assessment—not a source-code audit, security test, provider verification, or proof that software behavior works.

**Gate status:** G1 requirements approval NOT PASSED. G2 architecture/security approval NOT APPROVED. This document authorizes no implementation, tests, provider contact, credential generation, live financial activity, merge, or deployment.

## 1. Binding remediation rule

Follow the stages below in order. A later stage cannot be marked complete merely because a document describes it, a table maps it, or a proposed test ID exists.

Every corrective action must record a stable ID, exact file/deliverable, accountable role (never an invented person), dependencies, objective acceptance criteria, reviewer, evidence reference, gate impact, and status. Use VERIFIED only after the artifact is inspected and its criteria are actually met. For tests, preserve command, environment, date, result and relevant logs. This audit claims no tests were run.

## 2. Repository inventory

### 2.1 Branch-by-branch inventory

- **main:** README.md plus docs/47-detailed-architecture-review-and-contradiction-register.md. It does not contain the current discovery document set or the foundation scaffold.
- **docs/julypay-discovery-and-requirements:** README.md plus 42 Markdown files under docs/ (DOC-00–DOC-03 and DOC-24–DOC-61). This is the current discovery/documentation branch. It contains no app source or engineering scaffold.
- **feat/initial-foundation:** root package.json, pnpm-workspace.yaml, turbo.json, .gitignore, .env.example, README.md; apps/api, apps/checkout, apps/dashboard and packages/database, packages/payment-core, packages/provider-sdk directories containing only placeholder README files; docs/architecture.md and docs/development.md. PR #1 is open/draft/unmerged. It is a scaffold, not a working payment gateway.

The numbering gap from DOC-03 to DOC-24 is real. DOC-00 still lists DOC-04 through DOC-23 as “Not started”, although later documents cover overlapping topics. The index must distinguish legacy planned IDs, superseded plans, current source-of-truth documents, and work genuinely not started. Do not create duplicate documents just to fill numbering gaps.

### 2.2 Confirmed engineering gaps across branches

- No actual application source files are present in the app or package directories; they contain README placeholders only.
- The root package.json defines build/dev/lint/test/typecheck scripts, but the workspace directories have no package.json files implementing those scripts. There is no pnpm-lock.yaml, so dependency installation is not reproducible from the repository as currently shaped.
- No test suite, test harness, CI workflow, database schema/migrations, or deployment configuration is present.
- .env.example exists only on feat/initial-foundation, not on main or the documentation branch. The shared README refers to it as if it is available from all refs.
- .env.example uses local-development database credentials in its sample URL. Keep these explicitly local-only and never reuse them for shared/staging/production environments.

README.md describes a proposed stack (NestJS, TypeScript, PostgreSQL, Prisma, Redis, Next.js, pnpm workspaces and GitHub Actions) as “Initial architecture” and refers to environment templates. Label this as a proposal, explain the branch differences and current placeholder-only status, and do not claim the platform or CI exists.

## 2.3 Branch-alignment disposition (2026-10-24)

**Canonical active working branch for DOC-62:** `docs/julypay-discovery-and-requirements`. Continue documentation remediation and decisions here so active work does not split across the foundation and discovery branches.

PR #1 (`feat/initial-foundation`) has been retargeted to this canonical branch as an integration proposal. It remains **open, draft, and unmerged**. A merge attempt was rejected by GitHub because the PR is still a draft; it must not be promoted and merged merely to force alignment. The foundation files are only a scaffold, and DOC-62 requires review of the integration before it is accepted. PR #2 remains the draft proposal from the canonical branch to `main`.

**Target state:** one canonical active working branch; keep `main` unchanged; retain PR #1 and PR #2 as separate draft review boundaries until their acceptance conditions and applicable reviews are met. This is an alignment decision, not a claim that the branch contents have already been merged. Do not create another long-lived working branch unless a concrete technical reason is recorded.

## 2.4 Stage A execution log — 2026-10-24

| Action | Status | Evidence / remaining condition |
|---|---|---|
| A-01 README truthfulness | **Corrected on working branches; `main` integration deferred** | README was corrected on the canonical discovery branch and foundation branch to describe the stack as proposed, identify the scaffold as incomplete, state G1/G2 status and link to the current source documents. The older `main` README remains unchanged by design; RR-002 records that its correction must arrive through the reviewed PR #2 path after applicable gates. |
| A-02 DOC-00 index and authority reconciliation | **Verified on canonical branch** | All 20 legacy DOC-04–DOC-23 rows map to current source documents and explicitly distinguish covered, partial and open work. Every mapped document target was re-fetched successfully. DEC-043 records canonical-branch governance; DEC-044 records the crosswalk. No mapping asserts implementation or verification. |
| A-03 DOC-03 stale PR-specific instruction | **Verified on canonical branch** | The stale instruction naming PR #1 is absent; the durable integration-review and acceptance-condition wording is present. DEC-015 uses the same governance boundary. |
| A-04 DOC-32 audit scope/status | **Verified on canonical branch** | The header and executive summary distinguish the original review from supplemental coverage through DOC-61, point to DOC-62 for repository inventory, state audit limitations, and preserve G0 in progress, G1 not passed and G2 not approved. |
| A-05 DOC-47 supplemental review | **Verified on canonical branch** | The dated supplemental review of DOC-50–DOC-62 is present, separates the initial and supplemental review scopes, preserves AR-001–AR-021 as open unless separately dispositioned, and does not imply architecture approval. |
| A-06 Manual consistency pass | **Completed for the current canonical documentation set** | Re-fetched DOC-00, DOC-03, DOC-31, DOC-32, DOC-34, DOC-39, DOC-43, DOC-46, DOC-47, DOC-60 and DOC-61. DOC-31 has 54 unique requirement IDs and 32 P0 IDs; DOC-34 contains the same 54 IDs with no missing/extra IDs; DOC-43, DOC-60 and DOC-61 each reference the full TH-01–TH-20 set. All 20 DOC-00 legacy mapping targets were re-fetched successfully. DOC-03 has no stale PR #1 instruction; DOC-32 gate wording and DOC-47 supplemental scope are present. Current PR mergeability is recorded separately under A-08; it remains an integration blocker, not a claim of documentation inconsistency. |
| A-07 Foundation scaffold criteria | **Criteria recorded — no implementation** | RR-015/RR-016/RR-018 define the missing workspace manifests/lockfile, absent CI/tests and local-only environment sample constraints with future acceptance evidence. These are planning criteria only; no implementation or test execution is authorized. |
| A-08 Branch integration order | **Decision recorded; integration pending review** | `docs/julypay-discovery-and-requirements` is the canonical active branch. PR #1 (`feat/initial-foundation` → canonical branch) and PR #2 (canonical branch → `main`) remain draft/unmerged; GitHub currently reports both PRs as not mergeable. The canonical branch is 311 commits ahead of `main` and one commit behind it; the foundation branch is materially divergent from the canonical branch. Diagnose and resolve each PR's mergeability through an explicit reviewed integration path after applicable gates; do not force-merge, bulk-sync branches, or alter `main` directly. No PR was merged and `main` is unchanged. |

This log records completion of the canonical-branch documentation consistency work in Stage A. The working-branch README corrections, legacy index crosswalk, roadmap governance, audit scope and supplemental architecture review were re-fetched and checked. This does **not** mean PR #1 or PR #2 is accepted or mergeable, does not change `main`, and does not pass G1/G2. PR integration remains subject to the review and gate conditions recorded in A-08 and the findings register. No implementation or tests were authorized or performed.


## 2.5 Stage B execution log — 2026-10-24

| Action | Status | Evidence / remaining condition |
|---|---|---|
| B-01 Disposition all 54 DOC-31 requirements | **Scope disposition reconciled across current registers — 2026-10-24** | Verified DOC-00 DEC-046, DOC-31's accountable scope note, DOC-34 §8.7's 54-row table (51 Accept / 3 Defer), DOC-39's SUP-001–SUP-010 disposition, DOC-46's matching 32-ID P0 set, and DOC-47 AR-001/status narrative. EXT-001–EXT-003 remain deferred from core launch; all 32 P0 requirements remain accepted for retention. This is a scope/traceability decision, not feasibility or implementation evidence. Specialist legal, finance/accounting, security, provider and operations reviews remain required where gates demand them. G1 remains NOT PASSED; G2 remains NOT APPROVED. |
| B-02 Reconcile all 32 P0 requirements | **Complete — documentation cross-check verified 2026-10-24** | DOC-31, DOC-34 and DOC-46 each contain the same 32 P0 IDs, with no missing or extra IDs. DOC-34's full 54-row decision table contains 51 Accept and 3 Defer dispositions. DOC-39's ten supplemental requirements SUP-001–SUP-010 are recorded as accepted for retention; all eight P0 supplemental requirements are in the shared 32-ID P0 set. This is scope/traceability reconciliation only; P0 closure evidence, specialist reviews and G1/G2 remain open. No tests or implementation were performed. |
| B-03 Disposition DOC-47 architecture findings | **Status reconciliation recorded; evidence work remains open** | Verified DOC-47 and DOC-00 on 2026-10-24: AR-001–AR-018 and AR-020–AR-021 remain open; AR-019 is closed only for verified document links; DEC-047 records the accountable reviewer's approved status treatment. The targeted AR-008 cross-document audit found no demonstrable contradiction in the reviewed requirements/design references, but provider, funds-flow, finance/accounting, legal/compliance and operational evidence remains insufficient; money-out stays disabled/blocked. No finding closure or architecture approval is implied. G1 remains NOT PASSED; G2 remains NOT APPROVED. |
| B-04 Complete security traceability | **Documentation cross-check complete — 2026-10-24; control evidence remains absent** | Re-fetched DOC-31, DOC-34, DOC-43, DOC-60 and DOC-61. DOC-43/DOC-60/DOC-61 each cover TH-01–TH-20; DOC-60 has 21 SEC-E rows and DOC-61 has all 21 checklist entries; every requirement ID cited by DOC-60 rows exists in DOC-31 and DOC-34. Updated stale SEC-E17–SEC-E21 checklist wording in DOC-61 to reflect approved RCP-01–RCP-08 criteria. This verifies identifier presence and documentation consistency only—not complete semantic coverage, implementation, control effectiveness, test execution or independent security approval. All threats remain OPEN; all controls remain Draft — evidence absent. |
| B-05 Resolve or retain external blockers | **External blockers audited and explicitly retained — 2026-10-24; external resolution not performed** | DOC-28–DOC-30, DOC-37–DOC-38 and DOC-46 were reviewed; the targeted AR-003 audit is recorded above. No funds-flow model is selected; written multi-merchant/aggregator eligibility and production approval remain unconfirmed for MTN and Airtel. MTN's reviewed portal showed Test Collections subscriptions but no Disbursement subscription or visible production approval; Airtel showed no live applications and no visible production approval. Settlement/account ownership, payout funding/prefunding, full fees/limits, refunds/reversals, support terms, provider status/finality, qualified Ugandan legal/compliance conclusions, finance/accounting treatment, named operations/reconciliation owners and genuinely independent reviewers remain open. DOC-29 is an inquiry pack only; no provider contact, application, credentials, external legal engagement or binding commitment is authorized. Keep affected money-out and other externally blocked capabilities disabled. G1 NOT PASSED; G2 NOT APPROVED. |




### AR-003 targeted external-permission audit — 2026-10-24

**Result: the blocker is consistently represented across the reviewed documents; permission remains unconfirmed.** Read-only comparison covered DOC-47 AR-003; DOC-28 funds-flow model comparison; DOC-37 and DOC-38 provider evidence summaries; DOC-40 capability gates; DOC-41 provider/product-specific lifecycle requirements; and DOC-46 G2-D04/G2-02 ownership and evidence gates.

- **Consistent blocker:** DOC-28 keeps all three funds-flow alternatives unselected until contractual permission, account/settlement roles, product access, funding and reconciliation evidence is obtained. DOC-37/DOC-38 explicitly distinguish public technical documentation and visible portal state from written permission and contractual eligibility. DOC-40 keeps money-out disabled unless provider, legal, funds-flow and operational gates pass. DOC-46 requires account-specific production approval, funding source, limits and terms.
- **MTN evidence boundary:** reviewed portal observations showed Collections and Collection Widget subscriptions active in Test, but no Disbursement subscription or visible production approval. Public aggregator/disbursement API documentation is not account-specific permission.
- **Airtel evidence boundary:** reviewed dashboard observations showed zero live applications and three archived applications; relevant APIs are documented, but no production approval was visible. Documentation about merchant relationships does not establish HARAI's eligibility.
- **Still missing:** attributable written permission for HARAI's exact multi-merchant/sub-merchant role; approved contracting entity and each merchant's account/contract responsibilities; production entitlements; account ownership/control and settlement destination; payout funding/prefunding/reserves; complete fees, limits and timing; and accountable legal/compliance and finance/accounting review.
- **Conclusion:** no contradiction was found in the reviewed blocker statements, but absence of contradiction is not evidence of authorization. Keep AR-003 **OPEN / external blocker** and keep affected capabilities disabled. The portal findings are dated, bounded observations—not evidence of provider refusal or current account status.
- **Not performed:** no provider contact, application submission, credential access, binding commitment, live transaction, implementation, tests, merge or deployment.

### AR-008 targeted source audit — 2026-10-24

**Result: requirement/design references are materially aligned; the finding remains open and money-out remains blocked.** The review compared DOC-47 AR-008 with DOC-31 OUT-001/OUT-002/OUT-005/OUT-006, SUP-003, FIN-002, REC-001/REC-002; DOC-40; DOC-41; DOC-42; DOC-46 G2-D04/G2-02; and the provider evidence summaries in DOC-37/DOC-38.

- **Coverage alignment:** the requirement set addresses eligible merchant and verified beneficiary, approved funding, provider/method eligibility, limits/fees, required approvals, auditable actions, beneficiary-change/pending-payout safety, ambiguous-outcome recovery, append-only financial history and provider/settlement reconciliation.
- **Gate alignment:** DOC-40 and DOC-41 keep money-out disabled until applicable provider, legal, funds-flow, account/contract and operational gates pass. DOC-46 G2-D04 calls for account approval, production access, funding source, limits and operating terms; G2-02 keeps provider lifecycle/recovery blocked where authoritative evidence is absent.
- **Evidence remains insufficient:** DOC-37/DOC-38 record that the reviewed MTN portal showed no visible Disbursement subscription or production approval, and Airtel showed no visible production approval; written multi-merchant authority, funding/prefunding terms, settlement timing, full fees/limits and support terms remain unconfirmed. These observations are bounded to the read-only review and are not proof of provider refusal or current account status.
- **No demonstrable contradiction found in this targeted pass.** Do not weaken the requirements or infer authority from published API documentation. Keep AR-008 open and money-out disabled until the specific missing provider, funds-flow, finance/accounting, legal/compliance and operations evidence is available and reviewed.
- **Not performed:** no provider contact, credential use, live financial action, implementation, tests, merge or deployment.

### B-03 preliminary architecture-finding disposition recommendations — 2026-10-24

These are proposed status treatments for accountable review, not new architecture approvals. DOC-47 remains the authority for finding status. The recorded requirement-scope decision does not close design, external-evidence, specialist-review or verification findings.

| Finding | Recommended treatment | Remaining basis / closure condition |
|---|---|---|
| AR-001 | Retain open | The 51 Accept / 3 Defer scope decision is recorded, but requirement-to-design mapping, approved designs, actual test artifacts/evidence and reviewer/gate traceability remain incomplete. |
| AR-002 | Retain open | Funds-flow model and model-specific accounting/posting rules require owner decision, qualified finance/accounting and Uganda legal/compliance review, and actual provider/partner evidence. |
| AR-003 | Retain blocked | Written provider permission, product entitlements, merchant model, production status and account/settlement roles are unconfirmed. |
| AR-004 | Retain open | Provider/product/operation/API-version finality and event/status precedence matrix needs dated source evidence, accountable review and later authorized verification. |
| AR-005 | Retain open | Canonical cross-layer idempotency contract still needs accountable decisions for compatibility, fingerprints, retention, provider guarantees, imports, outbound events and restore authority. |
| AR-006 | Retain open | Lifecycle race, cancellation/expiry, late-success and compensating-operation rules need accountable approval, provider/finance evidence and later execution evidence. |
| AR-007 | Retain blocked | Provider-specific refund, reversal, return, dispute and money-out support, authority, funding, timing and finality remain unconfirmed. |
| AR-008 | Retain blocked | Money-out funding/balance authority, beneficiary controls, limits, approvals, returns, settlement and reconciliation must be reviewed against the selected funds-flow model and provider terms. |
| AR-009 | Retain open | Trusted proxy hops/CIDRs, forwarded-header handling, CORS/CSRF, allowlist recovery and environment configuration need approved deployment/security decisions and later negative tests. |
| AR-010 | Retain open | Concrete tenant-enforcement choices across data access, workers, callbacks, caches, storage, exports, support and revocation remain undecided and require security review/evidence. |
| AR-011 | Retain open | Numeric reliability, rate, pending-age, quota, RTO/RPO, backup and retention targets require owner/operations decisions grounded in provider limits and capacity. |
| AR-012 | Retain open | Correlation schema, audit integrity/access, redaction, retention/deletion and privacy boundaries need accountable security/privacy/finance decisions. |
| AR-013 | Retain blocked | Card processor/acquirer eligibility, written terms, card-data boundary, security/PCI scope, disputes, fees and settlement remain unestablished; card capability stays disabled. |
| AR-014 | Retain open | Qualified independent security, finance/accounting and legal/compliance review plus required external evidence are not supplied by the internal documentation review. |
| AR-015 | Retain open | Proposed test IDs are not execution evidence; artifacts, commit/build, environment, command/result, date and reviewer are absent until authorized testing occurs. |
| AR-016 | Retain open | Exact public API/event schemas, error mapping, pagination/versioning, signing, retry/expiry and replay authority need named owners and accountable technical/security decisions. |
| AR-017 | Retain open | Authoritative reconciliation sources, identifiers, cadence/targets, discrepancy ownership and stop conditions depend on selected funds flow, provider evidence and finance/operations review. |
| AR-018 | Retain open | Support access, data classification, retention/deletion/legal hold, backup access/encryption and restore/replay policy need qualified privacy/security/operations decisions. |
| AR-019 | Keep closed for its limited scope | Indexing and cross-links were verified. Do not expand this closure to architecture or control effectiveness. |
| AR-020 | Retain open | Candidate deployment topology remains unselected; technical, operations/platform and independent security review plus cost/recovery rationale are required. |
| AR-021 | Retain open | Canonical idempotency scope, retention/tombstones, collision handling, provider guarantees and restore-resume authority remain unapproved and unverified. |

**Accountable disposition recorded — 2026-10-24:** the user approved the proposed handling of the 21 architecture findings. AR-001–AR-018 and AR-020–AR-021 are explicitly retained as open; AR-003, AR-007, AR-008 and AR-013 remain capability blockers. AR-019 remains closed only for its narrow index/cross-link task. This disposition preserves current evidence status; it does not close findings, approve architecture or change G1/G2. Required specialist reviews and evidence remain outstanding.

Stage A records preparation and validation of the remediation plan. The Stage B batch disposition decision is now recorded below; it does not pass G1/G2, authorize implementation or tests, or permit provider contact, credentials, live financial activity, merge or deployment.


### Reviewer appointment record — 2026-10-24

- **Accountable requirements reviewer:** User (self-designated in conversation on 2026-10-24).
- **Scope:** Review and decide DOC-34's 54 proposed requirement dispositions, including proposed amendments and rationale.
- **Assistant role:** Supporting traceability, consistency checks and evidence preparation; not the accountable reviewer or a substitute for specialist sign-off.
- **Current decision status:** The 54-ID batch decision was recorded on 2026-10-24 (51 Accept / 3 Defer). Cross-document reconciliation and required specialist reviews remain open; this decision alone does not pass G1 or G2.

## 3. Findings register

| ID | Priority | Finding | Required correction | Acceptance evidence | Gate |
|---|---|---|---|---|---|
| RR-001 | P0 | No executable application exists across the reviewed refs; feat/initial-foundation is only a scaffold. | Keep the documentation branch documentation-only until G1/G2 pass. After separate authorization, turn the foundation branch into a real, reviewable implementation with source structure, valid workspace manifests, lockfile, migrations, tests and CI. | Tree inventory; later foundation PR and reproducible CI evidence only after authorization. | G3 blocked |
| RR-002 | P0 | README truthfulness and environment-template availability differ across branches; the older `main` README overstates maturity and implies `.env.example` is broadly available, while the corrected working-branch README identifies the incomplete scaffold and branch-specific template. | Keep the stack explicitly proposed, describe app/package folders as placeholders, state that no working app/CI is verified, and document where the environment template exists. Bring the corrected README to `main` only through the reviewed PR #2 path after applicable gates. | Corrected README verified on the canonical and foundation branches; PR #2 carries the intended change; after approved integration, verify `main` has the corrected content. No direct `main` edit. | Repository integrity |
| RR-003 | P0 | main lacks the current discovery set, while DOC-00 on the documentation branch says DOC-04–DOC-23 are “Not started” despite later docs covering overlapping subjects. | Reconcile legacy IDs, then define the reviewed/approved path for bringing the current document set into main. Do not merge automatically; PR #2 remains draft pending gates/review. | DOC-00 maps each topic to one authority and branch integration is separately reviewed. | G0/G1 |
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
| RR-015 | P0 | Root scripts reference build/dev/lint/test/typecheck, but workspace folders contain no package manifests or executable source and no lockfile exists. | Add a package.json to each real workspace only when its intended runtime/toolchain is approved; define truthful scripts/dependencies and generate/commit a compatible pnpm lockfile under authorized engineering work. Do not claim scripts work until actual CI proves it. | Clean checkout install and each declared command succeed in a documented environment; actual CI evidence is linked. | G3 |
| RR-016 | P0 | No tests or CI workflows exist, so no automated quality or security status can be asserted. | After authorization, introduce CI in staged gates and ensure failures block merging; never fabricate passing checks or add tests that are claimed to run without execution. | Workflow files and successful run links with test logs; independent review of security-critical coverage. | G3+ |
| RR-017 | P0 | Branch drift is material: main has only README and DOC-47; PR #1 holds the foundation scaffold; PR #2 holds the expanded discovery documentation. Both PRs are draft/unmerged. | Keep the PRs separate. Define merge ordering and conflict resolution after gate decisions; do not merge the foundation or docs branch simply to synchronize files. Reconcile README and document ownership across branches. | Reviewed branch/PR plan with base/head, dependencies, gates and independent reviews; no accidental merge. | Governance/G1/G2 |
| RR-018 | P1 | The foundation .env.example includes local database credentials and API_PORT despite no runtime validation. | Label values local-development-only, validate configuration in the eventual app, use placeholders/secret manager for shared environments, and ensure no real credentials or production data enter Git. | Reviewed sample and future config validation; secret scan results after CI exists. | G3/security |

## 4. Strict execution sequence

### Stage A — Repository truth and document authority

1. **A-01:** Fix README truthfulness on all working branches (RR-002).
2. **A-02:** Reconcile DOC-00 index/authority map and the planned relationship of PR #2 to main (RR-003, RR-008, RR-017).
3. **A-03:** Remove stale PR-specific roadmap instructions from DOC-03 (RR-004).
4. **A-04:** Refresh DOC-32 scope, summary and gate status (RR-005).
5. **A-05:** Add dated supplemental DOC-47 review of DOC-50–DOC-61 (RR-006).
6. **A-06:** Run the manual consistency pass: IDs, counts, links, proposal/approval labels, and claims about tests or implementation (RR-012).
7. **A-07:** Record the foundation scaffold gaps and define package/lockfile/CI completion criteria without claiming current scripts work (RR-015, RR-016).
8. **A-08:** Record cross-branch integration/dependency order for PR #1 and PR #2; keep both unmerged until their independent acceptance conditions are met (RR-017).

**Stage A exit:** DOC-00, DOC-03, DOC-32 and DOC-47 agree on scope, authority, governance and gates. G1/G2 are not presumed passed.

### Stage B — Requirements and decisions

7. **B-01:** The accountable requirements reviewer recorded the 54-ID decision on 2026-10-24: 51 Accept and EXT-001–EXT-003 Defer. Keep the decision reconciled across DOC-00, DOC-31, DOC-34, DOC-39, DOC-46 and DOC-47; the decision is not implementation or verification evidence.
8. **B-02:** Completed on 2026-10-24: DOC-31, DOC-34 and DOC-46 have identical 32-ID P0 sets; DOC-39 supplemental requirement dispositions are reflected in the decision record. P0 closure evidence and gates remain open.
9. **B-03:** Status reconciliation completed on 2026-10-24: re-fetched DOC-47 confirms AR-001–AR-018 and AR-020–AR-021 remain open and AR-019 is closed only for verified indexing/cross-links; DEC-047 in DOC-00 records the accountable reviewer's approval. The targeted AR-008 audit is recorded in DOC-47 and this plan. Finding closure, evidence work, and specialist review remain open; no architecture approval or G1/G2 pass is implied.
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
- Requirement disposition decision is recorded: 51 Accept and EXT-001–EXT-003 Defer across the intended launch baseline; maintain the reconciliation record and preserve the 32 P0 requirements. The decision is not implementation or verification evidence.
- Provider multi-merchant authority, production access, product entitlements, funds-flow model, settlement/account ownership, money-out funding authority and method-specific finality evidence are not established here.
- Named technical, independent security, finance/accounting, operations/reconciliation and independent requirements reviewers remain unassigned unless formally recorded.
- No executable app source, tests, CI, migrations or runtime configuration were present on any reviewed ref; the foundation branch contains only root workspace configuration and placeholder READMEs. No software behavior was verified.

## 7. Completion and change-control rule

Update this plan only with dated, evidence-backed status changes. If a blocker remains, preserve it as BLOCKED and identify the exact decision/evidence needed. Never mark a stage complete because the plan exists. Never lower a safety requirement or remove a blocker solely to meet a deadline or make a gate appear passed.

**Final rule:** Correct repository truthfulness and document authority first, close owner/evidence decisions second, approve architecture third, and only then begin separately authorized engineering work.

## 8. Stage C execution log — 2026-10-24

| Action | Status | Evidence / remaining condition |
|---|---|---|
| C-01 — Record selected funds-flow model and model-specific financial/accounting rules | **Started; blocked — no model selected** | Added a preliminary evidence scorecard to DOC-28 §11, cross-checked with DOC-37/DOC-38, DOC-44 §4 and DOC-46. The existing evidence is not equivalent or sufficient to select Model A (provider-managed), Model B (direct), or Model C (conditional hybrid). Written multi-merchant permission, product-specific production entitlements, contracting/account/settlement roles, payout funding/prefunding/reserves, complete fees/limits/timing/support, and qualified Uganda legal/compliance and finance/accounting review remain absent. DEC-048 records the blocked status in DOC-00; DOC-44 and DOC-46 link the same outcome. G1 remains NOT PASSED; G2 remains NOT APPROVED. No provider contact, applications, credential access, implementation, tests, merge or deployment was performed or authorized. |
| C-01 next action | **Initial non-binding inquiries sent by the product owner; awaiting written replies** | On 2026-10-24, the user reported manually sending informational inquiries from their own Gmail to the listed official MTN Uganda and Airtel Uganda business/API contacts. Scope was limited to written guidance on multi-merchant eligibility, collections/production access, payouts and funding, settlement/reconciliation, fees/limits, and onboarding/legal/technical requirements. This is owner-reported send status, not proof of delivery, provider response, permission, or entitlement. No application, contract, credentials, sensitive documents, or financial action was submitted. Wait for written responses; if no evidence arrives, C-01 remains BLOCKED. No further outreach is authorized by this record. Do not advance C-02/C-03 as approvals or begin C-04/G3 while prerequisite gates remain unmet. |

This log records a preliminary internal evidence comparison only. It does not complete C-01, select a funds-flow model, pass G1/G2, appoint specialist reviewers, or authorize external engagement or engineering.


### Provider inquiry execution note — 2026-10-24

The product owner confirmed that the two non-binding inquiries were manually sent from their own Gmail account. The intended recipients were MTN Uganda (`talkbusiness.ug@mtn.com`, `apiconnect@mtn.com`) and Airtel Uganda (`AirtelBusiness@ug.airtel.com`). The inquiries intentionally omit the product name and request informational guidance only. This is a user-reported send confirmation; delivery, reply-to routing, provider receipt, and provider response have not been independently verified. No replies are recorded as of this note. This narrow authorization covers the initial informational inquiries only; follow-up outreach, applications, contracts, credential use, onboarding, production access, and live financial activity require separate explicit authorization.
