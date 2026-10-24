# DOC-62 — HARAI Repository-Wide Remediation and Verification Plan

**Status:** Draft corrective-action plan from a read-only repository inventory; not an implementation authorization.  
**Review date:** 2026-10-24  
**Reviewed ref:** docs/julypay-discovery-and-requirements  
**Review boundary:** Read-only inventory covered the three active working refs: main, feat/initial-foundation, and docs/julypay-discovery-and-requirements. main contains README.md and one DOC-47 file; the documentation branch contains README.md and 42 Markdown documents under docs/; the foundation branch contains a root package.json, pnpm-workspace.yaml, turbo.json, .gitignore, .env.example, app/package placeholder READMEs and two short docs. Across these refs there is no actual application source, no workspace-level package.json files, no lockfile, no tests, no CI workflow, no database schema/migrations, and no deployment configuration. This is a cross-branch repository inventory and readiness assessment—not a source-code audit, security test, provider verification, or proof that software behavior works.

**Gate status:** G1 requirements approval NOT PASSED. G2 architecture/security approval NOT APPROVED. Only the initial non-binding informational inquiries recorded in DEC-049/DOC-62 §8 were authorized and reported sent; this document authorizes no further provider contact, implementation, tests, credential generation, live financial activity, merge, or deployment.

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
| C-01 — Record selected funds-flow model and model-specific financial/accounting rules | **Started; blocked — no model selected** | Added a preliminary evidence scorecard to DOC-28 §11, cross-checked with DOC-37/DOC-38, DOC-44 §4 and DOC-46. The existing evidence is not equivalent or sufficient to select Model A (provider-managed), Model B (direct), or Model C (conditional hybrid). Written multi-merchant permission, product-specific production entitlements, contracting/account/settlement roles, payout funding/prefunding/reserves, complete fees/limits/timing/support, and qualified Uganda legal/compliance and finance/accounting review remain absent. DEC-048 records the blocked status in DOC-00; DOC-44 and DOC-46 link the same outcome. G1 remains NOT PASSED; G2 remains NOT APPROVED. At the time this preliminary scorecard was first recorded, no provider contact had occurred. The later owner-reported initial informational inquiries are recorded in the C-01 next-action row and the Provider inquiry execution note below. No applications, credential access, implementation, tests, merge or deployment were performed or authorized. |
| C-01 next action | **Initial non-binding inquiries sent by the product owner; awaiting written replies** | On 2026-10-24, the user reported manually sending informational inquiries from their own Gmail to the listed official MTN Uganda and Airtel Uganda business/API contacts. Scope was limited to written guidance on multi-merchant eligibility, collections/production access, payouts and funding, settlement/reconciliation, fees/limits, and onboarding/legal/technical requirements. This is owner-reported send status, not proof of delivery, provider response, permission, or entitlement. No application, contract, credentials, sensitive documents, or financial action was submitted. Wait for written responses; if no evidence arrives, C-01 remains BLOCKED. No further outreach is authorized by this record. Do not advance C-02/C-03 as approvals or begin C-04/G3 while prerequisite gates remain unmet. |

### C-01 internal chronology consistency check — 2026-10-24

**Completed — documentation-only.** Reconciled the inquiry timeline across DOC-00 (DEC-022/DEC-049 and DOC-29 index), DOC-29, DOC-37 and DOC-38. The records now distinguish the 2026-10-19 read-only portal inspection (no provider contact during that inspection) from the product owner's later report that the initial non-binding inquiries were manually sent on 2026-10-24. Delivery, provider receipt, reply-to routing and replies remain unverified. The inquiry template is explicitly distinguished from the exact messages sent; further outreach remains separately gated. Relevant files were re-fetched after edits. No provider reply or permission was inferred.

This log records internal evidence comparison and chronology reconciliation only. It does not complete C-01, select a funds-flow model, pass G1/G2, appoint specialist reviewers, or authorize further external engagement or engineering.


### Provider inquiry execution note — 2026-10-24

The product owner confirmed that the two non-binding inquiries were manually sent from their own Gmail account. The intended recipients were MTN Uganda (`talkbusiness.ug@mtn.com`, `apiconnect@mtn.com`) and Airtel Uganda (`AirtelBusiness@ug.airtel.com`). The inquiries intentionally omit the product name and request informational guidance only. This is a user-reported send confirmation; delivery, reply-to routing, provider receipt, and provider response have not been independently verified. No replies are recorded as of this note. This narrow authorization covers the initial informational inquiries only; follow-up outreach, applications, contracts, credential use, onboarding, production access, and live financial activity require separate explicit authorization.

### Requirement-to-design/evidence traceability audit — 2026-10-24

**Result: identifier/disposition consistency passed; full requirement-to-design/evidence traceability remains OPEN.** Read-only review of DOC-31 and DOC-34 confirmed 54 unique requirement IDs and the same 32-ID P0 set represented by DOC-46. In DOC-34's primary requirement matrix, all 44 original rows have proposed test IDs and role-level owners, but the owner names are explicitly unassigned and the test IDs are proposals only. Forty of those 44 dependency cells do not cite a specific DOC/requirement ID, so the link from normative acceptance criteria to a specific design/source artifact is not explicit in the row. The ten supplemental rows must be assessed under the same standard. This is a traceability gap, not evidence that the referenced design work is absent; it requires row-level verification against the actual documents before claims of complete mapping.

**Next action:** prepare a row-level crosswalk for all 54 IDs with (1) normative DOC-31 criterion, (2) specific design/source document and section or an explicit `MISSING` marker, (3) proposed test ID(s), (4) evidence artifact state (`NOT CREATED`, `BLOCKED`, or a verified reference), (5) accountable role and named-person assignment state, and (6) required reviewer/gate. Do not invent links, people, tests or evidence. Preserve all accepted dispositions and the 32 P0 IDs. G1 remains NOT PASSED; G2 remains NOT APPROVED. No tests, implementation, provider contact, credentials, financial activity, merge or deployment were performed or authorized.


### Requirement-to-design/evidence crosswalk created — 2026-10-24

**Documentation artifact added:** DOC-63, `docs/63-requirement-design-evidence-crosswalk.md`, commit `7387bce50d7f07da8ac29d98c971c38f8b3f098f` (with a follow-up source-section correction in commit `b575a89884b54c7e4e14eed4a00a9c4869250390`). The crosswalk contains 54 unique requirement IDs: the 44 original rows from DOC-34's primary matrix plus SUP-001–SUP-010. Each row points to the normative DOC-31 requirement ID/section, records candidate design/source references or an explicit `MISSING` marker, preserves DOC-34 proposed test IDs, states the evidence gap/blocker, and records proposed role-level accountability and reviewer/gate requirements. The document labels source references as candidates pending content verification; it does not claim all links are complete or correct.

**Scope/gate check:** 51 Accept / 3 Defer (EXT-001–EXT-003) unchanged; all 32 P0 IDs retained; G1 NOT PASSED; G2 NOT APPROVED. Proposed test IDs are not executable evidence, role labels are not named appointments, and no requirement is marked implemented or verified by DOC-63. The next action is to verify each candidate source reference against the actual source section, resolve every `MISSING` entry, and then reconcile DOC-00/DOC-62 only on the basis of the inspected results. No tests, implementation, provider outreach, credentials, live financial activity, merge or deployment were performed or authorized.


### DOC-63 structural crosswalk verification — 2026-10-24

**Verified on canonical branch after re-fetch:** DOC-63 contains 54 table rows and 54 unique IDs, with no duplicate IDs; the 32 P0 IDs are all represented in DOC-46; the disposition count remains 51 Accept and 3 Defer (EXT-001–EXT-003). DOC-63 is indexed in DOC-00. Nine explicit `MISSING` markers remain in candidate source mapping notes and must not be treated as resolved. The check confirms structural coverage and disposition consistency only; it does not validate every candidate source section's substantive sufficiency, create evidence, appoint named people, execute tests, or pass G1/G2. The crosswalk itself labels design/source references as candidates pending row-by-row content verification. Next: verify each cited section against its actual content, close or preserve each missing link explicitly, and record evidence/reviewer assignments only when attributable records exist.


### DOC-63 section-level source review — 2026-10-24

**Result: candidate references refined; substantive traceability remains OPEN.** Re-fetched the canonical DOC-63 and inspected the actual source headings and selected relevant passages in DOC-40, DOC-41, DOC-42, DOC-43, DOC-44, DOC-46, DOC-48, DOC-49, DOC-50, DOC-51, DOC-56, DOC-57, DOC-58 and DOC-59. DOC-63 now records section-level candidate references for all 54 rows rather than relying only on broad document numbers. Reviewed content includes: DOC-41's proposed canonical states, unknown outcomes, separate operation families and no-blind-retry rules; DOC-42's append-only balanced postings, exact amount representation, fee/settlement separation, reconciliation layers and correction cases; DOC-43's tenant, callback, replay, rate-abuse and high-risk destination threat cases; DOC-48's provider capability boundary and operation-family rules; DOC-49's API idempotency, error, status and outbound-webhook proposals; DOC-50's capability-level activation gates; DOC-51's tenant isolation, least privilege and separation-of-duties controls; DOC-56's data ownership/integrity invariants; DOC-57's end-to-end flows, recovery and observability; and DOC-59's credential, tenant, IP-allowlist and abuse controls.

**Important limitation:** These are draft proposals and candidate mappings, not approved architecture or proof of implementation. Section references show relevant material exists, but they do not prove that every acceptance criterion is fully satisfied or free of contradiction. Provider-specific finality, production entitlements, funds-flow, fees/settlement, funding, numeric operating targets, qualified specialist reviews, named owners/reviewers and execution evidence remain open. DOC-63 retains explicit `MISSING` entries for FIN-004 and SUP-005 because no specific disputes/complaints case lifecycle, ownership and deadlines are confirmed. The DOC-30 source path was subsequently verified as `docs/30-uganda-compliance-and-funds-flow-review.md`; DOC-63 now cites §3.A and §9 for the regional-expansion professional-review dependency. That reference does not mean any country-specific review has occurred. The 54-ID coverage, 32 P0 set and 51 Accept / 3 Defer disposition are unchanged. G1 NOT PASSED; G2 NOT APPROVED. No tests, implementation, provider contact, credentials, financial activity, merge or deployment were performed or authorized.

**Next action:** compare each mapped section against the full normative criterion and record any partial/missing/contradictory coverage without inferring approval. Then reconcile DOC-63, DOC-34, DOC-46 and DOC-47 based on actual findings.


### DOC-63 referenced-document existence check — 2026-10-24

**Structural check passed.** Compared every DOC-number reference in DOC-63 against the canonical branch's `docs/` directory listing. All referenced document numbers have a corresponding document file; no missing document-number references were found. This is only a document-existence check: it does not prove that each cited section fully supports the mapped acceptance criterion. DOC-63 contains 54 unique rows, 51 Accept / 3 Defer, and two explicit criterion/design gaps (FIN-004 and SUP-005) for dispute/complaint case handling. Next work remains criterion-by-criterion content adequacy and contradiction review, with gaps preserved until an actual source section or approved design closes them.


### DOC-63 preliminary P0 criterion-coverage pass — 2026-10-24

**Result: all 54 rows now carry a preliminary content-coverage status, with P0 rows assessed first and the remaining 22 rows subsequently reviewed at a preliminary level.** DOC-63 distinguishes candidate source references from preliminary content-coverage assessments; all rows still require deeper criterion-by-criterion verification and contradiction review. The P0 assessment identifies several design concepts already present as proposals (tenant boundaries, lifecycle/unknown outcomes, idempotency, append-only financial records, reconciliation, callback/webhook security, merchant activation, access control and release gates), while preserving unresolved gaps: provider-specific finality and production authority; approved funds-flow, fee/settlement and funding rules; numeric rate limits and SLO/RTO/RPO; session/MFA and retention/deletion policy decisions; named runbook/reconciliation owners and independent reviewers; executable tests/evidence; and complete dispute/complaint case handling. FIN-004 and SUP-005 remain explicitly MISSING for a specific dispute/complaint lifecycle, ownership and deadlines. These are preliminary internal assessments, not independent review, acceptance or gate passage.

**Structural verification after update:** DOC-63 has 54 unique requirement rows; all 54 carry a preliminary content-coverage status, including all 32 P0 rows. The disposition remains 51 Accept / 3 Defer (EXT-001–EXT-003). G1 NOT PASSED; G2 NOT APPROVED. No tests, implementation, provider contact, credentials, financial activity, merge or deployment were performed or authorized.

**Next action:** deepen the criterion-by-criterion review across all 54 rows, validate each section's substantive sufficiency and contradiction status, and revisit preliminary classifications where full-section reading changes the result. Do not close findings merely because a design concept is mentioned in a draft.


### DOC-63 detailed criterion review — Batch 1 (2026-10-24)

**Completed — documentation-only, bounded review.** Compared the full normative DOC-31 criteria for GOV-001, TEN-001, IAM-002, COL-003–COL-005, OUT-002, FIN-002, REC-001, REC-002 and FIN-005 with relevant sections of DOC-00, DOC-41, DOC-42, DOC-43, DOC-44, DOC-48, DOC-49, DOC-51, DOC-56, DOC-57 and DOC-59. Refined six DOC-63 source mappings: TEN-001 now explicitly points to DOC-56 §15; COL-003 to DOC-42 §7.2 and DOC-49 §7.1; FIN-002 to DOC-42 §7.2 and DOC-57 §§10, 12; REC-001/REC-002 to the import-integrity and discrepancy-resolution sections; FIN-005 to DOC-42 §8.1 and DOC-48 §§4–5.

**Finding:** The inspected source sections contain relevant draft design proposals, but they do not establish complete criterion satisfaction. Material open conditions include named decision/operation owners and independent reviewers; provider-specific finality and retry guarantees; authoritative provider transaction/settlement evidence; fully approved import-integrity and discrepancy-resolution rules; accountable operational thresholds/escalation; and implementation/negative-test evidence. DOC-41/42/48/49 defer to the shared idempotency/identity proposal in DOC-42 §7.2; DOC-56 §15, DOC-57 §20 and DOC-59 §16 align to the shared tenant-isolation direction. No direct contradiction was established in these inspected sections, but this is not a repository-wide contradiction clearance. No coverage label was upgraded to verified/closed.

**Structural/scope boundary:** 54 unique IDs, all 32 P0 IDs, and the approved 51 Accept / 3 Defer split (EXT-001–EXT-003) remain unchanged. G1 NOT PASSED; G2 NOT APPROVED. No tests, implementation, provider outreach, credentials, financial activity, merge or deployment were performed or authorized.

**Next action:** Continue with the next criterion batch, inspect the exact cited sections against full DOC-31 wording, and record only evidence-supported mapping/status changes. Keep the crosswalk preliminary until all 54 rows have received detailed review.


### DOC-63 detailed criterion review — Batch 2 (2026-10-24)

**Completed — documentation-only, bounded review.** Compared the full DOC-31 criteria for MER-001, MER-002, IAM-001, IAM-002, MER-003 and MER-004 against DOC-42, DOC-44, DOC-46, DOC-48, DOC-50, DOC-51 and DOC-59. Refined six DOC-63 mappings to include role/permission/audit sources, provider capability evidence, credential scope/revocation, contradiction-resolution controls, privacy/retention and the explicit FIN-004/SUP-005 dispute/complaint gap.

**Finding:** Onboarding and access-control designs remain proposals. Exact lifecycle transition permissions and audit-event completeness are not independently verified; live activation is blocked pending provider authority/capability evidence; MFA/session/revocation parameters and sensitive-action limits remain open; privacy/retention/expiry/reverification decisions remain open; and merchant suspension/closure cannot yet demonstrate complete handling for pending transactions, disputes, complaints and settlements. DOC-50's optional sandbox path is consistent with DOC-48's separate provider-entitlement gate. No row was upgraded to verified/closed.

**Scope/gate boundary:** 54 unique IDs, all 32 P0 IDs, and 51 Accept / 3 Defer remain unchanged. G1 NOT PASSED; G2 NOT APPROVED. No tests, implementation, provider outreach, credentials, financial activity, merge or deployment were performed or authorized.


### DOC-63 detailed criterion review — Batch 3 (2026-10-24)

**Completed — documentation-only, bounded review.** Compared COL-001, COL-002, COL-006, COL-007 and SUP-002 against DOC-41, DOC-48, DOC-49, DOC-52, DOC-57 and DOC-58. Expanded DOC-63's source mappings to cover request validation/idempotency/errors, provider capability gates, outbound event delivery, link/session lifecycle, amount/currency validation and concurrency behavior.

**Finding:** Relevant behavior is described only in draft contracts and UX/data-flow proposals. Final schemas and method/currency allowlists, hosted-checkout journey and provider-specific availability, notification delivery/outcome semantics and templates, and payment-link expiry/cancellation/usage/concurrency rules remain unapproved or unverified. Production-first onboarding and optional sandbox do not imply provider production eligibility. No row was upgraded to verified/closed; no direct contradiction was established in the inspected sections.

**Scope/gate boundary:** 54 unique IDs, all 32 P0 IDs, and 51 Accept / 3 Defer remain unchanged. G1 NOT PASSED; G2 NOT APPROVED. No tests, implementation, provider outreach, credentials, financial activity, merge or deployment were performed or authorized.


### DOC-63 detailed criterion review — Batch 4 (2026-10-24)

**Completed — documentation-only, bounded review.** Reviewed OUT-001, OUT-003, OUT-004, OUT-006 and FIN-003, with adjacent FIN-001/OUT-005 mapping consistency, against DOC-28, DOC-30, DOC-41, DOC-42, DOC-48, DOC-50, DOC-51 and DOC-59. Expanded mappings for money-out activation, linked refund/reversal accounting patterns, beneficiary credential/approval controls, and qualified Uganda review/provider evidence dependencies.

**Finding:** Payouts, refunds and reversals/returns remain externally blocked by missing permission/capability evidence, unselected funds-flow/funding/settlement authority and required finance/accounting/legal review. Proposed linked-operation/compensating-entry patterns do not prove provider support or approved accounting treatment. Beneficiary changes remain partial/blocked pending identity, risk, cooling-off/approval and pending-payout rules. FIN-003 remains blocked without an approved fee schedule, fee-payer, rounding/tax treatment and accounting sign-off. No row was upgraded to verified/closed; no direct contradiction was established in this bounded batch.

**Scope/gate boundary:** 54 unique IDs, all 32 P0 IDs, and 51 Accept / 3 Defer remain unchanged. G1 NOT PASSED; G2 NOT APPROVED. No tests, implementation, provider outreach, credentials, financial activity, merge or deployment were performed or authorized.


### DOC-63 detailed criterion review — Batch 5 (2026-10-24)

**Completed — documentation-only, bounded review.** Reviewed DEV-001, DEV-002, SEC-001, SEC-002, OPS-001, OPS-002 and OPS-003 against DOC-03, DOC-40, DOC-42, DOC-43, DOC-44, DOC-45, DOC-48, DOC-49, DOC-50, DOC-51, DOC-56, DOC-57 and DOC-59. Refined mappings for credential lifecycle/compromise response, webhook destination security, account recovery, privacy/backup controls, restore-resume controls, release gates and reliability objectives.

**Finding:** Security and operational controls remain proposals and have no attributable verification evidence. Credential/session/MFA policy, retention/deletion/legal hold, backup/key handling, outbound destination protections, named runbook owners, numeric SLO/RTO/RPO, provider support/SLA and restore-resume authorization remain unresolved. DOC-49 and DOC-40/DOC-48 are directionally compatible on webhook security, but the canonical signing/destination/retry policy remains open. No row was upgraded to verified/closed; no direct contradiction was established in this bounded batch.

**Scope/gate boundary:** 54 unique IDs, all 32 P0 IDs, and 51 Accept / 3 Defer remain unchanged. G1 NOT PASSED; G2 NOT APPROVED. No tests, implementation, provider outreach, credentials, financial activity, merge or deployment were performed or authorized.


### DOC-63 detailed criterion review — Batch 6 (2026-10-24)

**Completed — documentation-only, bounded review.** Reviewed REP-001, DEV-003, SEC-003, OPS-004 and EXT-004 against DOC-28, DOC-30, DOC-40, DOC-43, DOC-45, DOC-48, DOC-49, DOC-51, DOC-52, DOC-53, DOC-55, DOC-56, DOC-57 and DOC-59. Refined mappings for tenant-safe report/export dispatch, API compatibility/error semantics, trusted proxy/IP policy, accessibility evidence and card-provider/legal prerequisites.

**Finding:** Report/export reproducibility and dispatch-time tenant enforcement remain unverified; API compatibility rules remain draft without final schemas/tests; network trust and allowlist controls remain unverified against deployment settings; accessibility criteria exist but manual keyboard/screen-reader review is absent; card processing remains blocked pending processor selection, written eligibility, security/PCI scope and qualified review. No row was upgraded to verified/closed, and no direct contradiction was established in this bounded batch.

**Scope/gate boundary:** 54 unique IDs, all 32 P0 IDs, and 51 Accept / 3 Defer remain unchanged. G1 NOT PASSED; G2 NOT APPROVED. No tests, implementation, provider outreach, credentials, financial activity, merge or deployment were performed or authorized.


### DOC-63 detailed criterion review — Batch 7 (2026-10-24)

**Completed — documentation-only, bounded review.** Reviewed SUP-001, SUP-003, SUP-004, SUP-006, SUP-007, SUP-009 and SUP-010 against the full DOC-31 §8 criteria and relevant API, state, security, accessibility and audit proposals. SUP-002 was reviewed in Batch 3; SUP-005 remains explicitly missing; SUP-008 was reviewed with OPS-003 in Batch 5. DOC-63 mappings now point to the more specific rate-limit, beneficiary, shared identity/state, API compatibility, trusted-proxy, accessibility-evidence and audit/correlation sections.

**Finding:** Numeric rate-limit/recovery limits, beneficiary cooling-off/approval and money-out authority, provider-specific state/finality mapping, final API compatibility contracts, deployment-specific edge trust configuration, manual accessibility review, and end-to-end audit/retention evidence remain open. No row was upgraded to verified/closed; no direct contradiction was established in the bounded sections.

**Scope/gate boundary:** 54 unique IDs, all 32 P0 IDs, and 51 Accept / 3 Defer remain unchanged. G1 NOT PASSED; G2 NOT APPROVED. No tests, implementation, provider outreach, credentials, financial activity, merge or deployment were performed or authorized.


### DOC-63 detailed criterion review — Batch 8 (2026-10-24)

**Completed — documentation-only, bounded review.** Reviewed COL-008, OUT-005, FIN-001, FIN-004, SUP-005 and deferred EXT-001–EXT-003 against DOC-28, DOC-30, DOC-31, DOC-40, DOC-42, DOC-45, DOC-48, DOC-49, DOC-56, DOC-57 and DOC-59. Refined rate-limit, audit evidence, currency/provider capability, case-handling gap and conditional future-scope references.

**Finding:** Numeric API limits/recovery budgets, approved currency precision, financial audit retention/authority and attributable evidence remain open. FIN-004 and SUP-005 remain explicit MISSING design gaps: reconciliation exceptions do not constitute a full dispute/complaint case lifecycle with owner, deadlines, communications, escalation, retention and outcome. EXT-001–EXT-003 remain deferred and not ready for reconsideration without method/country-specific evidence and review. No row was upgraded to verified/closed; no direct contradiction was established in this bounded batch.

**Scope/gate boundary:** 54 unique IDs, all 32 P0 IDs, and 51 Accept / 3 Defer remain unchanged. G1 NOT PASSED; G2 NOT APPROVED. No tests, implementation, provider outreach, credentials, financial activity, merge or deployment were performed or authorized.


### DOC-63 final criterion review and architecture-register reconciliation — 2026-10-24

**Completed — documentation-only.** The detailed criterion review covered all 54 unique DOC-31 requirement IDs in eight recorded batches. Structural verification confirmed DOC-31, DOC-34 and DOC-63 have the same 54-ID set; all 32 P0 rows remain present; dispositions remain 51 Accept and 3 Defer (EXT-001–EXT-003). DOC-63 is indexed in DOC-00, and every row has a preliminary content-coverage status. The two explicit design gaps FIN-004 and SUP-005 remain MISSING; draft design references and planned test IDs are not verification evidence.

**Architecture finding added:** AR-022 (P1, open) was added to DOC-47 because the accepted FIN-004/SUP-005 criteria have no complete dispute/complaint case lifecycle. DOC-00, DOC-34, DOC-46, DOC-47 and DOC-63 now cross-reference the gap. Reconciliation exception handling is not equivalent to complaint/dispute case management. Closure requires a defined lifecycle, transaction/evidence linkage, owner, deadlines/escalation, communications, access controls, retention/outcome rules and relevant support/compliance/finance review.

**Reconciliation status:** DOC-47 now contains 22 findings (16 P0, 6 P1); AR-019 remains closed only for its narrow index/cross-link verification task. AR-022 is open and newly recorded; no prior finding was silently closed. G1 remains NOT PASSED; G2 remains NOT APPROVED. No tests, implementation, provider outreach, credentials, financial activity, merge or deployment were performed or authorized.

**Next action:** Continue with the remaining evidence and architecture-decision closure work in DOC-62 Stage B/C only as evidence arrives and approvals are recorded. Do not treat the completed documentation audit as permission to begin Stage C-04/G3 or engineering.


### DOC-64 case-management proposal — 2026-10-24

Created `docs/64-dispute-and-transaction-complaint-case-management-design-proposal.md` as a draft proposal for accepted requirements FIN-004 and SUP-005. It defines a candidate case lifecycle, ownership, deadline provenance, escalation, evidence and communication history, tenant-scoped access, resolution/reopen behavior, separation from financial operations, conceptual entities and 15 proposed acceptance criteria. No provider/legal deadlines, named destinations, retention periods or accounting rules were invented. The proposal requires support/operations, provider, qualified Uganda legal/compliance, finance/accounting, security/privacy and technical review as applicable. It is not approved, implemented or verified. AR-022 remains open; G1 NOT PASSED; G2 NOT APPROVED. No tests, implementation, provider outreach, credentials, financial activity, merge or deployment occurred.

### DOC-64 proposal review and cross-document reconciliation — 2026-10-24

Created DOC-65 as an internal review matrix comparing DOC-64 with the normative FIN-004/SUP-005 criteria in DOC-31 and related controls. DOC-64 is a useful draft design input but remains preliminary partial: provider-specific duties/entitlements and legal deadlines, approved category/transition rules, named owners and escalation destinations, retention/privacy policy, finance/accounting treatment, specialist reviews and execution evidence remain open. DOC-63 now reflects preliminary partial design coverage for FIN-004/SUP-005 while retaining BLOCKED dependencies and execution evidence NOT CREATED. DOC-34's narrative and DOC-47 AR-022 now acknowledge the draft proposal without closing the finding. DOC-00 indexes DOC-65. Requirement dispositions remain 54 unique IDs, 51 Accept / 3 Defer (EXT-001–EXT-003), and 32 P0; no requirement was verified/closed. AR-022 remains open; G1 NOT PASSED; G2 NOT APPROVED. No tests, implementation, provider outreach, credential use, financial activity, merge or deployment occurred or was authorized.


### Specialist research programme and source register — 2026-10-24

Created DOC-66 to begin a source-led research programme across provider-specific obligations, Uganda payment-system law and consumer protection, deadline provenance, accountable routing, privacy/retention/security, and finance/accounting. It records primary sources, their scope limits, preliminary observations, a reading/review sequence, and nine outstanding evidence items. The document explicitly does not claim professional qualification or substitute for qualified Ugandan counsel, a practising accountant, provider-specific written evidence, or independent review. DOC-00 indexes DOC-66. DOC-64 remains a draft proposal; AR-022 open; G1 NOT PASSED; G2 NOT APPROVED. Reading/research and documentation only; no provider outreach, tests, implementation, credentials, financial activity, merge or deployment authorized.

### Preliminary specialist obligation and evidence matrix — 2026-10-24

Created DOC-67 to translate the first source-validation pass into a clause-scoped matrix covering the National Payment Systems Act, DPPA, BoU source gaps, MTN public rules, Airtel evidence gaps, deadlines, operational routing, PCI scope and accounting review. The matrix separates verified public-source text from applicability questions and unverified provider/account-specific terms. It expressly prohibits treating consumer reversal rules as HARAI API deadlines, inventing retention periods or operational routes, and finalizing accounting treatment without actual contracts/funds flow and a practising accountant's review. DOC-00 indexes DOC-67. Next research step: retrieve and clause-review the current BoU Consumer Protection Guidelines, 2025 oversight framework, January 2024 PSP/PSO financial-reporting guidelines and full current NPS/DPPA regulations. DOC-64 remains a draft; AR-022 open; G1 NOT PASSED; G2 NOT APPROVED. Documentation/research only; no follow-up provider outreach, tests, implementation, credentials, financial activity, merge or deployment authorized.


### NPS consumer-protection source validation — 2026-10-24

Further source review identified the National Payment Systems (Consumer Protection) Regulations, 2022 (S.I. 103 of 2022), including reg. 6's requirement for a covered licensee to submit categorized complaint information and remedial measures to BoU within 15 days after each month-end. This is a conditional regulatory-reporting deadline, not a general consumer complaint response/resolution SLA; applicability depends on HARAI's legal/licensing role and must be confirmed by qualified Ugandan counsel. The 2021 NPS Regulations' ULII page also notes that amendments remain unapplied in its consolidated view, so the 2022 NPS Amendment Regulations must be reconciled before relying on the 2021 text as current. DOC-66 records these source additions; DOC-67 records the conditional deadline and evidence requirements. G1 remains NOT PASSED; G2 remains NOT APPROVED; AR-022 remains OPEN. Research/documentation only; no regulator/provider contact, tests, implementation, credentials, financial activity, merge or deployment authorized.


### Bank of Uganda 2025 framework validation — 2026-10-24

Reviewed the official August 2025 National Payment Systems Oversight Policy Framework. It distinguishes PSP and PSO activity, includes aggregation/enabling technology in the PSO description, says covered PSPs/PSOs require licensing, calls for explicit approval for certain additional PSP services/product partnerships, and states material operational responsibility is retained even when third parties are contracted. It lists domestic payment gateway services and merchant acquisition in the eligible payment activity annex. These findings reinforce that HARAI's actual activities/funds flow need a qualified Ugandan legal classification before the product role is selected. The detailed BoU Consumer Protection Guidelines PDF and January 2024 PSP/PSO financial reporting/external auditor guideline PDF were not retrieved for clause-by-clause review; their details remain unverified. DOC-67 now records the findings and limits. G1 NOT PASSED; G2 NOT APPROVED; AR-022 OPEN. Research/documentation only; no regulator/provider outreach, tests, implementation, credentials, financial activity, merge or deployment authorized.


## Research log — second specialist source pass (2026-10-24)

- Added DOC-67 §§12–14: PDPO annual data-protection compliance report as a conditional obligation; verified scope of the 2022 amendment to NPS Regulations; clearly enumerated source-retrieval gaps.
- Created DOC-68 as a question pack for qualified Ugandan payments counsel, privacy counsel/DPO, practising accountant and later provider-contract review. It is for internal preparation only; it does not authorize external outreach.
- Primary-source findings now distinguish the NPS monthly complaint report (15 days after month end for covered licensees) from the PDPO annual compliance report (current public notice lists 23 November 2026 for covered registered data organisations). Neither is assumed to apply to HARAI until legal/entity status is confirmed.
- Verified the published 2021 NPS Regulations and 2022 amendment: the identified 2022 instrument substitutes Schedules 3 and 4; ULII still flags outstanding amendments. Counsel must complete the current-law reconciliation.
- Status correction after later source retrieval: the full 2011 BoU Financial Consumer Protection Guidelines PDF and the January 2024 PSP/PSO Financial Reporting & External Auditor Guidelines PDF were subsequently reviewed; see the later dated research-log entries and DOC-67 §§15–16. The November 2023 financial reporting version remains unlocated for comparison. No claims of HARAI-specific legal classification or compliance are made.
- Gates unchanged: AR-022 OPEN; G1 NOT PASSED; G2 NOT APPROVED; funds-flow model unselected. No tests, implementation, provider/regulator outreach, credentials, reporting submission, financial activity, merge or deployment authorized.


## Research log — BoU consumer-protection guideline text located (2026-10-24)

- Located and reviewed the full 18-page 2011 BoU Financial Consumer Protection Guidelines PDF hosted on the legacy BoU website. Captured para. 9 complaint handling: documented procedures, prompt written acknowledgement except accepted resolution within 3 business days, final response by end of two weeks with a defined consumer-delay exclusion, six-monthly BoU reporting due within one month after each half-year, and systemic root-cause remediation.
- Recorded a material applicability caveat: the guideline defines covered financial services provider categories (banks, credit institutions, MDIs, forex bureaus and money-remittance companies). It must not automatically be treated as directly applicable to a standalone payment gateway/PSP/PSO or as a provider SLA without counsel's current-law review.
- Added a mandatory reconciliation question: 2011 six-monthly reporting versus 2022 NPS Consumer Protection Regulations monthly reporting. No assumption that one replaces the other.
- Located the 10-page January 2024 BoU Financial Reporting & External Auditor Guidelines PDF URL, but text extraction returned no body and download into the local analysis environment failed. Marked clauses as not reviewed; comparison with November 2023 remains open.
- Gates remain unchanged: AR-022 OPEN; G1 NOT PASSED; G2 NOT APPROVED; no implementation, tests, provider/regulator outreach, reporting submission, credentials, live financial activity, merge or deployment authorized.


## Research log — January 2024 financial reporting guidelines reviewed (2026-10-24)

- Visually reviewed all rendered pages of the 10-page January 2024 BoU Guidelines on Financial Reporting and External Auditors for PSPs, PSOs and Issuers of Payment Instruments.
- Confirmed the document states commencement 1 December 2023 and applies to BoU-regulated PSPs/PSOs/issuers; quarterly unverified statements due day 15 after quarter-end and external-auditor-verified statements by day 30; annual IFRS/ICPAU statements with January–December year, draft audited statements/management letter by 31 March, BoU approval before publication, extracts published by 30 April and website publication; auditor prequalification/approval and quarterly review requirements. See DOC-67 §16.
- At the time of this earlier log entry, the November 2023 PDF had not yet been obtained. The later log records its retrieval and comparison; the previously missing printed page 5 has now been rendered and matched against January page 5. The comparison remains targeted, not a complete textual redline. Do not claim supersession. All obligations remain conditional on HARAI's legal category and current licence/coverage status.
- Historical next-step note superseded by the later research log below: both versions were retrieved and compared. Remaining work is qualified confirmation of controlling version/applicability and mapping the actual funds-flow to reporting-template categories.
- Gates unchanged: AR-022 OPEN; G1 NOT PASSED; G2 NOT APPROVED; no implementation, tests, provider/regulator outreach, reporting submission, credentials, live financial activity, merge or deployment authorized.


## Research log — BoU guideline comparison and targeted DPPA review (2026-10-24)

- Retrieved both official BoU financial-reporting PDFs from the current BoU website: November 2023 (11 pages) and January 2024 (10 pages). Both state commencement 1 December 2023 and the same regulated-entity categories, but their visible rendered text differs.
- Recorded in DOC-67 §17.1: annual newspaper publication wording changes from “at least two” in November to “a newspaper” in January; January expressly includes website display of audited statements; the clause 9(a) annual auditor package and clause 7(a) frameworks are worded differently. The formerly missing November printed page 5 has now been inspected and contains the same annual bid/prequalification and auditor-approval controls visible on January page 5, so these are not confirmed January additions. No formal repeal/supersession statement was found; controlling version remains UNVERIFIED pending qualified confirmation and any needed full redline.
- Reviewed targeted provisions of the official DPPA Regulations 2021 PDF and PDPO annual-report guidance. DOC-67 §17.2 records registration (regs. 13–15), security practices/measures (31–32), immediate breach notification and Form 7 information (33), data-subject complaint handling (40–41), cross-border processing (DPPA s.19/reg. 30), retention/deletion (DPPA s.18), and annual PDPO report (reg. 50; 90 days after Government FY).
- Reconciled three separate reporting regimes in DOC-67 §17.3: 2011 BoU guideline six-monthly report (para. 9(8), conditional scope/current status); 2022 NPS Consumer Protection Regulations monthly report within 15 days after month-end (reg. 6); and DPPA Regulations annual PDPO report within 90 days after Government FY (reg. 50). No assumption that they replace one another or establish the same customer-resolution SLA.
- Updated DOC-68 with the outstanding qualified counsel/accountant/privacy reviewer questions and evidence acceptance criteria.
- Scope limitation: this was a targeted document review based on official PDFs, rendered pages and official PDPO guidance, not a formal legal opinion, exhaustive clause-by-clause audit of every provision, or confirmation of HARAI's legal/regulatory/data-protection status. HARAI's role, licensing, provider contracts, data locations and PDPO registration remain unverified.
- Gates unchanged: DOC-64 remains draft; DOC-65 preliminary; AR-022 OPEN; FIN-004/SUP-005 unverified; G1 NOT PASSED; G2 NOT APPROVED. No implementation, tests, provider/regulator outreach, filing/submission, credential use, live financial activity, merge or deployment authorized.


- Follow-up targeted privacy pass added to DOC-66 §17 and DOC-67 §17.2: DPPA ss. 12–15 on lawful/specific purpose, pre-collection notice, minimization and accuracy; DPPA s.16 / Regulations reg. 29 on correction/deletion requests, including a written decision within seven days under reg. 29(2); and registration lifecycle provisions in Regulations regs. 15–27. These are source-led candidate controls, not approved HARAI SLAs or an exhaustive legal audit.
- Updated DOC-68 with specific questions for privacy notice, field minimization, accuracy/correction, data-rights deadlines and registration lifecycle evidence.\n- Created DOC-69 as a candidate privacy data inventory/control-mapping worksheet for payment initiation, provider events, settlement/reconciliation, complaint cases/evidence, communications, logs, support, analytics, backups and rights/retention workflows. It is not a verified production inventory; actual fields, roles, recipients, regions and retention remain open.


## Research log — DOC-69 cross-document privacy inventory validation (2026-10-24)

- Created DOC-70 to compare DOC-69 against the conceptual data/operation models and threat/workflow proposals in DOC-41, DOC-42, DOC-43, DOC-49, DOC-56, DOC-57, DOC-58, DOC-59 and DOC-64.
- Recorded 12 candidate inventory gaps for validation: developer credential/webhook configuration; money-out beneficiary data; provider attempts and callback/status evidence; asynchronous job/recovery metadata; merchant verification and capability decisions; generated reports/exports; security/privacy incidents and privileged-access approvals; correction/refund/reconciliation evidence; abuse-control telemetry; privacy-rights and privacy-complaint records; secondary storage/copy locations; and a row-level ownership/recipient/location/retention crosswalk.
- These are documentation questions, not proof that the workflows or data categories exist or must be collected. DOC-70 explicitly requires in-scope/out-of-scope/unverified dispositions, minimal collection, source evidence and qualified review. No retention period, data location, provider payload or legal basis was inferred.
- Updated DOC-69 with a link to DOC-70 and indexed DOC-70 in DOC-00.
- No implementation or tests were performed. Gates remain unchanged: DOC-64 draft; AR-022 OPEN; FIN-004/SUP-005 unverified; G1 NOT PASSED; G2 NOT APPROVED; funds-flow model unselected. No provider/regulator outreach, credentials, live financial activity, filing/submission, merge or deployment authorized.
