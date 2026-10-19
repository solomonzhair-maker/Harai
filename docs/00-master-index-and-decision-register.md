# HARAI — Master Index and Decision Register

**Status:** Draft — discovery in progress  
**Last reviewed:** 2026-10-18  
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
| DEC-006 | Payment methods | MTN MoMo and Airtel Money are priority candidates; bank/card methods require validation | Proposed | Verify authorized access, markets, fees and test environments |
| DEC-007 | Technology stack | NestJS/TypeScript, PostgreSQL/Prisma, Redis, Next.js and pnpm workspace are candidate choices | Proposed | Evaluate against requirements before approval |
| DEC-008 | Tenant isolation | Multi-tenant isolation is required from the beginning | Proposed — high priority | Define boundaries and negative tests |
| DEC-009 | Financial records | Financial movements must be auditable; ledger model is not yet approved | Proposed — high priority | Specify ledger and reconciliation |
| DEC-010 | API credentials | Scoped, revocable credentials with secure storage and rotation | Proposed | Define key lifecycle and display-once behavior |
| DEC-011 | IP allowlisting | Optional API access restriction with safe recovery behavior | Proposed | Define trusted proxy and dynamic-IP behavior |
| DEC-012 | Webhooks | Signed, retryable event delivery with deduplication | Proposed | Define signatures, retry schedule and replay controls |
| DEC-013 | Reference-product evidence | Unknown features remain unconfirmed; observations must be traceable | Approved working rule | Continue read-only discovery |
| DEC-014 | Coding gate | No payment-engine implementation until requirements are reviewed and explicitly authorized | Approved working rule | Keep implementation behind documented gate |
| DEC-015 | Foundation PR | Draft PR #1 stays open and unmerged during discovery | Approved working rule | Review foundation after architecture approval |
| DEC-016 | Funds-flow architecture evaluation | Compare provider-managed funds and direct provider integrations before selecting a model | Confirmed planning preference; no commercial/legal commitment or live activity authorized | Build an evidence-based comparison for collections, money-out, settlement, fees, permissions, regulatory responsibilities, operational reliability and multi-merchant support |

## 7. Open questions and blockers

1. Which exact merchant categories must be supported at launch, and what eligibility rules apply?
2. Which currencies are required at launch, and which settlement arrangements can be supported?
3. Which MTN MoMo and Airtel Money products are accessible under authorized agreements?
4. Which bank and card methods can be offered through suitable providers/acquirers?
5. Does HARAI only route provider requests, or does it assume custody/settlement responsibilities?
6. What collection, payout, refund, reversal, dispute and settlement capabilities exist per provider?
7. What fees, limits, minimums, taxes and settlement timelines apply?
8. What merchant verification and approval requirements apply before live transactions?
9. Which roles and dual-control approvals are required for money-out operations?
10. What availability, recovery-time, recovery-point and support targets are required?
11. Which legal, regulatory, privacy, retention and contractual reviews are required?
12. What launch package is commercially and operationally sustainable without promising unavailable methods or unsafe financial operations?

## 8. Change control

Changes affecting money movement, access control, tenant boundaries, balances, settlement, credential handling, provider selection or launch scope must identify affected requirement/decision IDs, evidence, security and financial impact, tests, and accountable approval. Unresolved authority or evidence gaps remain blockers.

## 9. Current gate

**Discovery note (2026-10-19):** The product owner confirmed a Uganda-first direction, a preference for a strong, comprehensive launch package rather than a deliberately small initial MVP, and inclusion of both collections and money-out workflows in the planned launch package. Refunds and reversals should be specified where provider-supported. HARAI is intended to serve multiple business types over time. The product owner selected **compare provider-managed funds and direct provider integrations before deciding** as the planning approach for funds flow. This is not a final legal/commercial decision and does not authorize provider onboarding, live credentials, handling real funds, payment-engine implementation, or launch. Public JulyPay positioning, onboarding claims, pricing and its stated third-party processing relationship are recorded in DOC-27. Provider capability, commercial access, funds-flow responsibilities, currency/settlement model, workflow behavior, permissions and error/recovery behavior remain unconfirmed.

**G0 — Discovery and requirements: IN PROGRESS.** No production integration, live credentials, real payment action, merge of the foundation PR, or deployment is authorized by this draft.
