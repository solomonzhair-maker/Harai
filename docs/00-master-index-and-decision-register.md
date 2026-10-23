# HARAI — Master Index and Decision Register

**Status:** Draft — discovery in progress; POD-01–POD-12 product defaults approved, G1 not passed  
**Last reviewed:** 2026-10-19  
**Implementation gate:** This document does not authorize payment-engine implementation.

## 1. Purpose

HARAI is intended to be a reusable payment gateway for the owner's current and future systems, including Shigona and Herupu. Its long-term direction is to serve multiple businesses and integrated platforms through an original implementation, original branding, and authorized payment-provider integrations.

The current market direction is **Uganda first**, with a **strong, comprehensive launch package** as the planning ambition—not a deliberately minimal MVP. HARAI should be designed for multiple merchant types and future regional expansion. The planned launch package includes both **collections and money-out workflows**, alongside refunds/reversals where supported, financial controls, merchant tools, and developer integrations. Exact enabled capabilities must still be validated against provider access, commercial agreements, legal requirements, cost, security, and operational readiness.

This is a requirements and planning document set. It is not evidence that provider access, commercial agreements, legal permission, or production readiness have been completed.

## 2. Source-of-truth rules

1. Approved documents in this repository are the source of truth for HARAI requirements.
2. Every requirement needs a stable ID, status, source/rationale, dependencies, security and financial impact, and testable acceptance criteria.
3. JulyPay observations must be labelled Observed, Partially observed, Unconfirmed, or Proposed. Assumptions must never be presented as facts.
4. When documents conflict, record the conflict in the decision register; do not silently choose a value.
5. Never commit credentials, session data, private customer information, or real transaction details.
6. No payment-engine coding, live financial actions, merge, or deployment may be inferred as approved merely because a document was drafted.
7. Drafts remain proposals until explicitly approved by the product owner and relevant accountable reviewers.
8. A comprehensive launch ambition must not be used to claim support for a method, country, currency, payout direction, or settlement model until it is verified and authorized.

## 3. Document index

| ID | Document | Status | Purpose |
|---|---|---|---|
| DOC-00 | Master index and decision register | Draft | Document map, decisions, blockers and change control |
| DOC-01 | JulyPay feature discovery inventory | Draft | Observed workflows and explicit unknowns |
| DOC-02 | Product scope and requirements framework | Draft | Boundaries, requirement format and core principles |
| DOC-03 | End-to-end implementation roadmap | Draft | Phases, subphases, gates and exit criteria |
| DOC-04 | Roles, tenancy and access control | Not started | Tenant isolation and permissions |
| DOC-05 | Merchant onboarding and verification | Not started | Merchant setup and activation gates |
| DOC-06 | Collections and hosted checkout | Not started | Collection lifecycle and payment links |
| DOC-07 | Mobile-money provider integrations | Not started | MTN MoMo and Airtel Money capabilities |
| DOC-08 | Bank, card and additional payment methods | Not started | Method-specific requirements and prerequisites |
| DOC-09 | Payouts, withdrawals, refunds and reversals | Not started | Money-out and exception handling |
| DOC-10 | Transaction state machine and failure handling | Not started | Canonical states, retries and timeouts |
| DOC-11 | Ledger and financial integrity | Not started | Immutable records, fees and reconciliation |
| DOC-12 | API contracts and versioning | Not started | Authentication, endpoints, schemas and errors |
| DOC-13 | API keys, IP controls and credential security | Not started | Key lifecycle, scopes, allowlists and rotation |
| DOC-14 | Webhooks and event delivery | Not started | Signing, retries, deduplication and delivery logs |
| DOC-15 | Audit, fraud and risk controls | Not started | Traceability, limits and review gates |
| DOC-16 | Data model and database integrity | Not started | Entities, constraints and migrations |
| DOC-17 | Architecture and service boundaries | Not started | Applications, adapters, queues and data flows |
| DOC-18 | Observability and operations | Not started | Logs, metrics, alerts and incident handling |
| DOC-19 | Requirements-to-tests traceability | Not started | Requirement verification evidence |
| DOC-20 | Test strategy and scenario catalogue | Not started | Unit, contract, integration and recovery tests |
| DOC-21 | Compliance and launch readiness | Not started | Provider, legal and operational prerequisites |
| DOC-22 | Deployment, rollback and disaster recovery | Not started | Environments, secrets, backups and releases |
| DOC-23 | V1 acceptance and release decision | Not started | Final acceptance and explicit release gates |
| DOC-24 | HARAI differentiators and additional feature proposals | Proposed | Original capabilities beyond the JulyPay interface reference |
| DOC-25 | JulyPay read-only discovery log | In progress | Session history, remaining checklist and evidence boundary |
| DOC-26 | Authenticated JulyPay read-only findings | Draft | Authenticated UI observations, HARAI implications and unresolved gaps |
| DOC-27 | Public JulyPay website and commercial-positioning review | Draft | Public product positioning, onboarding, pricing, stated processing relationship and open questions |
| DOC-28 | Funds-flow architecture comparison | Draft research baseline updated with read-only portal observations; no model selected | Compare provider-managed funds, direct integrations, and conditional hybrid approach using official and account-specific evidence |
| DOC-29 | Provider verification and written inquiry pack | Draft — ready for owner review | Formal provider questions, evidence tracker, scoring method and hard gates |
| DOC-30 | Uganda compliance and funds-flow review checklist | Draft — professional review required | Structure qualified legal/compliance review and funds-flow decision gates |
| DOC-31 | Launch requirements traceability and acceptance criteria | Draft — product/independent review required | Stable requirement IDs, testable acceptance criteria, dependencies, and release gates |
| DOC-32 | Documentation consistency and requirements gap review | Draft audit updated; G1 not ready to pass | Cross-document consistency, missing requirements, external blockers and recommended sequence |
| DOC-33 | G1 requirements review package | Draft — G1 not passed | Consolidated proposed launch baseline, decision owners, evidence register, hard gates and approval workflow |
| DOC-34 | Requirement-level traceability matrix | Draft — 54 requirements mapped; provider evidence crosswalk added; roles and tests proposed, not verified | Per-requirement proposed role ownership, test IDs, dependencies, gates, provider evidence and review blockers |
| DOC-35 | Internal cross-document review and corrective-action register | Draft review by ChatGPT; not external certification | Cross-document consistency findings, corrections applied and remaining work before G1/G2 |
| DOC-36 | Product owner decision brief for G1 | Owner-approved defaults POD-01–POD-12; external evidence and G1 gates remain open | Approved product direction for market, merchant eligibility, currency, methods, launch scope, pricing, risk and governance |
| DOC-37 | Provider portal evidence review and owner checklist | Read-only MTN/Airtel portal inspection completed; written eligibility and commercial evidence pending | Account observations, official source register, safe inspection record and evidence limitations |
| DOC-38 | MTN MoMo and Airtel Uganda provider evidence comparison | Draft evidence comparison; no provider/model selected | Consolidated portal findings, capability/access matrix, remaining hard gates and design implications |
| DOC-39 | G1 requirements gap closure batch 1 | Draft — ten supplemental requirements added; G1 remains open | Record gap-closure work, supplemental requirement IDs, dependencies and next traceability actions |
| DOC-40 | G2 architecture and financial-safety baseline | Draft architecture proposal — not approved for implementation | Review component boundaries, transaction state machine, idempotency, callbacks/webhooks, ledger, reconciliation, tenancy, security and funds-flow gates |
| DOC-41 | Transaction state machine and provider mapping | Draft proposal — provider finality/transition contracts unconfirmed | Review internal lifecycle, separate operation families, MTN/Airtel evidence mapping and recovery rules |
| DOC-42 | Ledger and reconciliation design proposal | Draft proposal — finance review required | Review immutable posting invariants, settlement reconciliation, discrepancies and correction workflow |
| DOC-43 | HARAI threat model and security controls | Draft threat model — independent security review required | Review trust boundaries, threat register, proposed controls and required validation evidence |
| DOC-44 | G2 architecture decision and review package | Draft — G2 not approved | Consolidated architecture decisions, funds-flow decision protocol, G2 exit criteria, non-waivable blockers and reviewer roles |
| DOC-45 | Evidence register and acceptance-test planning standard | Draft — evidence gaps open; test IDs are plans only | Evidence classes, provider evidence gaps, minimum test families, evidence acceptance rules and review workflow |
| DOC-46 | G2 gap-closure and ownership matrix | Draft — 32 P0 requirements mapped; owners remain unassigned | Role-level closure work, evidence dependencies, G2 blockers, sequence and closure record template |
| DOC-47 | Detailed architecture review and contradiction register | Draft internal review — G1/G2 remain open; findings are not independent certification | Cross-document architecture findings, severity, closure actions, evidence requirements and approval impact |
| DOC-48 | Provider adapter contract and operation capability matrix | Draft contract — provider authority and live capability remain unconfirmed | Provider-neutral adapter boundary, operation capability evidence, normalized results, retry safety and conformance-test plan |
| DOC-49 | Merchant API specification and developer integration guide | Draft proposal — not implemented or approved | Proposed merchant-server API, authentication/tenant controls, payment-intent contract, idempotency/errors, signed merchant webhooks, sandbox onboarding and open decisions |

## 4. Reference boundary and product differentiation

JulyPay is a visual and merchant-workflow reference only. HARAI will use its own internal architecture, database, payment engine, security model, API contracts, provider adapters and original branding. DOC-24 records additional feature proposals; these remain proposals until assessed and explicitly approved. No hidden JulyPay behavior or provider capability may be inferred from a visible screen.

## 5. Status vocabulary

- **Draft:** proposed, not approved.
- **Under review:** being checked for completeness and consistency.
- **Approved:** explicitly accepted by the accountable decision-maker.
- **Blocked:** cannot proceed until a named question, dependency, authority or evidence requirement is resolved.
- **Unconfirmed:** not verified from available evidence.
- **Superseded:** replaced by a newer identified decision; preserve history.

## 6. Decision register

| ID | Topic | Current position | Status | Next action |
|---|---|---|---|---|
| DEC-001 | Product direction | Reusable, multi-merchant payment gateway with original code/branding and a comprehensive launch ambition | Confirmed direction; requirements still to approve | Define and validate the full launch package |
| DEC-002 | Initial market | Uganda first; regional expansion should remain possible | Confirmed direction | Confirm currency, settlement expectations, and market-specific obligations |
| DEC-003 | Launch ambition | Strong, comprehensive package rather than a deliberately minimal MVP | Confirmed direction | Build a complete capability catalogue and feasibility matrix; do not equate ambition with unverified provider support |
| DEC-004 | Merchant segments | Eventually serve multiple business types, not only ISPs | Confirmed long-term direction | Define target merchant categories and onboarding/permission needs |
| DEC-005 | Collections and money-out | Include both collections and money-out workflows in the planned launch package; include refunds/reversals where provider-supported | Confirmed product direction; provider feasibility unconfirmed | Define requirements and verify each provider's authorized capabilities |
| DEC-006 | Payment methods | HARAI's intended multi-provider package includes both MTN MoMo and Airtel Money (not an either/or choice), card payments through an eligible processor/acquirer, and additional suitable methods | Confirmed product direction; provider/processor feasibility and approval unconfirmed | Verify each provider/method independently: contractual multi-merchant eligibility, technical access, markets/currencies, fees, settlement, lifecycle, security/compliance and test environments; do not enable unsupported paths |
| DEC-007 | Technology stack | NestJS/TypeScript, PostgreSQL/Prisma, Redis, Next.js and pnpm workspace are candidate choices | Proposed | Evaluate against requirements before approval |
| DEC-008 | Tenant isolation | Multi-tenant isolation is required from the beginning | Proposed — high priority | Define boundaries and negative tests |
| DEC-009 | Financial records | Financial movements must be auditable; ledger model is not yet approved | Proposed — high priority | Specify ledger and reconciliation |
| DEC-010 | API credentials | Scoped, revocable credentials with secure storage and rotation | Proposed | Define key lifecycle and display-once behavior |
| DEC-011 | IP allowlisting | Optional API access restriction with safe recovery behavior | Proposed | Define trusted proxy and dynamic-IP behavior |
| DEC-012 | Webhooks | Signed, retryable event delivery with deduplication | Proposed | Define signatures, retry schedule and replay controls |
| DEC-013 | Reference-product evidence | Unknown features remain unconfirmed; observations must be traceable | Approved working rule | Continue read-only discovery |
| DEC-014 | Coding gate | No payment-engine implementation until requirements are reviewed and explicitly authorized | Approved working rule | Keep implementation behind documented gate |
| DEC-015 | Foundation PR | Draft PR #1 stays open and unmerged during discovery | Approved working rule | Review foundation after architecture approval |
| DEC-016 | Funds-flow architecture evaluation | Compare provider-managed funds and direct provider integrations before selecting a model | Confirmed planning preference; no commercial/legal commitment or live activity authorized | DOC-28 compares public evidence; DOC-29 contains the written inquiry pack; obtain official responses and qualified Uganda compliance review |
| DEC-017 | Money-out launch scope | Payouts/withdrawals are intended launch-package capabilities alongside collections; refunds/reversals where supported | Confirmed product direction; live feasibility and authorization unconfirmed | DOC-02 and DOC-03 now reflect the intended scope while retaining provider, legal, funding, reconciliation and operational gates |
| DEC-018 | G1 requirements baseline | DOC-33 consolidates the proposed baseline, unresolved decisions, evidence requirements and G1 hard gates; its Section 12 records a cross-review against all 54 DOC-31 requirement IDs | Draft — product-owner and accountable reviewer decision pending | Record owner decisions and name reviewers; complete requirement-level owner/test/evidence mapping; resolve or explicitly disposition blockers before declaring G1 passed |
| DEC-019 | Requirement-level traceability | DOC-34 maps all 54 DOC-31 requirements to proposed role owners, test IDs, dependencies and gate statuses; no names, tests or verification are claimed as complete | Draft — mapping prepared; owner assignments and evidence pending | Product owner to confirm/amend proposed responsibility split; later map proposed test IDs to actual test artefacts after architecture approval |
| DEC-020 | Cross-document review | DOC-35 records the internal review, corrections and remaining gate boundaries; it is not external certification | Draft review completed; G1 remains not passed | Continue controlled documentation work, correct any inconsistencies found, and preserve explicit gates for owner decisions, provider evidence, professional review and implementation authorization |
| DEC-021 | Product-owner G1 decisions | Product owner approved all recommended defaults POD-01 through POD-12 on 2026-10-19; this is product-direction approval, not provider/legal/production approval | Approved product defaults; G1 not passed | DOC-02/DOC-31 are aligned; continue provider/processor evidence, qualified Uganda legal/compliance review, funds-flow analysis, detailed requirement review and independent review; preserve implementation/provider-contact/merge/deploy gates |
| DEC-022 | Provider account evidence | DOC-37 records owner-assisted read-only portal findings; DOC-38 compares visible access and official technical documentation. No provider contact or account-changing action occurred | Portal inspection completed; written eligibility, contract, production and commercial evidence pending | Continue documentation-based comparison; keep unknowns unscored and do not infer aggregator approval from an API menu or merchant-related endpoint |
| DEC-023 | Approved product defaults | POD-01–POD-12 are approved as the working product direction: Uganda first, controlled broad merchant eligibility, UGX first, both mobile-money providers plus cards/additional methods, collections and money-out, provider-supported refunds/reversals, no funds-flow selection yet, configurable pricing/risk controls, privacy-by-design, measurable reliability, and separate consequential-action gates | Approved product direction; capability feasibility and G1 evidence remain open | Carry approved defaults into requirements/architecture; do not treat them as provider permission, legal clearance, numeric thresholds, processor selection or implementation authorization |

## 7. Open questions and blockers

1. Which exact merchant categories must be supported at launch, and what eligibility rules apply?
2. Which currencies are required at launch, and which settlement arrangements can be supported?
3. Which MTN MoMo and Airtel Money products are accessible under authorized agreements, including written permission for a multi-merchant gateway?
4. Which card processor/acquirer and additional bank/payment methods can be offered under suitable provider agreements, and what are their method-specific fees, settlement terms, security responsibilities and eligibility conditions?
5. Does HARAI only route provider requests, or does it assume custody/settlement responsibilities?
6. What collection, payout, refund, reversal, dispute and settlement capabilities exist per provider?
7. What fees, limits, minimums, taxes and settlement timelines apply?
8. What merchant verification and approval requirements apply before live transactions?
9. Which roles and dual-control approvals are required for money-out operations?
10. What availability, recovery-time, recovery-point and support targets are required?
11. Which legal, regulatory, privacy, retention and contractual reviews are required?
12. What launch package is commercially and operationally sustainable without promising unavailable methods or unsafe financial operations?
13. Which written provider confirmations and qualified Ugandan legal/compliance conclusions are required before selecting a funds-flow model?
14. Which requirement IDs and acceptance criteria does the product owner approve as the launch baseline (DOC-31)?
15. Which open gaps and external blockers in DOC-32 have been resolved, explicitly deferred or accepted with documented rationale?
16. Does the product owner approve, amend or reject the proposed G1 review baseline in DOC-33, and which named reviewers will assess its security, finance/reconciliation, technical, operational and Uganda compliance assumptions?
17. Which named people, if any, are available to fill the technical lead, independent security reviewer, finance/reconciliation reviewer, operations owner and independent requirements reviewer roles listed in DOC-34? If none are appointed yet, leave them unassigned and identify the plan to recruit/engage them.

## 8. Change control

Changes affecting money movement, access control, tenant boundaries, balances, settlement, credential handling, provider selection or launch scope must identify affected requirement/decision IDs, evidence, security and financial impact, tests, and accountable approval. Unresolved authority or evidence gaps remain blockers.

## 9. Current gate

**Discovery note (2026-10-19):** The product owner confirmed a Uganda-first direction, a preference for a strong, comprehensive launch package rather than a deliberately small initial MVP, and inclusion of both collections and money-out workflows in the planned launch package. Refunds and reversals should be specified where provider-supported. HARAI is intended to serve multiple business types over time. The product owner selected **compare provider-managed funds and direct provider integrations before deciding** as the planning approach for funds flow. This is not a final legal/commercial decision and does not authorize provider onboarding, live credentials, handling real funds, payment-engine implementation, or launch. Public JulyPay positioning, onboarding claims, pricing and its stated third-party processing relationship are recorded in DOC-27. Provider capability, commercial access, funds-flow responsibilities, currency/settlement model, workflow behavior, permissions and error/recovery behavior remain unconfirmed.

**G0 — Discovery and requirements: IN PROGRESS. G1 — Requirements approval: NOT PASSED.** DOC-33 consolidates the proposed baseline and hard gates; its Section 12 reviews the 54 DOC-31 requirement IDs. DOC-34 now maps all 54 IDs to proposed role owners and test IDs. No people have been appointed and no test results are claimed. DOC-35 records an internal cross-document review and corrective actions. DOC-36 records product-owner approval of POD-01–POD-12 on 2026-10-19; provider/legal/production gates remain open. The owner has explicitly clarified that both MTN MoMo and Airtel Money are intended, alongside card payments and additional suitable methods; this product direction is recorded in DEC-006, while each method's feasibility and authorization remain unconfirmed. DOC-37 records the owner-assisted read-only inspection of both developer portals; DOC-38 consolidates account observations and official API documentation; DOC-28 incorporates those observations into the still-unscored funds-flow comparison; DOC-32 records the updated gap disposition; DOC-34 links the findings to existing requirement IDs without claiming tests were run or verified. MTN showed active Collections and Collection Widget subscriptions in Test, but no Disbursement subscription or visible production approval. Airtel showed zero live and three archived applications, with relevant APIs documented but no visible production approval. Neither inspection establishes HARAI's contractual multi-merchant eligibility, settlement model, commercial terms or legal permissions. G1 remains open pending accountable review/disposition of all 54 requirements, written provider evidence where required, qualified Ugandan legal/compliance review, funds-flow analysis and complete requirement-level test/evidence mapping. This documentation does not authorize implementation, provider outreach, live credentials, real transactions, merge or deployment.

DEC-016 records the product owner's preference to compare provider-managed funds and direct provider integrations before deciding. DOC-28 contains the public-evidence matrix, weighted comparison criteria, hard gates and inquiry checklist; no provider-specific commercial or eligibility scores are assigned without written evidence. DOC-40 now has supporting design proposals in DOC-41 (state machine/provider mapping), DOC-42 (ledger/reconciliation) and DOC-43 (threat model). DOC-28 compares all three candidate funds-flow models side by side, with unknowns unscored. The next sequence is accountable architecture review and continued evidence collection, not an automatic model selection. DOC-36 product defaults are approved; remaining detailed requirements, provider/legal evidence and accountable reviewer decisions remain open. Any provider outreach requires separate explicit authorization. Complete G1 only after owner decisions, written provider evidence where required, qualified Ugandan legal/compliance review, and requirement-level test/evidence mapping; then complete G2 architecture/security review using DOC-40–DOC-43.
