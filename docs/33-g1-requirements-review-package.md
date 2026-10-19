# DOC-33 — G1 Requirements Review Package

**Status:** Draft — submitted for product-owner and accountable reviewer consideration  
**Gate:** G1 — requirements approval (not passed)  
**Related documents:** DOC-00, DOC-02, DOC-03, DOC-28, DOC-29, DOC-30, DOC-31, DOC-32  
**Purpose:** Consolidate the proposed launch baseline, decisions, evidence dependencies, review responsibilities and exact exit conditions into one package for a deliberate G1 decision.

> **Important boundary:** This package is a review aid, not approval, legal advice, provider confirmation, a final funds-flow decision, or authorization to implement the payment engine. It does not authorize contacting providers, onboarding, obtaining live credentials, handling real funds, merging pull requests or deploying.

## 1. Executive summary

HARAI is planned as an original, reusable, multi-merchant payment gateway, Uganda-first, designed for multiple business categories and possible later regional expansion. The product ambition is a comprehensive launch package rather than a deliberately minimal MVP. The intended scope includes collections and money-out, refunds/reversals where supported, reconciliation, merchant operations, developer APIs and webhooks, financial reporting, security and operational controls.

That ambition must be separated from feasibility. No payment method or financial flow may be advertised or enabled for live use until the required provider permission, contractual eligibility, qualified legal/compliance review, funding and settlement model, reconciliation evidence, security controls and operational readiness are established.

**Current recommendation:** review and resolve the product decisions in this package; keep externally dependent capabilities blocked; obtain written provider evidence only after explicit owner authorization; arrange qualified Ugandan legal/compliance review; and do not declare G1 passed until all hard gates in Section 7 have evidence or an explicit, valid disposition that does not conceal a launch-critical blocker.

## 2. Proposed product baseline for review

The following summarizes current direction. It is not a substitute for explicit approval of DOC-31 requirement IDs.

### 2.1 Product and market

- Original HARAI branding, code, architecture, data model, payment engine, API contracts and provider adapters. JulyPay is a workflow/visual reference only.
- Uganda-first, with regional expansion treated as a separate market-by-market decision.
- Multi-merchant platform, not limited to ISPs. Launch merchant categories and prohibited/restricted categories remain undecided.
- Comprehensive launch ambition, delivered only to the extent each capability passes its own gates.
- Proposed candidates include MTN MoMo and Airtel Money. Public product pages are not proof of HARAI eligibility or permission. Bank/card methods remain subject to separate verification.

### 2.2 Intended capability groups

| Capability group | Proposed baseline | Gate / limitation |
|---|---|---|
| Merchant onboarding and verification | Registration, verification lifecycle, review decisions, suspension and closure | Provider eligibility, qualified compliance review, privacy controls |
| Collections | Hosted checkout/payment links and supported provider collection flows | Written provider permission, method-specific lifecycle and reconciliation |
| Money-out | Payout/withdrawal workflows are intended launch scope | Must identify lawful/contracted funding source, account ownership, authorization, limits, returns and reconciliation |
| Refunds and reversals | Support where the relevant provider and original flow permit it | Define distinct provider capabilities and ledger/state effects; never imply universal support |
| Transactions and financial records | Traceable transaction lifecycle, immutable/auditable financial records and compensating corrections | Approved state machine, ledger model, independent finance review |
| Reconciliation | Match internal records with provider status and settlement/payout evidence; investigate breaks | Actual provider data/report access, funds-flow model, named operational ownership |
| Developer tools | Scoped API credentials, API documentation/versioning, signed webhooks and delivery/replay controls | Security architecture, contract tests and safe secret handling |
| Merchant/admin workspace | Role-aware dashboard, transaction search, reports, support and operational queues | Tenant isolation, least privilege, accessibility and auditability |
| Risk, support and operations | Limits, reviews, incident response, observability, backup/restore and recovery runbooks | Risk policy, provider support/SLA evidence, accountable owners and tested procedures |
| Additional methods/features | Bank/card, additional currencies, scheduling, split settlement, marketplace-specific payouts and regional expansion as applicable | Separate product approval and method/market-specific evidence |

### 2.3 Requirement priority meanings

Use DOC-31's definitions:

- **P0 — hard safety gate:** must be designed, reviewed and verified before any live financial operation.
- **P1 — intended core launch capability:** part of the comprehensive launch ambition, but subject to feasibility and release gates.
- **P2 — conditional extension:** requires separate approval and verified feasibility.
- **Blocked:** a dependency, permission, decision or evidence item prevents claiming availability or enabling the affected capability.

Priority is not permission. A P1 capability may remain blocked. A P0 control cannot be waived merely to meet a date. If the launch ambition cannot safely be met with available permissions and providers, document the exact blocked capability and options for owner decision; do not silently relabel it as supported.

## 3. Decisions required for G1

The table below proposes decision owners by role, not by invented name. Roles remain unassigned until the product owner names accountable people. “Proposed default” is a recommendation for review, not an approved decision.

| ID | Decision required | Proposed default / recommendation | Accountable owner | Evidence / dependency | Current status |
|---|---|---|---|---|---|
| G1-01 | Launch merchant categories and exclusions | Start with an explicit allowlist of merchant categories and prohibited/restricted activity; do not accept “all businesses” without eligibility rules | Product owner + compliance reviewer | Target customers, provider eligibility and applicable rules | Open |
| G1-02 | Launch currencies and precision | Enable only currencies supported by each contracted provider and approved settlement flow; define minor-unit and rounding rules per currency | Product owner + finance reviewer | Provider contract/fee schedules, settlement evidence and finance review | Open |
| G1-03 | Collection methods and providers | Treat MTN MoMo and Airtel Money as candidates, not confirmed integrations; assess each product separately | Product owner + technical lead | Written provider response, sandbox/production access terms and contract | Blocked on evidence |
| G1-04 | Multi-merchant permission and contracting route | Require explicit written permission for the intended gateway/aggregator model before live use | Product owner + compliance reviewer | Provider terms, contract, required licenses/approvals | Blocked on evidence |
| G1-05 | Funds-flow model | Compare provider-managed funds, direct provider integrations and a conditional hybrid only against verified evidence; select no model by assumption | Product owner + finance + compliance | DOC-28 scorecard, provider responses, funds-flow diagrams and legal review | Open / evidence blocked |
| G1-06 | Account ownership and custody responsibilities | Explicitly identify whose account receives collections, who controls funds, who settles merchants and whether HARAI ever holds or controls funds | Product owner + finance + compliance | Contractual structure and qualified Ugandan review | Blocked |
| G1-07 | Money-out funding source | Identify whether payouts use provider business-account balances, prefunding or another expressly permitted route; specify funding checks, returns and insufficient-funds behavior | Product owner + finance + compliance | Provider capability, funding terms, account ownership and reconciliation data | Blocked |
| G1-08 | Settlement schedule and evidence | Define settlement timing, cutoff/time zone, fees, holds/reserves if any, reports and exception handling from actual provider terms | Product owner + finance | Written terms, sample reports and provider operational commitments | Open |
| G1-09 | Refund, reversal and dispute lifecycle | Model refunds, provider reversals, payout returns and disputes as distinct flows where their semantics differ; retain original records and compensating entries | Product owner + technical + finance | Method-specific lifecycle docs, contract and DOC-30 review | Open / provider dependent |
| G1-10 | Fees, pricing, taxes, reserves and fee payer | Do not copy JulyPay pricing or invent costs; approve a transparent schedule only after provider costs, tax treatment, limits and commercial viability are known | Product owner + finance + qualified tax/legal reviewer | Written fee schedules, cost model and tax review | Open |
| G1-11 | Transaction and ledger authority | Approve canonical states, financial invariants, duplicate/concurrency handling and reconciliation adjustment controls before implementation | Technical lead + independent finance reviewer | DOC-10/DOC-11 design, provider state evidence and review | Open |
| G1-12 | Merchant roles and high-risk approvals | Least privilege; independent approval for designated high-risk actions where policy or law requires; initiator cannot self-approve where separation is required | Product owner + security + compliance | Role matrix, threat/risk assessment and provider conditions | Open |
| G1-13 | Risk limits and manual review | Define transaction/payout limits, velocity rules, exception queues, review authority and escalation; no invented limit values | Product owner + risk/compliance + finance | Provider limits, risk assessment and operating capacity | Open |
| G1-14 | Data privacy, retention and deletion/hold | Minimize sensitive data; define purpose, access, encryption, retention, deletion, legal holds and support access under qualified review | Product owner + privacy/compliance + security | Data inventory, provider terms and Ugandan privacy review | Open |
| G1-15 | Security baseline and credential/network controls | Require tenant isolation, scoped/revocable credentials, secure sessions, webhook authentication, redacted logs, rate limits, safe proxy/IP controls and auditable administration | Technical lead + independent security reviewer | Threat model, design review and DOC-31 P0 criteria | Open |
| G1-16 | Availability, support and recovery objectives | Set service objectives, maximum pending age, response/escalation targets, RTO/RPO and backup/restore expectations only after capacity and provider dependencies are assessed | Product owner + technical/operations lead | Business impact, hosting capacity, provider SLA and restore plan | Open |
| G1-17 | API and integration contract | Approve authentication, idempotency, errors, versioning, webhook signing/retries, correlation IDs, deprecation and developer support | Product owner + technical lead | DOC-12/DOC-14 draft contracts and integration requirements | Open |
| G1-18 | Accessibility and supported clients | Define responsive browser/PWA targets and accessibility acceptance for checkout and dashboards; defer unsupported client assumptions | Product owner + UI/technical lead | Critical journey inventory and accessibility review | Open |
| G1-19 | Named accountable owners and independent reviewers | Name responsible people for product, engineering, security, finance/reconciliation, operations, compliance/legal and independent review; reviewer must be independent of the work reviewed | Product owner | Explicit appointment/availability | Open — blocking |
| G1-20 | Commercial viability and launch boundaries | Confirm expected merchant value, operating cost, support model, provider fees and a viable scope; record exclusions instead of hiding cost or feasibility gaps | Product owner + finance | Cost model, provider evidence and support capacity | Open |

## 4. Required review responsibilities

These are roles needed for the review, not claims that people have already been appointed.

- **Product owner:** decides product intent, launch categories, acceptable trade-offs and scope; explicitly approves or rejects the baseline and records exclusions.
- **Technical lead:** checks feasibility, requirement completeness, dependencies, state transitions, interfaces, data integrity and delivery sequencing.
- **Independent security reviewer:** challenges threat model, tenant isolation, authentication, secrets, network controls, privacy safeguards and incident readiness. Must not merely self-approve their own implementation.
- **Finance/reconciliation reviewer:** checks funds-flow accounting, ledger invariants, fees, settlement, money-out funding, returns, reversals and reconciliation evidence.
- **Qualified Ugandan legal/compliance professional:** reviews the actual proposed business model, provider contracts, permissions, licensing/regulatory implications, data obligations and money-flow diagrams. DOC-30 is a checklist, not legal advice.
- **Operations/support owner:** accepts runbooks, monitoring, incident escalation, support coverage and provider escalation dependencies.
- **Independent reviewer:** performs a separately accountable review of the requirements package and evidence. Independence cannot be replaced by the author reviewing their own work.

No names, appointments, external reviews or approvals are recorded as complete at this time.

## 5. Evidence register

Use the following evidence classes. Store confidential contracts, credentials, personal identity evidence and other sensitive material in controlled storage, not in this public repository. Commit only a redacted summary, date, evidence owner, reference/location under approved access controls, and conclusion.

| Evidence ID | Evidence needed | Acceptable evidence form | Blocks |
|---|---|---|---|
| EV-01 | Written provider confirmation of product availability, merchant/aggregator eligibility and permission for HARAI's intended model | Dated written response and/or executed contract, reviewed for scope and limitations | Provider activation, live collections and affected money-out methods |
| EV-02 | Provider technical and operational capability | Official API/product documentation, sandbox details, status semantics, webhook/retry behavior, support and escalation terms | Adapter design and method-specific acceptance |
| EV-03 | Fees, limits, settlement, funding and returns | Current written schedules/terms, sample settlement/payout reports, prefunding/return rules | Pricing, finance model, reconciliation and money-out |
| EV-04 | Qualified Ugandan legal/compliance conclusion | Written professional review of the actual proposed flows, contracts, roles and obligations | Selecting funds-flow model and authorizing live business operations |
| EV-05 | Funds-flow/account ownership diagram | Reviewed diagram showing payer, provider, merchant accounts, control/custody, fees, settlement, payout funding, exceptions and reconciliations | Architecture and financial responsibility decisions |
| EV-06 | Requirement approval and scope exclusions | Product-owner decision record with accepted DOC-31 IDs, blocked items, deferred items and rationale | G1 completion |
| EV-07 | Security/privacy review | Threat model, data inventory, access controls, retention plan, findings and independent disposition | G1 security readiness and later implementation gate |
| EV-08 | Ledger/reconciliation review | Reviewed financial invariants, sample lifecycle scenarios, reconciliation sources and discrepancy handling | Financial implementation approval |
| EV-09 | Operational capacity and recovery plan | Owner-approved objectives, provider support dependencies, runbooks, backup/restore approach and test plan | G1 operational baseline and production gate |
| EV-10 | Traceability plan | DOC-31 requirements mapped to decisions, design artefacts, test IDs, evidence owners and release gates | G1 exit criteria |

**Evidence discipline:** public marketing pages can inform questions but cannot establish HARAI's contractual eligibility. Verbal assurances should be followed by written confirmation. A missing response remains missing evidence; do not assign a score or mark a gate passed by inference.

## 6. Requirement disposition rules

Every DOC-31 requirement proposed for launch must have a recorded disposition:

- **Approved:** the accountable product owner accepted its meaning and scope, with required specialist reviews completed where applicable.
- **Blocked:** required authority, provider capability, legal conclusion, design dependency or evidence is missing. It must not be represented as available.
- **Deferred:** deliberately moved outside the approved launch scope with owner, rationale, user impact and re-entry condition recorded.
- **Rejected:** explicitly excluded by an accountable decision with rationale and affected requirements updated.
- **Implemented / Verified:** only used later, after authorized implementation and linked evidence; writing or code alone does not prove verification.

A blanket approval of “the document” is not enough if critical decisions remain ambiguous. Decisions involving legal permission, custody/funds flow, settlement or funding source must not be treated as accepted solely by product preference.

## 7. Hard gates for passing G1

G1 may be marked **Passed** only when all applicable conditions below have documented evidence and the product owner plus required accountable reviewers explicitly record their decisions.

- [ ] **Scope baseline:** target merchant categories, exclusions, markets, currencies, payment methods and intended launch capabilities are explicitly approved or individually blocked/deferred/rejected.
- [ ] **Traceability:** every in-scope DOC-31 requirement has an accountable owner, priority, acceptance criteria, dependency, planned test/evidence reference and release gate.
- [ ] **Provider eligibility:** written permission/contractual evidence is available for every provider-dependent capability intended to be enabled at launch; unverified capabilities remain blocked.
- [ ] **Funds flow and account ownership:** candidate flow diagrams identify who receives, controls, holds and settles funds, with responsibilities reviewed by finance and qualified Ugandan legal/compliance counsel.
- [ ] **Money-out funding:** funding source, sufficiency checks, limits, approvals, return/failure handling and reconciliation route are explicitly understood and evidenced for each planned payout method.
- [ ] **Refund/reversal/dispute behavior:** provider-specific capabilities, finality, permitted transitions, deadlines, ledger effects, returns and reconciliation treatment are defined or explicitly blocked.
- [ ] **Financial integrity:** ledger principles, fee calculation/rounding, reconciliation sources, discrepancy ownership and compensating-correction controls receive finance review.
- [ ] **Security and privacy:** P0 controls, threat model, tenant boundaries, credential/network protection, sensitive-data access and retention requirements receive independent review; unresolved critical findings block G1.
- [ ] **Operational ownership:** accountable technical, security, finance, operations, compliance and independent review roles are named; support/escalation dependencies and realistic recovery expectations are recorded.
- [ ] **Acceptance plan:** test/evidence mapping covers normal, rejected, duplicate, concurrent, delayed, unknown-outcome, partial-failure, reversal/return, reconciliation-break and recovery scenarios where applicable.
- [ ] **Commercial viability:** provider costs, proposed merchant pricing, tax review, operating/support costs and material limits are understood enough for the owner to make an informed scope decision; unknowns are not disguised as facts.
- [ ] **Independent review:** a separately accountable reviewer records findings and disposition. Self-review by the document author/implementer does not satisfy this condition.
- [ ] **Decision register:** DOC-00 and the relevant documents reflect all decisions, open issues, blocked capabilities, owners and the explicit G1 outcome.

**Important distinction:** G1 requirements approval does not itself authorize implementation, production provider onboarding, live credentials, real transactions, merging PR #1/#2 or deployment. Those remain separate, explicit gates under the roadmap.

## 8. Review workflow

1. Product owner reviews the proposed baseline and the decision table; approves, edits or rejects each relevant proposal.
2. Assign named accountable owners and independent reviewers. Do not infer appointment from a role being listed here.
3. Identify the smallest set of unresolved decisions that require written provider evidence or professional advice; do not contact providers until explicitly authorized.
4. Arrange qualified Ugandan legal/compliance review of actual candidate funds-flow diagrams and proposed contracts when available.
5. Finance and technical reviewers validate funding, settlement, ledger and reconciliation assumptions against evidence.
6. Security/privacy and operations reviewers assess P0 controls, ownership and recovery expectations.
7. Update DOC-31 and DOC-00 with stable decisions, owners, evidence references, blockers and requirement dispositions.
8. Record an explicit G1 decision. If any hard gate is unresolved, disposition is **G1 not passed**; list the blockers and next actions.
9. Only after G1 is passed should the team finalize G2 architecture documents for review. Implementation still requires a separate explicit authorization.

## 9. Approval record template

Complete this only when an actual review has occurred. Do not pre-fill names, dates or approvals.

- **Review date:** Not recorded
- **Product owner:** Not yet recorded
- **Technical lead:** Not yet recorded
- **Independent security reviewer:** Not yet recorded
- **Finance/reconciliation reviewer:** Not yet recorded
- **Qualified Ugandan legal/compliance reviewer:** Not yet recorded
- **Independent reviewer:** Not yet recorded
- **DOC-31 baseline decision:** Pending
- **Accepted requirement IDs / sections:** Pending
- **Blocked requirements and reason:** Pending
- **Deferred/rejected requirements and rationale:** Pending
- **Outstanding evidence references:** Pending
- **Material risks accepted and by whom:** Pending; legal/contractual requirements cannot be waived by product preference
- **G1 outcome:** **Not passed — review and evidence pending**
- **Implementation authorization:** Not granted by this document

## 10. Current blockers and next sequence

Current blockers carried forward from DOC-32 include provider multi-merchant permission and eligibility, legal/compliance review, funds flow and account ownership, money-out funding, currency and settlement terms, provider lifecycle behavior, commercial viability, named accountable owners and independent review.

Recommended next sequence:

1. Product owner review of DOC-33 and DOC-31, with decisions recorded rather than assumed.
2. Name the technical, security, finance/reconciliation, operations, compliance/legal and independent reviewers.
3. If authorized, send the DOC-29 written inquiry pack to selected providers and record actual responses.
4. Obtain qualified Ugandan legal/compliance review of candidate funds-flow options using DOC-30.
5. Update the requirements baseline and evidence register; re-audit consistency.
6. Pass G1 only when the Section 7 conditions are met; then prepare G2 architecture and security design for review.

## 11. Audit boundary and status

This package consolidates known product direction and unresolved requirements as of 2026-10-19. It does not assert that providers have approved HARAI, that legal review has happened, that reviewer roles are staffed, or that the funds-flow model is selected.

**Current disposition: G1 NOT PASSED.** The next action is a requirements review and evidence/owner resolution, not payment-engine implementation. No provider has been contacted by this document. No live credentials, real transactions, pull-request merge or deployment are authorized.


## 12. Cross-review against DOC-31 (2026-10-19)

A document-level cross-review was performed against the current DOC-31 requirements catalogue. This is an editorial consistency check, not an independent security, finance, legal or product-owner sign-off.

### 12.1 Coverage observed

DOC-31 currently contains 43 distinct requirement IDs:
- Governance, tenancy, merchant onboarding and identity/access: GOV-001, TEN-001, MER-001–MER-004, IAM-001–IAM-002.
- Collections and payer experience: COL-001–COL-008.
- Money-out, refunds and reversals: OUT-001–OUT-006.
- Ledger, fees, reconciliation and reporting: FIN-001–FIN-005, REC-001–REC-002, REP-001.
- Developer platform, security and operations: DEV-001–DEV-003, SEC-001–SEC-003, OPS-001–OPS-004.
- Conditional extensions: EXT-001–EXT-003.

DOC-33's decision table and hard-gate checklist cover these same major domains. No intentional reduction of the stated comprehensive launch ambition was introduced by this review. Provider- and market-dependent features remain gated rather than presumed available.

### 12.2 Findings and required disposition

| Review ID | Finding | Severity / status | Required next action |
|---|---|---|---|
| XRV-001 | Requirement IDs and broad domains align between DOC-31 and DOC-33, but DOC-33 is a review package and does not itself approve any individual requirement | Governance-critical; open | Product owner records accepted, amended, blocked, deferred or rejected requirement IDs |
| XRV-002 | DOC-31 defines the fields required for traceability, but the catalogue does not yet attach named owners, concrete test IDs, reviewers and evidence references to every requirement row | G1 blocker | Build/complete a requirement-by-requirement traceability matrix after scope decisions and owner assignments |
| XRV-003 | DOC-33 lists role-based review responsibilities, but no people have been named or confirmed as available | G1 blocker | Product owner names accountable technical, security, finance/reconciliation, operations and independent reviewers; arrange qualified Ugandan legal/compliance review |
| XRV-004 | Provider eligibility, multi-merchant permission, money-out funding, settlement and account ownership cannot be settled by document consistency review | External evidence blocker | Obtain explicit owner authorization before provider outreach; then record written responses and qualified professional review |
| XRV-005 | Fees, taxes, reserves, limits, service/recovery objectives and commercial viability are not yet based on approved values/evidence | Decision/evidence blocker | Keep numeric targets and prices uncommitted until provider evidence, capacity analysis and relevant review are available |
| XRV-006 | G1 approval is distinct from implementation authorization; the current docs consistently preserve this boundary | Control verified at document level | Continue to require separate explicit authorization for implementation, merge, provider onboarding and deployment |

### 12.3 Review outcome

**Document consistency: adequate to proceed with owner review. G1 readiness: not achieved.** The key remaining gap is not another general narrative document; it is a decision-and-evidence record with named accountable reviewers and requirement-level traceability. No individual requirement is marked Approved, Implemented or Verified by this cross-review. No product-owner decision, independent specialist review, provider response or legal conclusion has been inferred.



## Supplemental requirement update (2026-10-19)

DOC-39 added SUP-001–SUP-010 to DOC-31. DOC-34 now maps all 54 unique requirement IDs. This update does not mean the expanded baseline is approved: each supplemental requirement requires accountable disposition, numeric thresholds remain open where identified, proposed test IDs are not execution evidence, and G1 remains not passed.
