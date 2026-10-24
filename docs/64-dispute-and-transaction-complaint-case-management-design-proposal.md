# DOC-64 — Dispute and Transaction Complaint Case-Management Design Proposal

**Status:** Draft proposal for requirements/design review — not approved, implemented, legally validated, or verified.  
**Created:** 2026-10-24  
**Canonical branch:** `docs/julypay-discovery-and-requirements`  
**Related requirements:** DOC-31 FIN-004 and SUP-005; MER-004; OUT-005; REC-002; SEC-002; SUP-010.  
**Related finding:** DOC-47 AR-022 (open — design missing).  
**Purpose:** Define a reviewable case-management design for disputes and transaction complaints without inventing provider/legal deadlines, selecting a funds-flow model, or authorizing engineering or financial actions.

## 1. Authority and safety boundaries

This document proposes product behavior and design questions only. It does not:
- establish that MTN, Airtel, a card processor, or any other provider offers a specific dispute/chargeback process or grants HARAI authority to act;
- set legal, regulatory, card-scheme, provider, or contractual response deadlines;
- select a funds-flow model, determine who legally owes or controls funds, or approve an accounting treatment;
- authorize refunds, reversals, payouts, ledger postings, provider contact, production access, tests, implementation, or live financial activity;
- replace qualified Uganda legal/compliance, privacy, finance/accounting, security, provider, or operations review.

**Hard rule:** a case record is not itself a financial instruction. Creating, assigning, messaging, escalating, resolving, or closing a case must not silently change a payment, ledger entry, settlement, refund, reversal, payout, or provider transaction. Any consequential financial operation must use its own separately authorized operation, capability checks, idempotency, approvals, provider evidence, and append-only accounting trail.

Unverified external deadlines must never be guessed. Until an attributable provider/processor/legal source and accountable owner confirm a deadline rule, affected cases must be marked **deadline rule unverified** and routed to a human for assessment; the system must not display an invented “compliant” due date.

## 2. Goals and non-goals

### 2.1 Goals
1. Give every in-scope dispute or transaction complaint a durable, unique case ID and an accountable human/team owner.
2. Link the case to the original HARAI transaction and relevant provider/settlement references without treating those references as proof of a financial outcome.
3. Preserve a time-ordered history of status changes, assignments, deadlines, evidence references, communications, decisions and outcomes.
4. Make deadlines, overdue items, external dependencies and escalations visible and auditable.
5. Restrict sensitive case data and attachments to authorized, tenant-scoped users; record privileged access.
6. Separate investigation and communication from separately authorized financial operations.
7. Support reopen/correction workflows without erasing history.
8. Provide measurable acceptance criteria for FIN-004 and SUP-005, subject to specialist review.

### 2.2 Non-goals
- Automatically deciding whether a customer, merchant, provider or HARAI is liable.
- Automatically accepting or contesting a card chargeback or provider dispute without a separately approved process.
- Automatically issuing refunds, reversals, payouts, settlement adjustments or ledger corrections.
- Promising a resolution time before support capacity and external deadlines are verified.
- Using case notes as a substitute for provider-authoritative transaction/settlement evidence.
- Treating reconciliation discrepancy management as a full complaint/dispute workflow.

## 3. Case scope and intake

### 3.1 Proposed case categories
Use a controlled, versioned category list; initial candidates are:
- transaction complaint (payment not received/recognized, wrong amount, duplicate allegation, payment status disagreement);
- provider or processor dispute/chargeback, only for a supported and authorized method;
- refund/reversal complaint, distinct from the refund/reversal operation itself;
- settlement or payout complaint, only where the relevant capability is authorized;
- suspected fraud or unauthorized transaction;
- other transaction-related complaint requiring triage.

Categories are proposals. Product, support, provider and compliance owners must approve the actual taxonomy and provider/method applicability.

### 3.2 Intake sources
Potential sources are merchant-submitted support forms, authorized HARAI support-agent intake, and verified provider/processor dispute notifications where that integration is explicitly approved. Each source must record source type, source reference, received timestamp, and the identity/role of the submitting actor where known.

Do not claim automated provider intake exists until product-specific provider evidence, interface specifications and implementation are verified. For manual intake, the operator must record the original source and reference without representing copied text as provider-authenticated evidence.

### 3.3 Required intake fields
- immutable case ID;
- tenant/merchant ID derived from trusted server-side identity or validated source mapping;
- category and concise reason code;
- source type and source reference;
- related HARAI transaction ID, where known; provider/processor transaction reference stored separately and marked with provenance;
- received-at timestamp and actor/source attribution;
- initial priority and rationale;
- assigned owner/team or an explicit **unassigned — triage required** state;
- deadline state: unassessed, verified, not applicable with reason, or rule unverified;
- evidence references and data-classification labels, not unrestricted raw payload copies;
- status and status-history entry.

If no transaction can be reliably matched, the case may be created as **unlinked — matching required**. Do not invent a transaction link. Record the matching attempt, evidence and responsible owner. A material identity/tenant mismatch must fail closed and be escalated; do not expose another merchant's transaction to resolve ambiguity.

## 4. Proposed lifecycle and transition rules

These are proposed logical states; exact provider-specific states must be mapped separately and must not be conflated with payment transaction states.

| Case state | Meaning | Permitted next states (proposal) |
|---|---|---|
| `NEW` | Intake recorded; triage not completed | `TRIAGE`, `CANCELLED_DUPLICATE` |
| `TRIAGE` | Validate category, tenant, transaction link, priority and deadline source | `OPEN`, `WAITING_EXTERNAL`, `ESCALATED`, `CANCELLED_DUPLICATE` |
| `OPEN` | Owned and actively investigated | `WAITING_CUSTOMER`, `WAITING_MERCHANT`, `WAITING_PROVIDER`, `ESCALATED`, `RESOLUTION_PROPOSED` |
| `WAITING_CUSTOMER` | Action or evidence requested from customer/complainant | `OPEN`, `ESCALATED`, `RESOLUTION_PROPOSED` |
| `WAITING_MERCHANT` | Action or evidence requested from merchant | `OPEN`, `ESCALATED`, `RESOLUTION_PROPOSED` |
| `WAITING_PROVIDER` | Provider/processor response or evidence is pending | `OPEN`, `ESCALATED`, `RESOLUTION_PROPOSED` |
| `WAITING_EXTERNAL` | External dependency exists but does not fit a more specific waiting state | `OPEN`, `ESCALATED`, `RESOLUTION_PROPOSED` |
| `ESCALATED` | Routed to a higher authorized support, risk, compliance, finance or operational role | `OPEN`, an applicable waiting state, `RESOLUTION_PROPOSED` |
| `RESOLUTION_PROPOSED` | Findings and proposed outcome recorded; required approval/confirmation pending | `OPEN`, an applicable waiting state, `RESOLVED` |
| `RESOLVED` | Outcome recorded with evidence and required approval | `REOPENED`, `CLOSED` |
| `REOPENED` | Previously resolved case reopened with reason and actor | `OPEN`, `ESCALATED`, an applicable waiting state |
| `CLOSED` | Administrative closure after required checks; history remains retained under approved policy | `REOPENED` only through authorized, audited reopen flow |
| `CANCELLED_DUPLICATE` | Duplicate case linked to a surviving canonical case | `CLOSED` after cross-link and review |

**Transition invariants**
- Every transition records prior state, new state, actor/service identity, timestamp, reason, and relevant evidence reference.
- Invalid transitions are rejected server-side; UI hiding alone is not authorization.
- A duplicate case is never silently deleted. It links to the surviving case and preserves its own history.
- Waiting states require a recorded dependency, responsible party, next-review date or an explicit reason that the date cannot yet be established.
- Escalation records the reason, target role/team, timestamp, acknowledgement where available, and follow-up responsibility.
- Resolution requires a structured outcome, rationale, supporting evidence references, communication disposition and required reviewer approval.
- Closing a case never deletes its events or changes financial records. Reopening preserves the previous resolution and records why it is no longer sufficient.
- If provider evidence conflicts or a material financial state is uncertain, keep the case open/escalated and the financial state unresolved under the approved reconciliation/operation rules. Do not resolve uncertainty by overwriting evidence.

## 5. Ownership, deadlines and escalation

### 5.1 Ownership
Each active case must have one accountable owner role/team at a time, with optional supporting participants. Role labels are not named-person assignments. The operating model must name actual accountable people/rotations and backup coverage before production.

Proposed role responsibilities:
- **Support/complaints owner:** intake quality, customer/merchant communication, assignment, follow-up and case completeness.
- **Provider operations owner:** provider evidence, provider reference verification and provider escalation through an authorized channel.
- **Finance/reconciliation reviewer:** assessment of settlement/ledger implications and review of any separately proposed financial correction.
- **Risk/fraud owner:** suspected abuse/fraud triage and approved restricted-access handling.
- **Compliance/legal reviewer:** applicable complaint obligations, external deadlines, required notices, retention/hold and escalation rules.
- **Security/privacy owner:** access controls, sensitive evidence, incident escalation and data-minimization rules.
- **Independent approver:** required where policy calls for maker-checker or independent approval; must not be the initiator where separation is required.

### 5.2 Deadline source of truth
Create a separately governed **deadline-rule registry** before production. Each rule must include:
- rule ID and version;
- provider/processor, method, country/jurisdiction, case category and triggering event;
- source type, source document/URL or contract reference, source date/version and evidence location;
- start/stop/pause/resume calculation semantics, timezone, business-day/calendar-day convention and holiday calendar where applicable;
- response/submission deadline and any internal target, clearly distinguished;
- accountable owner and qualified reviewer;
- effective date, expiry/review date, superseded rule and audit history.

No numeric external SLA is proposed in this document. Legal/provider/contractual time limits must be supplied from authoritative sources and reviewed by qualified personnel. Internal service targets must be separately approved based on actual staffing and operations capacity. Until both the relevant rule and its calculation are approved, show **deadline rule unverified**; require prompt human triage and escalation rather than fabricating a due date.

### 5.3 Escalation triggers
Candidate triggers, subject to policy approval:
- deadline rule missing or cannot be confidently applied;
- case approaching an approved deadline or becoming overdue;
- high-impact or repeated complaint, suspected fraud, unauthorized access, data incident or safety concern;
- contradictory provider evidence, uncertain transaction finality, material reconciliation break or requested financial action;
- no owner, owner unavailable, missed internal follow-up or external dependency with no response;
- access request for sensitive evidence or a conflict of interest involving the current owner.

Each trigger must route to a defined role/team, preserve the reason and timestamp, and remain visible until acknowledged or otherwise dispositioned. Routing destinations and escalation time thresholds remain open decisions; do not invent people, queues or provider contacts.

## 6. Evidence and communications

### 6.1 Evidence references
Store evidence as controlled references to approved records/files, with metadata:
- evidence ID, case ID and related transaction/provider reference;
- evidence type, source/provenance, received/created time, uploader/collector and verification status;
- content integrity/hash where appropriate and approved;
- sensitivity classification, access policy, retention class and any legal hold;
- relationship to a finding or proposed outcome.

Preserve original evidence where required; annotations or redactions create new derived versions with linkage rather than silently replacing originals. A hash demonstrates content identity, not authenticity or truth. Evidence imported from a provider must be labelled according to the actual verification performed.

Do not put API secrets, authentication tokens, full card numbers, sensitive authentication data, unnecessary personal data, or unrestricted raw provider payloads into case notes, analytics or general logs. Attachments, exports and diagnostic artefacts require the same access/redaction/retention controls as the primary case record.

### 6.2 Communication history
Each inbound/outbound communication event should record channel, actor, recipient role/address reference as permitted, timestamp, template/version if used, delivery/receipt state where verifiable, case/transaction reference, and a controlled content or evidence reference. Clearly distinguish drafted, submitted, sent, delivered, acknowledged and failed states. Never claim a message was sent or delivered based solely on a draft or a UI action without reliable outcome evidence.

Approved templates, languages, notices, identity-verification steps and communication channels are separate product/compliance decisions. Do not expose one merchant's case details to another tenant or an unverified requester.

## 7. Resolution and financial-operation separation

A case resolution must use a structured outcome taxonomy, to be approved by support, finance and compliance owners. Initial candidate outcomes:
- information provided / no financial change;
- complaint not substantiated, with rationale and evidence;
- merchant/provider action pending or completed, with independent reference;
- separately authorized refund/reversal/other operation initiated, pending, succeeded, failed or unknown;
- duplicate case linked to canonical case;
- referred to provider/processor or qualified compliance/legal reviewer;
- unable to determine / remains unresolved.

These are case outcomes, not transaction statuses. If a financial operation is considered:
1. Create or reference the separate operation through its approved workflow.
2. Verify provider capability, contractual permission, merchant/method/currency eligibility, funding and limits, identity/authorization, approvals and idempotency.
3. Record the operation ID and evidence in the case without copying secrets.
4. Keep the case outcome aligned with verified operation/provider evidence; pending or unknown operation outcomes must remain pending/unknown.
5. Preserve original financial records and use approved linked compensating entries for any correction; never write directly to ledger state from the case-management path.

No such financial operation is authorized by this design proposal.

## 8. Access control, tenant isolation and audit

Proposed minimum controls:
- Derive tenant scope from trusted server-side identity and validated persisted ownership; never trust a client-supplied merchant ID as sufficient authority.
- Enforce authorization on every case read, search, attachment, export, assignment, transition, communication and evidence download.
- Apply least privilege by role and case assignment; restrict fraud, identity and sensitive provider evidence to explicitly authorized roles.
- Elevated support access must be case-bound, reasoned, scoped, time-limited and auditable, consistent with DOC-51/DOC-56/DOC-59.
- Search, counts, notifications and exports must not leak the existence or details of another tenant's cases.
- Audit case creation, viewing of restricted evidence, export, assignment, deadline edits, status transitions, communications, evidence changes, resolution, closure, reopening and privileged access.
- Keep audit events append-only under an approved integrity policy; corrections are additional events, not silent edits.
- Apply approved retention, deletion, legal-hold, backup and restore rules. Exact periods and legal-hold rules remain unapproved.
- Case records and private evidence must not enter offline caches unless a separately reviewed policy explicitly allows the particular data; default is no private evidence offline.

## 9. Proposed logical data entities

These are conceptual entities, not a physical schema or migration specification.

| Entity | Purpose | Key relationships / controls |
|---|---|---|
| `Case` | Current searchable projection of case identity, tenant, category, priority, status, owner and resolution summary | Immutable ID; tenant scope; created/updated metadata; current state derived only through validated transitions |
| `CaseEvent` | Append-only timeline of transitions, assignments, escalation, deadline changes, communications and decisions | Case ID; actor; timestamp; event type; reason; prior/new values where applicable |
| `CaseTransactionLink` | Links a case to zero or more validated HARAI transactions and external references | Link type, provenance, match confidence/state, verifier and time; no guessed links |
| `CaseEvidence` | Metadata/reference for evidence and controlled derived versions | Source, integrity metadata, sensitivity, access and retention class |
| `CaseCommunication` | Communication attempts and outcomes | Channel, actor, recipient reference, timestamp, status, approved content/evidence reference |
| `CaseAssignment` | Ownership and supporting team/participant history | Role/team, assignment actor/time, start/end, reason |
| `CaseDeadline` | Deadline rule applied to a case, its source/version and calculation state | Rule registry ID/version; source; calculated timestamp only after approved rule; uncertainty/exception state |
| `CaseEscalation` | Escalation event, target role/team, reason, acknowledgement and outcome | Links to case event; no invented target destination |
| `CaseOperationLink` | Links to a separately authorized refund/reversal/other operation | Operation ID/type/state/evidence reference; cannot directly mutate the operation or ledger |
| `CaseRuleVersion` | Versioned category, deadline, escalation and resolution rules | Effective dates, approver/reviewer, source and supersession history |

Physical storage, constraints, indexing, encryption, retention, deletion/hold, access-policy implementation and transaction boundaries require G2 architecture/security/privacy/finance review. This proposal does not select PostgreSQL table layouts, event-sourcing implementation, RLS, queues or a particular service topology.

## 10. Notifications, reporting and operational views

Proposed views:
- **My active cases:** assigned cases, next action, deadline state and overdue/escalation indicators.
- **Unassigned triage queue:** new cases, missing transaction links, missing deadline rules and intake-quality issues.
- **Waiting on external party:** waiting state, last verified contact event, next review and escalation status.
- **Deadline risk:** only cases with an approved deadline calculation; separate “rule unverified” queue so unknown deadlines are not hidden.
- **Escalations:** target role/team, acknowledgement, owner and resolution state.
- **Case audit timeline:** readable history with links to controlled evidence and operation references.
- **Management reporting:** counts by category/status/age/owner, reopened cases, overdue cases under approved rules, unresolved cases and outcomes. Enforce tenant scope and data minimization; do not expose sensitive case contents in aggregate views.

Notifications must be based on persisted case events and disclose only the minimum necessary information. Delivery outcomes must be recorded accurately; retries must not create duplicate case transitions or falsely claim successful delivery.

## 11. Open decisions and evidence required

The following are intentionally unresolved and block production readiness:
1. Approved complaint/dispute taxonomy and which methods/providers support each category.
2. Provider/processor-specific case intake, required evidence, response channels and authoritative deadlines.
3. Qualified Uganda legal/compliance review of complaint obligations, notices, escalation, privacy, retention and recordkeeping.
4. Named support/operations owner, backup coverage, service hours and escalation destinations.
5. Finance/accounting review of outcomes that may relate to refunds, reversals, settlements or ledger corrections.
6. Security/privacy review of case evidence classification, access, attachments, exports, retention/deletion/legal hold and audit integrity.
7. Case priority definitions, internal service targets, deadline-warning windows and business-calendar rules.
8. Approved communication templates, language support, verification and delivery semantics.
9. Duplicate detection policy and safe manual resolution when transaction matching is ambiguous.
10. Physical data model, authorization enforcement, migration, backup/restore and operational monitoring.
11. Exact acceptance evidence and independent reviewer appointments.

No named person, deadline, provider destination, contractual obligation or numeric SLA is supplied here because the reviewed source material does not establish it.

## 12. Acceptance criteria proposed for FIN-004 and SUP-005

The following are design/verification criteria to refine after specialist review; test IDs remain proposals and no tests have been run.

- **CASE-001 — Intake integrity:** a case receives a unique ID and source attribution; missing/ambiguous transaction references remain explicitly unlinked rather than guessed.
- **CASE-002 — Tenant isolation:** a user cannot read, search, export, attach evidence to or infer another tenant's case without explicit approved cross-tenant authority.
- **CASE-003 — Ownership:** every active case has an owner/team or is visibly in an unassigned triage state with escalation handling.
- **CASE-004 — Transition control:** allowed transitions succeed; invalid transitions fail server-side and leave an auditable rejection.
- **CASE-005 — Deadline provenance:** an external deadline is calculated only from an approved, versioned rule with known trigger semantics; absent/ambiguous rules show “deadline rule unverified” and route for human triage.
- **CASE-006 — Overdue/escalation:** approved deadline warnings and overdue events create auditable escalation to a verified role/team; unknown routing is not represented as successful handoff.
- **CASE-007 — Evidence integrity/privacy:** evidence provenance, access and derived versions are tracked; prohibited secrets/sensitive authentication data are rejected or redacted under approved policy.
- **CASE-008 — Communication history:** drafted, submitted, sent, delivered, failed and acknowledged states are distinguished according to available evidence.
- **CASE-009 — Financial separation:** case creation, status change, resolution and closure do not mutate transaction or ledger records or trigger financial operations.
- **CASE-010 — Linked operation traceability:** separately authorized financial operations are linked by ID; pending/unknown results cannot be represented as successful resolution.
- **CASE-011 — Resolution/reopen:** resolution requires structured outcome, rationale, evidence and applicable approval; reopening preserves the prior decision and records a reason.
- **CASE-012 — Audit and retention:** sensitive access, exports, assignment, deadlines, transitions and closure are attributable and retained under an approved policy.
- **CASE-013 — Duplicate handling:** duplicate cases link to a canonical case without deleting either history or hiding unresolved work.
- **CASE-014 — Suspension/closure interaction:** merchant suspension/closure does not erase or orphan open cases; authorized owners and access remain governed by approved policy.
- **CASE-015 — Recovery:** after restore, case events, evidence references, notifications and linked operation states are reconciled before consequential workers resume, under the approved restore/replay authority.

## 13. Review and closure workflow

1. Support/operations owner reviews lifecycle, roles, intake, customer/merchant communication and service coverage.
2. Provider integration owner supplies attributable provider/processor-specific capability, source and deadline evidence through an authorized process.
3. Qualified Uganda legal/compliance adviser validates obligations, deadlines, notices, retention/hold and escalation.
4. Finance/accounting reviewer validates separation from refund/reversal/settlement/ledger processes and correction authority.
5. Security/privacy reviewer validates tenant isolation, access, evidence handling, exports, audit and retention.
6. Technical architecture owner reconciles the conceptual entities and transitions with DOC-42 and the canonical data/idempotency/operation contracts, then records physical-design decisions in the appropriate architecture documents.
7. Update DOC-31/DOC-34/DOC-46/DOC-63 with exact references and review outcomes. Update AR-022 only when the missing design and required evidence are actually reviewed; document creation alone does not close it.
8. Only after G1/G2 criteria and separate explicit G3 authorization may implementation planning proceed. Any tests, code, provider contact, credentials, financial activity, merge or deployment require the applicable authorization and gate.

## 14. Current disposition

- FIN-004 and SUP-005 remain **accepted in scope, design not yet approved, not implemented, not verified**.
- AR-022 remains **OPEN — design missing / review pending** until evidence and required reviews justify a status change.
- G1 remains **NOT PASSED**; G2 remains **NOT APPROVED**.
- This proposal does not authorize implementation, tests, provider outreach, credentials, financial activity, merge or deployment.
