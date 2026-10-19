# DOC-34 — Requirement-Level Traceability Matrix

**Status:** Draft working matrix — not an approval or verification record  
**Related documents:** DOC-00, DOC-03, DOC-31, DOC-32, DOC-33  
**Branch:** docs/julypay-discovery-and-requirements  
**Purpose:** Make every current DOC-31 requirement actionable by linking it to a proposed accountable role, proposed test/evidence work, dependencies and a gate status.

> **Important:** The owner entries below name roles, not people. No person has been appointed by this document. Test identifiers are proposals for planning; they do not imply tests exist, have run, or passed. Statuses remain Draft until supported by actual decisions and evidence. This matrix does not authorize implementation, provider contact, live credentials, financial activity, merge or deployment.

## 1. Status and traceability rules

- **Draft — not verified:** baseline entry exists; approval or verification evidence is absent.
- **Blocked:** required external permission, provider evidence, professional review or material decision is missing.
- **Approved:** only after explicit accountable approval is recorded.
- **Implemented / Verified:** only after authorized implementation and linked execution evidence; never infer from a test name.
- Role ownership is a proposal for the product owner to assign to named people.
- Proposed test IDs must later be mapped to actual test files/cases, environments, results and evidence links.
- Keep confidential provider contracts, credentials and personal identity records out of this public repository. Store only redacted evidence summaries and controlled references.
- DOC-31 remains the normative source for requirement wording and acceptance criteria. This matrix is the tracking view, not a replacement.

## 2. Requirement-level matrix

The matrix contains 43 unique requirement IDs from the current DOC-31 catalogue. All requirement criteria remain as written in DOC-31; use the requirement ID to trace to the complete acceptance criterion.

| Requirement ID | Priority | Requirement | Proposed accountable role(s) | Proposed test IDs | Dependency / evidence reference from DOC-31 | Gate / disposition | Current status |
|---|---|---|---|---|---|---|---|
| GOV-001 | P0 | Decisions and scope are controlled | Product owner + decision-register steward (person unassigned) | T-GOV-001-ACCEPTANCE, T-GOV-001-NEGATIVE (proposed) | DOC-00; named owner and independent reviewer | G1 safety requirement; design/review required before live operation | Draft — not verified |
| TEN-001 | P0 | All merchant data is tenant-isolated | Technical lead + independent security reviewer (person unassigned) | T-TEN-001-API, T-TEN-001-JOB, T-TEN-001-REPORT (proposed) | Approved tenant model; security review | G1 safety requirement; design/review required before live operation | Draft — not verified |
| MER-001 | P1 | Merchant onboarding has explicit lifecycle states | Product owner + compliance reviewer (person unassigned) | T-MER-001-ACCEPTANCE, T-MER-001-NEGATIVE (proposed) | Provider KYC rules; qualified compliance review | G1 scope decision; implementation/release verification required | Draft — not verified |
| MER-002 | P0 | Live merchant activation is gated | Merchant operations owner + compliance reviewer (person unassigned) | T-MER-002-ACCEPTANCE, T-MER-002-NEGATIVE (proposed) | Provider and compliance evidence; activation policy | G1 evidence dependency + relevant implementation/release gate | Draft — not verified |
| IAM-001 | P0 | Users receive least-privilege access | Security/identity lead + product owner (person unassigned) | T-IAM-001-ACCEPTANCE, T-IAM-001-NEGATIVE (proposed) | Approved roles and permission matrix | G1 safety requirement; design/review required before live operation | Draft — not verified |
| IAM-002 | P0 | Sensitive actions have appropriate approval controls | Security/identity lead + product owner (person unassigned) | T-IAM-002-ACCEPTANCE, T-IAM-002-NEGATIVE (proposed) | Risk policy; legal/provider conditions | G1 safety requirement; design/review required before live operation | Draft — not verified |
| MER-003 | P0 | Merchant verification evidence is handled safely | Merchant operations owner + compliance reviewer (person unassigned) | T-MER-003-ACCEPTANCE, T-MER-003-NEGATIVE (proposed) | Provider requirements; privacy review | G1 evidence dependency + relevant implementation/release gate | Draft — not verified |
| MER-004 | P1 | Merchant suspension and closure are controlled | Product owner + compliance reviewer (person unassigned) | T-MER-004-ACCEPTANCE, T-MER-004-NEGATIVE (proposed) | Compliance, provider and records-retention rules | G1 scope decision; implementation/release verification required | Draft — not verified |
| COL-001 | P1 | A merchant can create a collection request through an authenticated API | Collections product owner + technical lead (person unassigned) | T-COL-001-ACCEPTANCE, T-COL-001-NEGATIVE (proposed) | Approved API contract; provider capability matrix | G1 scope decision; implementation/release verification required | Draft — not verified |
| COL-002 | P1 | Hosted checkout clearly identifies the payment | Collections product owner + technical lead (person unassigned) | T-COL-002-ACCEPTANCE, T-COL-002-NEGATIVE (proposed) | Approved UX, disclosures and provider flow | G1 scope decision; implementation/release verification required | Draft — not verified |
| COL-003 | P0 | Duplicate collection submissions are safe | Technical lead + independent security reviewer (person unassigned) | T-COL-003-IDEMPOTENCY, T-COL-003-CONCURRENCY (proposed) | Transaction and idempotency design | G1 safety requirement; design/review required before live operation | Draft — not verified |
| COL-004 | P0 | Pending and unknown outcomes are handled safely | Technical lead + independent security reviewer (person unassigned) | T-COL-004-TIMEOUT, T-COL-004-UNKNOWN, T-COL-004-LATE-EVENT (proposed) | Provider status APIs and written finality/retry rules | G1 safety requirement; design/review required before live operation | Draft — not verified |
| COL-005 | P0 | Only verified events can change transaction state | Technical lead + independent security reviewer (person unassigned) | T-COL-005-AUTH, T-COL-005-REPLAY, T-COL-005-DUPLICATE (proposed) | Provider callback security specification | G1 safety requirement; design/review required before live operation | Draft — not verified |
| COL-006 | P1 | Payment communications reflect verified state | Collections product owner + technical lead (person unassigned) | T-COL-006-ACCEPTANCE, T-COL-006-NEGATIVE (proposed) | Notification rules; privacy and message templates | G1 scope decision; implementation/release verification required | Draft — not verified |
| COL-007 | P1 | Payment links and collection sessions have bounded lifecycles | Collections product owner + technical lead (person unassigned) | T-COL-007-ACCEPTANCE, T-COL-007-NEGATIVE (proposed) | Approved checkout/payment-link rules | G1 scope decision; implementation/release verification required | Draft — not verified |
| COL-008 | P0 | API abuse is bounded without breaking recovery | Technical lead + independent security reviewer (person unassigned) | T-COL-008-ACCEPTANCE, T-COL-008-NEGATIVE (proposed) | Threat model and API policy | G1 safety requirement; design/review required before live operation | Draft — not verified |
| OUT-001 | P1 / Blocked | Authorized users can initiate supported payouts | Money-out product owner + finance/compliance reviewer (person unassigned) | T-OUT-001-ACCEPTANCE, T-OUT-001-NEGATIVE (proposed) | Written provider permission, legal review, funds-flow and funding-source approval | G1 evidence dependency + relevant implementation/release gate | Draft — not verified |
| OUT-002 | P0 | Payout uncertainty cannot cause duplicate payment | Technical lead + finance/reconciliation reviewer (person unassigned) | T-OUT-002-TIMEOUT, T-OUT-002-UNKNOWN, T-OUT-002-LATE-EVENT (proposed) | Provider idempotency/status/retry documentation | G1 safety requirement; design/review required before live operation | Draft — not verified |
| OUT-003 | P1 / Blocked | Refunds are linked to original collections | Money-out product owner + finance/compliance reviewer (person unassigned) | T-OUT-003-ACCEPTANCE, T-OUT-003-NEGATIVE (proposed) | Written refund support, rules, settlement and accounting review | G1 evidence dependency + relevant implementation/release gate | Draft — not verified |
| OUT-004 | P1 / Blocked | Reversals and returns are recorded without erasing history | Money-out product owner + finance/compliance reviewer (person unassigned) | T-OUT-004-ACCEPTANCE, T-OUT-004-NEGATIVE (proposed) | Provider lifecycle rules and ledger approval | G1 evidence dependency + relevant implementation/release gate | Draft — not verified |
| OUT-005 | P0 | High-risk financial actions are auditable | Technical lead + finance/reconciliation reviewer (person unassigned) | T-OUT-005-ACCEPTANCE, T-OUT-005-NEGATIVE (proposed) | Permission model; privacy/retention review | G1 safety requirement; design/review required before live operation | Draft — not verified |
| OUT-006 | P1 | Beneficiary changes are controlled | Money-out product owner + finance/compliance reviewer (person unassigned) | T-OUT-006-ACCEPTANCE, T-OUT-006-NEGATIVE (proposed) | Provider requirements and risk policy | G1 scope decision; implementation/release verification required | Draft — not verified |
| FIN-001 | P0 | Financial amounts use exact monetary representation | Finance/reconciliation reviewer + technical lead (person unassigned) | T-FIN-001-ACCEPTANCE, T-FIN-001-NEGATIVE (proposed) | Currency policy and finance review | G1 safety requirement; design/review required before live operation | Draft — not verified |
| FIN-002 | P0 | Financial history is append-only/auditable | Finance/reconciliation reviewer + technical lead (person unassigned) | T-FIN-002-APPEND-ONLY, T-FIN-002-COMPENSATION, T-FIN-002-CONCURRENCY (proposed) | Approved ledger model and independent finance review | G1 safety requirement; design/review required before live operation | Draft — not verified |
| FIN-003 | P0 | Fees and net settlement are explainable | Finance/reconciliation reviewer + technical lead (person unassigned) | T-FIN-003-ACCEPTANCE, T-FIN-003-NEGATIVE (proposed) | Provider fee schedule; tax/legal and owner approval | G1 evidence dependency + relevant implementation/release gate | Draft — not verified |
| REC-001 | P0 | Transactions reconcile to provider evidence | Finance/reconciliation reviewer + operations owner (person unassigned) | T-REC-001-MATCH, T-REC-001-MISMATCH, T-REC-001-UNRESOLVED (proposed) | Provider reports/API access; funds-flow model | G1 evidence dependency + relevant implementation/release gate | Draft — not verified |
| REC-002 | P0 | Reconciliation discrepancies cannot be hidden | Finance/reconciliation reviewer + operations owner (person unassigned) | T-REC-002-MATCH, T-REC-002-MISMATCH, T-REC-002-UNRESOLVED (proposed) | Operations policy; finance review | G1 evidence dependency + relevant implementation/release gate | Draft — not verified |
| REP-001 | P1 | Reports and exports are permissioned and reproducible | Product/reporting owner + privacy reviewer (person unassigned) | T-REP-001-ACCEPTANCE, T-REP-001-NEGATIVE (proposed) | Reporting definitions and privacy review | G1 scope decision; implementation/release verification required | Draft — not verified |
| FIN-004 | P1 | Disputes and transaction complaints are trackable | Finance/reconciliation reviewer + technical lead (person unassigned) | T-FIN-004-ACCEPTANCE, T-FIN-004-NEGATIVE (proposed) | Provider dispute/complaint obligations; support policy | G1 scope decision; implementation/release verification required | Draft — not verified |
| FIN-005 | P0 | Financial state transitions are formally specified | Finance/reconciliation reviewer + technical lead (person unassigned) | T-FIN-005-ACCEPTANCE, T-FIN-005-NEGATIVE (proposed) | DOC-10 state-machine design; provider lifecycle evidence | G1 safety requirement; design/review required before live operation | Draft — not verified |
| DEV-001 | P1 | API credentials have a secure lifecycle | Developer-platform lead + security reviewer (person unassigned) | T-DEV-001-ACCEPTANCE, T-DEV-001-NEGATIVE (proposed) | Security architecture and threat model | G1 scope decision; implementation/release verification required | Draft — not verified |
| DEV-002 | P1 | Webhooks are authenticated and recoverable | Developer-platform lead + security reviewer (person unassigned) | T-DEV-002-AUTH, T-DEV-002-REPLAY, T-DEV-002-DUPLICATE (proposed) | Event contract and signing design | G1 scope decision; implementation/release verification required | Draft — not verified |
| SEC-001 | P0 | Authentication and sessions are protected | Independent security reviewer + technical lead (person unassigned) | T-SEC-001-ACCEPTANCE, T-SEC-001-NEGATIVE (proposed) | Security design; identity provider choice | G1 safety requirement; design/review required before live operation | Draft — not verified |
| SEC-002 | P0 | Sensitive information is minimized and protected | Independent security reviewer + technical lead (person unassigned) | T-SEC-002-ACCEPTANCE, T-SEC-002-NEGATIVE (proposed) | Data inventory, privacy review and provider terms | G1 safety requirement; design/review required before live operation | Draft — not verified |
| OPS-001 | P0 | Failures and incidents have owned runbooks | Operations owner + technical lead (person unassigned) | T-OPS-001-ACCEPTANCE, T-OPS-001-NEGATIVE (proposed) | Named operational owners; provider SLA/support | G1 safety requirement; design/review required before live operation | Draft — not verified |
| OPS-002 | P0 | Production release requires evidence-based gates | Operations owner + technical lead (person unassigned) | T-OPS-002-ACCEPTANCE, T-OPS-002-NEGATIVE (proposed) | DOC-03 G5–G8; independent reviewers | Production release gate; later than G1 | Draft — not verified |
| DEV-003 | P1 | API behavior is versioned and diagnosable | Developer-platform lead + security reviewer (person unassigned) | T-DEV-003-ACCEPTANCE, T-DEV-003-NEGATIVE (proposed) | Approved API contract | G1 scope decision; implementation/release verification required | Draft — not verified |
| SEC-003 | P0 | Network and browser access controls are safe | Independent security reviewer + technical lead (person unassigned) | T-SEC-003-CORS, T-SEC-003-CSRF, T-SEC-003-PROXY, T-SEC-003-ALLOWLIST (proposed) | Threat model and deployment topology | G1 safety requirement; design/review required before live operation | Draft — not verified |
| OPS-003 | P0 | Reliability targets and recovery are measurable | Operations owner + technical lead (person unassigned) | T-OPS-003-ALERT, T-OPS-003-RESTORE, T-OPS-003-PENDING-AGE (proposed) | Owner decisions; provider SLAs; operational capacity | G1 safety requirement; design/review required before live operation | Draft — not verified |
| OPS-004 | P1 | Accessibility and responsive operation are verified | Operations owner + technical lead (person unassigned) | T-OPS-004-ACCEPTANCE, T-OPS-004-NEGATIVE (proposed) | UI acceptance plan and accessibility review | G1 scope decision; implementation/release verification required | Draft — not verified |
| EXT-001 | P2 / Blocked | Additional payment methods and currencies | Product owner + relevant provider/compliance reviewer (person unassigned) | T-EXT-001-ACCEPTANCE, T-EXT-001-NEGATIVE (proposed) | Provider contracts and legal review | Conditional extension; blocked until separately approved | Draft — not verified |
| EXT-002 | P2 / Blocked | Scheduling, split settlement and marketplace payouts | Product owner + relevant provider/compliance reviewer (person unassigned) | T-EXT-002-ACCEPTANCE, T-EXT-002-NEGATIVE (proposed) | Separate product decision and feasibility review | Conditional extension; blocked until separately approved | Draft — not verified |
| EXT-003 | P2 / Blocked | Regional expansion | Product owner + relevant provider/compliance reviewer (person unassigned) | T-EXT-003-ACCEPTANCE, T-EXT-003-NEGATIVE (proposed) | Country-specific professional review | Conditional extension; blocked until separately approved | Draft — not verified |

## 3. Evidence and test mapping instructions

For each row, the assigned owner must add the following when known:

1. Named accountable person and backup, where appropriate.
2. Concrete test file/path and case names corresponding to the proposed test IDs.
3. Test level: unit, API, integration, provider contract, security, accessibility, reconciliation, recovery or operational exercise.
4. Preconditions and test-data source. Use synthetic data; do not put real identity documents, live secrets or real payment details in tests.
5. Expected result and actual result, including run date, commit SHA, environment and evidence link.
6. External evidence reference and date, with conclusion/limitations. Missing provider/legal evidence stays missing.
7. Review decision, reviewer identity/role, date and finding disposition.
8. Status: Draft, In review, Approved, Blocked, Implemented, Verified, Deferred or Rejected, consistent with DOC-31/DOC-33.

A proposed test ID is not a test result. An API mock is not evidence that a provider supports the behavior. Provider sandbox behavior is not by itself proof of production eligibility or contractual permission.

## 4. Current cross-cutting blockers

- **People and accountability:** product owner must assign named technical, security, finance/reconciliation, operations and independent reviewers; qualified Ugandan legal/compliance review is still needed.
- **Provider eligibility:** written evidence is needed for multi-merchant/aggregator permission, supported methods, payout capabilities, refunds/reversals, fees, limits, settlement and support.
- **Funds flow:** account ownership, custody/control, settlement route, payout funding source, returns and reconciliation evidence remain unresolved.
- **Commercial/legal:** prices, taxes, reserves, limits, merchant eligibility, data retention and applicable obligations require evidence and relevant review.
- **Implementation/test evidence:** no test IDs in this document should be read as tests already implemented or run. Actual code work remains behind the requirements/architecture and explicit authorization gates.
- **G1:** remains NOT PASSED until the DOC-33 hard gates are evidenced and decisions are recorded.

## 5. Proposed owner assignment register

No names have been provided or inferred. Fill this register only when the product owner confirms assignments.

| Role | Named person | Confirmed available? | Independence/conflict check | Assigned requirement IDs | Status |
|---|---|---|---|---|---|
| Product owner | Pending | Pending | N/A | GOV-001 and scope/decision approvals | Unassigned |
| Technical lead | Pending | Pending | Must not self-certify independent reviews | Technical requirements across catalogue | Unassigned |
| Independent security reviewer | Pending | Pending | Independent of the work reviewed | TEN-001, IAM-001–IAM-002, COL/DEV/SEC/OPS controls as applicable | Unassigned |
| Finance/reconciliation reviewer | Pending | Pending | Must independently review financial invariants and reconciliation | FIN-001–FIN-005, REC-001–REC-002, payout/refund flows | Unassigned |
| Operations/support owner | Pending | Pending | Must have authority to own runbooks and incidents | OPS-001–OPS-004 and operational aspects across flows | Unassigned |
| Qualified Ugandan legal/compliance professional | Pending | Pending | Qualified for the relevant advice; scope and date recorded | MER, funds flow, provider eligibility, privacy, tax and launch obligations | Unassigned |
| Independent requirements reviewer | Pending | Pending | Separate from document author/implementer | Entire G1 package and evidence | Unassigned |

## 6. Decision and review queue

The following are the next actions that can be completed without pretending decisions have been made:

1. Review proposed role ownership and test IDs; change them to fit the eventual implementation architecture.
2. Assign named reviewers and record availability/independence.
3. Resolve the product-owner scope decisions in DOC-33, including merchant categories, currencies, fee/pricing approach and launch exclusions.
4. Only after explicit authorization, obtain written provider responses and record evidence in controlled storage with redacted summaries in the repository.
5. Arrange qualified Ugandan legal/compliance review of actual candidate funds-flow diagrams.
6. Convert proposed test IDs to actual tests after requirements and architecture approval, then attach results only when tests are run.
7. Reconcile this matrix with DOC-31 and DOC-00 after each decision batch.

## 7. Current disposition

**G1 NOT PASSED.** This matrix makes the traceability work visible but does not close external evidence gaps or replace the product owner's decisions. The immediate next action requiring owner input is a small set of product decisions and the identity/availability of reviewers; provider outreach and professional advice must not be represented as completed.