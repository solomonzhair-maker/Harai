# DOC-46 — G2 Gap-Closure and Ownership Matrix

**Status:** Draft working tracker — no gate is passed by this document  
**Date:** 2026-10-19  
**Related documents:** DOC-00, DOC-28, DOC-30, DOC-31, DOC-34, DOC-37, DOC-38, DOC-40–DOC-45, DOC-48–DOC-50  
**Purpose:** Convert unresolved architecture decisions and every P0 requirement in DOC-31 into actionable closure work, with role-level accountability, evidence expectations and blocking effects.

> Roles are proposed, not assigned to named people. No provider contact, application, payment-engine implementation, merge or deployment is authorized by this tracker. A listed test plan is not a test result.

## 1. How to use this tracker

DOC-50 adds proposed merchant onboarding, verification, capability-level activation, suspension and closure controls. Map its ONBOARD-* planning IDs to DOC-31 requirements and future executed evidence; document existence does not close any P0 blocker or authorize production access.

- The normative requirement wording and acceptance criteria remain in DOC-31. This document summarizes the closure path; it does not replace the acceptance criteria.
- The accountable roles below must be assigned to named people by the product owner before G2 sign-off. Required independent reviewers must not be replaced by the sole author of the work being reviewed.
- Status begins **Open — unverified** unless an evidence-backed update is made. Do not mark an item closed because a document, proposed test ID or checklist exists.
- A blocker may be closed only when its stated evidence is received, reviewed, accepted, and linked to the relevant decision/requirement.
- “Internal” means the work can be prepared without contacting providers or making external commitments. “External” means written provider authority, qualified professional review, or other external evidence is required. “Implementation” means executable proof can only be produced after a separate implementation/test authorization.
- A blocker for a method or flow must disable that method or flow; it must not silently be treated as a whole-platform approval.

## 2. P0 requirement closure matrix

The following rows cover all 32 P0 requirement IDs currently listed in DOC-31. Use DOC-31 for full acceptance criteria.

| Requirement ID | Requirement | Proposed accountable role(s) | Minimum closure evidence | Gate / blocking effect |
|---|---|---|---|---|
| GOV-001 | Decisions and scope are controlled | Product owner + decision-register steward | Named owner, decision IDs/statuses, linked evidence, and independent review path recorded | Governance; G1/G2 |
| TEN-001 | All merchant data is tenant-isolated | Technical lead + independent security reviewer | Approved tenant model plus negative tests across APIs, workers, reports, support and callbacks | G1/G2; live-operation hard gate |
| MER-002 | Live merchant activation is gated | Merchant onboarding owner + compliance reviewer | Written eligibility/activation policy; provider approval evidence; testable disabled-by-default gate | G1/G2; per-method launch gate |
| IAM-001 | Users receive least-privilege access | Identity/access owner + security reviewer | Role/permission matrix, revocation/session policy and authorization test plan | G2; live-operation hard gate |
| IAM-002 | Sensitive actions have appropriate approval controls | Risk/operations owner + finance/compliance reviewer | Sensitive-action policy, limits, separation-of-duties matrix and approval evidence | G2; money-out/refund hard gate |
| MER-003 | Merchant verification evidence is handled safely | Merchant onboarding owner + privacy reviewer | Evidence inventory, least-privilege access, retention/reverification rules and synthetic-data test plan | G1/G2 |
| COL-003 | Duplicate collection submissions are safe | Payments technical lead | Atomic idempotency design, payload conflict semantics and concurrency-test plan | G2; live collection hard gate |
| COL-004 | Pending and unknown outcomes are handled safely | Payments technical lead + provider integration owner | Provider-specific finality/status/retry evidence and unknown-outcome recovery criteria | G2; live collection hard gate |
| COL-005 | Only verified events can change transaction state | Provider integration owner + independent security reviewer | Applicable callback authentication/signature contract, replay rules and negative-test plan | G2; live collection hard gate |
| COL-008 | API abuse is bounded without breaking recovery | API/platform owner + security reviewer | Per-tenant/credential/endpoint rate policy and recovery-safe limits; abuse-test plan | G2; live API hard gate |
| OUT-002 | Payout uncertainty cannot cause duplicate payment | Money-out owner + provider integration owner + finance reviewer | Provider idempotency/status evidence, funding/permission gates and ambiguous-outcome test plan | G2; money-out hard gate |
| OUT-005 | High-risk financial actions are auditable | Finance/operations owner + security reviewer | Audit-event schema, permissions, retention policy and sensitive-action test plan | G2; live financial operations |
| FIN-001 | Financial amounts use exact monetary representation | Finance/accounting reviewer + technical lead | Currency/precision policy and examples approved; no floating-point monetary arithmetic | G2; ledger hard gate |
| FIN-002 | Financial history is append-only/auditable | Finance/accounting reviewer + technical lead | Posting invariants, correction/compensation rules and concurrency/deduplication plan approved | G2; ledger hard gate |
| FIN-003 | Fees and net settlement are explainable | Finance/accounting reviewer + product owner + qualified tax/legal adviser | Written provider fee schedules, approved pricing/tax/rounding semantics and worked examples | G2; funds-flow and live pricing gate |
| REC-001 | Transactions reconcile to provider evidence | Finance/reconciliation owner + provider integration owner | Provider transaction and settlement data access confirmed; stable matching keys and exception workflow defined | G2; funds-flow/live operations gate |
| REC-002 | Reconciliation discrepancies cannot be hidden | Finance/reconciliation owner + operations owner | Discrepancy severity, owner, age/escalation, resolution authority and evidence trail defined | G2; live financial operations |
| FIN-005 | Financial state transitions are formally specified | Payments technical lead + finance reviewer + provider integration owner | Reviewed canonical state machine with per-method finality/exception mapping and planned transition tests | G2; live financial operations |
| SEC-001 | Authentication and sessions are protected | Identity/access owner + independent security reviewer | Session, MFA/step-up, recovery, expiry/revocation and abuse controls documented and testable | G2; live account access |
| SEC-002 | Sensitive information is minimized and protected | Privacy/data owner + security reviewer + qualified compliance adviser | Data inventory, access/encryption/redaction/retention/deletion rules and support-access controls | G2; data processing/live launch |
| OPS-001 | Failures and incidents have owned runbooks | Named operations owner + provider support liaison | Runbooks, escalation roles, provider support assumptions and incident exercises defined | G2/G3; operational readiness |
| OPS-002 | Production release requires evidence-based gates | Release owner + independent security and finance reviewers | Evidence-based release checklist and explicit stop conditions approved; no hard-gate waiver | G2/G5–G8; production hard gate |
| SEC-003 | Network and browser access controls are safe | Platform/deployment owner + security reviewer | Trusted-proxy, CORS/CSRF, IP allowlist and lockout recovery threat review/test plan | G2; production hard gate |
| OPS-003 | Reliability targets and recovery are measurable | Operations owner + product owner + technical lead | Owner-approved measurable SLO/RTO/RPO, pending-age and alert targets with capacity rationale | G2/G3; production readiness |
| SUP-001 | API abuse and recovery-request rate limits are explicit | API/platform owner + security reviewer | Documented endpoint-specific limits, bounded polling and observable throttling; abuse/recovery test plan | G1/G2; live API hard gate |
| SUP-002 | Payment links and checkout sessions have controlled lifecycle and usage | Collections product owner + technical lead | Explicit expiry/cancel/revoke/usage semantics and concurrent-use acceptance criteria | G1/G2; checkout release gate |
| SUP-003 | Beneficiary and payout-destination changes are protected | Money-out owner + security/finance reviewer | Destination-change policy, step-up/approval/cooling-off rules and pending-payout handling | G2; money-out hard gate |
| SUP-004 | Transaction state transitions and finality are canonical | Payments technical lead + provider integration owner + finance reviewer | DOC-41 state model reconciled with provider evidence; transition/finality scenarios specified | G2; live financial operations |
| SUP-006 | Public API contracts remain versioned and predictable | API/platform owner + developer experience reviewer | Versioning/deprecation, stable errors, correlation IDs and deterministic pagination contract | G1/G2; public API release gate |
| SUP-007 | Browser, proxy and IP-allowlist trust boundaries are defined | Platform/deployment owner + independent security reviewer | Trusted proxy boundary, origin/CSRF rules, authenticated allowlist changes and recovery tests | G2; production hard gate |
| SUP-008 | Reliability and recovery objectives are measurable | Operations owner + technical lead + product owner | Approved SLO/RTO/RPO, backup frequency, restore verification and incident escalation evidence | G2/G3; production hard gate |
| SUP-010 | Security and financial events are correlated end-to-end | Observability/platform owner + security reviewer + finance reviewer | Correlation map across API/provider/callback/ledger/reconciliation/cases; redaction and audit-access tests | G2; live financial operations |

## 3. Cross-cutting G2 decisions and evidence blockers

| Work item | Proposed accountable role | Work that can proceed internally now | Evidence or authority still required | Closure condition |
|---|---|---|---|---|
| G2-D01 Architecture topology | Technical architecture owner | Compare modular-monolith and worker separation against failure modes, operability and expected scale | Independent architecture review | Recorded decision with rationale, consequences and reviewer findings |
| G2-D02 Funds-flow model | Product owner + finance reviewer | Maintain equal criteria and compare known evidence in DOC-28 | Written provider permission/contracts; qualified Uganda legal/compliance advice; fee and settlement terms | One model supported by equivalent evidence; critical unknowns cannot be scored favorably |
| G2-D03 Provider multi-merchant authority | Provider/commercial owner (person to be assigned) | Prepare a precise written question pack and evidence checklist; no sending yet | Written confirmation/contract from each relevant provider | Exact HARAI operating model and enabled capabilities expressly permitted |
| G2-D04 Provider production/money-out access | Provider integration owner | Keep capability matrix and disabled-by-default rules current | Applicable account approval, production access, funding source, limits and operating terms | Each method independently passes its permission and operational gate |
| G2-D05 Ledger and accounting | Finance/accounting reviewer | Prepare example postings and discrepancy cases using synthetic amounts | Qualified review of accounts, gross/net/fees, refund/return treatment, tax and corrections | Approved accounting semantics and reconciliation policy |
| G2-D06 Security and tenant isolation | Independent security reviewer + technical owner | Refine threat/control mapping and negative-test design | Independent review; later authorized test execution and remediation evidence | No unresolved critical isolation, authentication, callback, credential or privileged-access issue |
| G2-D07 Reliability and recovery | Operations owner + technical owner | Draft incident/restore/reconciliation runbooks and propose measurable objectives | Owner-approved targets and evidence of restore/alert exercises after implementation authorization | RTO/RPO/SLO, ownership, alerting and recovery are testable and approved |
| G2-D08 API and event contracts | API owner + provider integration owner | Draft stable error, versioning, correlation and outbound webhook contract proposals | Provider-specific contracts where applicable; independent security review | Versioned, testable contracts and explicit unknowns documented |
| G2-D09 Test readiness | Test/quality owner (person to be assigned) | Map requirement IDs to planned scenarios and define synthetic fixtures | Separate authorization to implement/run tests; CI/build evidence | Results are tied to commit/environment, failures retained, required retests pass |
| G2-D10 Independent reviews | Product owner | Identify candidate reviewers and their scope/conflicts | Named, qualified reviewers and dated review outcomes | Required reviews recorded; self-review is not independent approval |
| G2-D11 Compliance and privacy | Qualified Uganda legal/compliance adviser + privacy owner | Assemble the exact proposed funds flow, data map and question list | Written advice scoped to HARAI's actual role, methods and intended launch | Applicable obligations, conditions and unresolved advice are recorded |
| G2-D12 Release readiness | Release owner + security/finance/operations reviewers | Keep production disabled and maintain the hard-gate checklist | Complete test, provider, legal, finance, operations and independent sign-off evidence | Separate explicit release decision; G2 alone does not authorize production |

## 4. Evidence dependency map

| Dependency group | Primary references | Evidence type expected | Current consequence if missing |
|---|---|---|---|
| Provider eligibility and commercial terms | DOC-37, DOC-38, DOC-45 | E3 written authority/contract, supported by E1/E2 | Affected provider/method cannot be enabled; funds-flow model may remain undecidable |
| Funds flow and regulatory role | DOC-28, DOC-30, DOC-44 | E3 qualified written advice plus provider contract/evidence | No funds-flow selection or live financial operations |
| State machine and recovery | DOC-41, DOC-40, DOC-31 | E1/E3 provider-specific evidence, E4 reviewed design, later E5 tests | No safe finality/retry behavior for affected method |
| Ledger and reconciliation | DOC-42, DOC-31 | E4 finance-reviewed model, provider reports/contracts, later E5/E6 evidence | No live posting, settlement or unresolved-discrepancy handling |
| Security and tenant isolation | DOC-43, DOC-40, DOC-31 | E4 independent review, later E5 test evidence | No live activation where critical controls are unproven |
| Operations and resilience | DOC-45, DOC-31, DOC-03 | E4 runbooks, later E5/E6 exercises | No production readiness |
| Independent review | DOC-44, DOC-45 | E7 named reviewer outcome | Relevant gate remains open |


## 4A. Consolidated G1/G2 review checklist — current blockers

This checklist consolidates the next gate-review work; it is not a calendar, gate approval, or assignment of named people. All role labels remain **unassigned** until the product owner records names, qualifications, scope, availability and conflicts. Keep status **Open / Blocked** until evidence is received and accepted. A document or planned test alone is not closure evidence.

### G1 — requirements approval

| Check | Accountable role to assign | Minimum decision/evidence required | Current status and blocking effect |
|---|---|---|---|
| G1-01 — Baseline and requirement disposition | Product owner + requirements steward + independent requirements reviewer | Review all 54 DOC-31 IDs, priorities, acceptance criteria, dependencies, exclusions and requirement-to-evidence mappings; record disposition and dissent | Open; G1 cannot pass while requirements are undisposed or material conflicts remain |
| G1-02 — Approved security wording incorporated | Requirements steward + independent security reviewer | Confirm RCP-01–RCP-08 are present in DOC-31 and mapped consistently in DOC-34, DOC-60 and DOC-61; approval covers wording only | Wording recorded as approved on 2026-10-23; independent confirmation and design/evidence remain open |
| G1-03 — Provider eligibility and capability | Provider/commercial owner + product owner | Written, applicable evidence for multi-merchant/aggregator eligibility, method-by-method access, merchant/KYC restrictions, production access, callbacks/status/retry behavior, fees, settlement and support | Blocked on written evidence; affected methods cannot be claimed available or enabled |
| G1-04 — Funds flow and Uganda obligations | Product owner + qualified Uganda legal/compliance adviser + finance/accounting reviewer | Evidence-backed comparison of DOC-28 models, who receives/controls/pays out funds, contractual permission, regulatory role, tax/accounting and complaint responsibilities | Blocked; no funds-flow model selected and no live financial operation |
| G1-05 — Named reviewers and authority | Product owner | Name qualified technical, security, finance/accounting, legal/compliance, privacy and operations reviewers; record independence/conflicts and decision authority | Open; role labels are not appointments, and author self-review is not independent sign-off |
| G1-06 — Launch claims and method scope | Product owner + provider/compliance reviewers | Distinguish intended product scope from verified, contractually permitted capabilities; keep each unsupported method/operation explicitly disabled | Open; no claim of operational availability based on product intent alone |

### G2 — architecture and financial-safety approval

| Check | Accountable role to assign | Minimum decision/evidence required | Current status and blocking effect |
|---|---|---|---|
| G2-01 — Architecture decisions | Technical architecture owner + independent architecture reviewer | Disposition G2-DEC-01–12 and DOC-47 AR-001–AR-021; AR-019 is closed only for indexing and cross-linking; AR-021 remains open because the canonical idempotency contract is not yet approved and verified across API requests, provider attempts, callbacks, ledger postings, reconciliation imports and restore/replay. DOC-42 §7.1, DOC-41 §5B, DOC-48 §4.2 and DOC-49 §7 now contain a coordinated proposal covering request fingerprint/conflict, atomic reservation, linked identities, source/content deduplication, retention and restore/replay. Exact endpoint compatibility, canonicalization/version migration, retention/tombstones, provider-specific identity guarantees, collision handling and restore-resume authority remain open; no implementation or test evidence exists. Document alternatives, consequences, evidence, unresolved risks and reviewer outcome | Open; topology and material design decisions remain proposals |
| G2-02 — Provider lifecycle and safe recovery | Payments technical lead + provider integration owner + finance reviewer | Provider-evidenced state/finality per provider product, operation and API version; separate source authenticity, attempt correlation, status meaning and settlement meaning; UNKNOWN outcomes; callback/enquiry/settlement conflict handling; idempotency/replay; cancellation/late-event races; refund/return and money-out semantics | Blocked where authoritative provider evidence is absent; no safe live processing for affected capability |
| G2-03 — Ledger and reconciliation | Finance/accounting reviewer + reconciliation/operations owner | Approved posting/correction semantics, fee and settlement calculations, stable source identity/content integrity, discrepancy authority, evidence provenance and matching sources | Open/blocked; no live posting or settlement based on unreviewed semantics |
| G2-04 — Tenant isolation and dispatch-time authorization | Technical owner + independent security reviewer | Design and later authorized negative-test evidence that server-derived tenant ownership is revalidated for API, jobs, callbacks, webhook delivery/retry/replay, reports and exports | Open; live-operation hard gate until verified |
| G2-05 — Outbound webhook SSRF | Security/platform owner + independent security reviewer | Review approved HTTPS/URL policy, IPv4/IPv6 private/reserved/loopback/link-local/metadata blocks, DNS and rebinding defenses, redirect revalidation, egress restrictions and no credential forwarding; later authorized tests | Open; outbound delivery to untrusted destinations cannot be enabled before design and verification |
| G2-06 — Restore and replay safety | Operations owner + technical owner + finance reviewer | Backup access/encryption/key/retention/integrity policy; post-restore worker/replay pause; database/ledger/queue/provider-reference validation, reconciliation and auditable authorized resume; later restore exercise | Open; consequential workers/replay remain disabled after restore until all checks pass |
| G2-07 — CI/deployment identity and artifacts | Platform/release owner + security reviewer | Attributable least-privilege identities, environment-scoped credentials, secret protection, dependency provenance/review and auditable release artifacts | Open; no production release until reviewed and verified |
| G2-08 — Sensitive-data lifecycle | Privacy/data owner + security reviewer + qualified compliance adviser | Inventory and minimize/redact traces, analytics, screenshots, fixtures, offline caches, support exports, diagnostics, build artifacts and backups; approve access/retention controls; do not offline-cache private verification evidence | Open; data processing and live launch remain gated |
| G2-09 — Contradictory provider evidence | Finance/reconciliation owner + authorized risk/operations role + independent approver where policy requires | Explicit resolution authority, source evidence/provenance, rationale, audit history and linked correction; no unauthorized override or silent overwrite | Open; disputed consequential financial state must remain unresolved pending authorized evidence-based resolution |
| G2-10 — Card-data boundary | Card/processor owner + security reviewer + qualified compliance adviser | Confirm processor permission, data flow, applicable reviewed security scope and explicit enablement gate; ensure prohibited sensitive card data is not copied into HARAI-controlled systems | Open; card path stays disabled pending required authority and review |
| G2-11 — P0 control ownership and evidence | Each assigned control owner + relevant independent reviewer | For all 32 current P0 IDs, record proposed design/control, owner, dependencies, evidence class, verification approach and unresolved risk; later attach authorized reproducible results where required | Open; no P0 requirement is declared passed by this checklist |
| G2-12 — Operations, incidents and release boundary | Operations owner + release owner + security/finance reviewers | Named discrepancy/incident ownership, runbooks, measurable owner-approved objectives, restore/reconciliation readiness, dated review outcomes and a separate release decision | Open; G2 does not itself authorize implementation or production release |

### Review appointment and independence boundary — 2026-10-23

The product owner has designated ChatGPT as the sole reviewer for the current internal documentation and proposed-design review. ChatGPT will perform the consolidated review of requirements traceability, architecture consistency, payment lifecycle/finality, ledger/reconciliation, tenant isolation, security-control design, operational recovery and cross-document contradictions; record findings and recommendations; and verify that document changes address the findings.

**Review status and limits:** this appointment is not an independent third-party review, a professional certification, or evidence that controls have been implemented or tested. ChatGPT must not mark a control as verified solely because it proposed or documented that control. Written provider permissions/contracts, qualified Uganda legal/compliance advice, finance/accounting acceptance where needed, and authorized execution evidence remain external or real-world dependencies. Where the gate criteria require genuinely independent review, that criterion remains unmet unless the product owner later appoints a separately accountable reviewer. G1 remains NOT PASSED and G2 remains NOT APPROVED until their respective evidence-based criteria are met. This appointment does not authorize tests, implementation, provider contact, production credentials, real financial activity, merge or deployment.

### Gate decision rule

- **G1 passes only** after the product owner and required independent reviewers record the reviewed requirements baseline and the applicable external/provider/legal dependencies are resolved or explicitly scoped as blockers to individual capabilities.
- **G2 passes only** after material architecture decisions, every applicable P0 design/control, funds-flow and financial semantics, security review, named accountability and required evidence are accepted with no unresolved non-waivable blocker.
- **Neither gate can be passed by this checklist alone.** Tests, implementation, provider contact, production credentials, real financial activity, merge and deployment remain separately unauthorized unless explicitly approved.
- Until names are recorded, report the next action as **“product owner to appoint reviewer/owner”**, not as though a person has accepted responsibility.

## 5. Priority sequence

This sequence is a dependency order, not a calendar or a claim that staffing/duration is known.

1. **Assign accountable people:** product owner identifies technical, finance/accounting, security, operations, privacy/compliance and test owners; record reviewer independence and conflicts.
2. **Resolve decision framing:** keep funds-flow models unscored until evidence is comparable; confirm exactly what legal/provider questions must be answered.
3. **Close design contradictions:** reconcile DOC-40, DOC-41, DOC-42 and DOC-43 against every P0 requirement; document accepted changes and unresolved points.
4. **Acquire external evidence only when authorized:** written provider terms and qualified legal/finance/security review; provider contact is not authorized by this document.
5. **Approve the detailed design baseline:** decision records, threat controls, state transitions, accounting semantics, exception authority and operations are reviewed.
6. **Request a separate implementation authorization:** only after the owner has reviewed the scope, remaining blockers and proposed test plan.
7. **Execute and retain verification evidence:** run approved automated/contract/integration/concurrency/security/recovery tests in controlled environments; use synthetic data unless specifically approved otherwise.
8. **Review findings independently:** fix and retest failures; no silent waivers of legal or financial safety gates.
9. **Make a separate G2 outcome decision:** document pass/fail/blocked with named reviewers and evidence links.
10. **Keep production as a separate gate:** provider production approval, operational readiness and release sign-off are still required after G2.

## 6. Closure record template

For each item, record:
- ID and requirement/decision link
- Named accountable owner and required reviewer
- Current state: Open, Blocked, In progress, Evidence received, Reviewed, Accepted, Rejected, or Superseded
- Specific evidence claim and evidence ID from DOC-45
- Evidence class, source/date/version and limitations
- Findings, unresolved risk and required remediation
- Reviewer name, role, date and outcome
- Next action and blocking scope
- Change history and linked commit/document revision

## 7. Current assessment

The immediate internal work is to assign roles, review and reconcile the design proposals, and prepare synthetic test specifications. External authority, qualified professional reviews and executed verification evidence are not replaceable by further prose. No named specialist or independent gate reviewer has been assigned; ChatGPT is appointed only for the internal documentation/design review. No P0 item is declared passed, no funds-flow model is selected, and G2 remains open.
