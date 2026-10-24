# DOC-69 — Preliminary Privacy Data Inventory and Control-Mapping Register

**Status:** Preliminary design/evidence worksheet — not approved, not legally validated, not a verified inventory of production processing.  
**Created:** 2026-10-24  
**Canonical branch:** `docs/julypay-discovery-and-requirements`  
**Related documents:** DOC-47 (architecture review and contradiction register), DOC-51/DOC-56/DOC-59 (security/access-control materials as cross-referenced by DOC-64), DOC-62 (remediation plan), DOC-64 (draft complaint/dispute case-management proposal), DOC-66 (specialist research programme), DOC-67 (preliminary obligation/evidence matrix), DOC-68 (qualified reviewer question pack), DOC-70 (cross-document validation and gap register).  
**Purpose:** Convert the known proposed HARAI gateway and complaint-case workflows into a reviewable data-inventory worksheet, identify privacy/security control questions, and list the evidence needed before a privacy design or retention policy can be approved.

> This document does not claim that every listed data category is currently collected, that HARAI has a particular legal role or registration status, or that a given legal basis/retention period has been selected. Entries are candidate categories inferred from the documented proposed workflows and must be confirmed against actual UI fields, API contracts, provider messages, hosting, logs, contracts and operational procedures. Do not treat a candidate as a requirement to collect the data.

## 1. Authority and status

- This is an internal design worksheet, not a privacy impact assessment, legal opinion, DPO determination, regulatory filing, production data map or implementation authorization.
- HARAI's legal entity, controller/processor/collector roles, PDPO registration/renewal status, provider contracts, funds flow, live data locations, subprocessors and production data collection are not established by this document.
- The DOC-64 complaint/dispute workflow remains a draft proposal. Its concepts (tenant-scoped access, controlled evidence references, restricted attachments, append-only audit history, no private evidence in offline caches by default) are candidate design constraints, not verified controls.
- Any actual personal data must be mapped before production approval. Where a field, purpose, recipient, legal basis, location or retention period is unknown, record **UNVERIFIED** rather than inventing it.
- Source-led candidate duties are drawn from the Data Protection and Privacy Act, 2019 (DPPA), including sections 12–16, 18–19, 23 and 29, and the Data Protection and Privacy Regulations, 2021, including regulations 15–27, 29–33, 40–41 and 50. Current applicability and interpretation require qualified Ugandan privacy counsel/DPO review.
- Official sources:
  - [DPPA consolidated text on ULII](https://ulii.org/en/akn/ug/act/2019/9/eng%402023-12-31)
  - [Official PDPO-hosted DPPA Regulations 2021 PDF](https://pdpo.go.ug/media/2022/03/Data_Protection_and_Privacy_Regulations-2021.pdf)
  - [PDPO annual compliance report guidance](https://pdpo.go.ug/media/2024/01/Guidance-Note-on-Completion-of-the-Annual-DPP-Compliance-Report.pdf)
  - [PCI Security Standards Council — PCI DSS](https://www.pcisecuritystandards.org/standards/pci-dss/)

## 2. Candidate processing inventory

The categories below are candidates to verify, not claims that HARAI currently processes every item. “Purpose” is a proposed purpose statement to be validated, not a final lawful-basis conclusion.

| Processing area | Candidate data / metadata | Proposed purpose to validate | Key controls / evidence needed | Open questions |
|---|---|---|---|---|
| Account and authentication | User/merchant identifier, name/contact details, role, tenant/merchant association, login/security events, MFA/recovery metadata | Account administration, authentication, access control and security | Data-field inventory; privacy notice; least privilege; secure credential handling; access logs; account recovery abuse controls | Which fields are essential? Who is controller for each field? What identity assurance is proportionate? |
| Merchant onboarding and due diligence | Legal/business name, registration details, authorized representative/contact, review status, supporting-document references, screening outcomes if applicable | Establish merchant identity, authority and eligibility where required | Collect only fields required by confirmed policy/provider/legal obligation; restrict documents; record source and reviewer; separate decision metadata from raw documents | What evidence is actually required by provider/contract/law? Are identity documents needed? Who may view them? |
| Payment initiation and transaction status | HARAI transaction ID, merchant order/reference, amount/currency, payment method, provider reference, timestamps, status, error/retry/idempotency metadata | Initiate and track a requested transaction; reconcile authoritative states | Minimize payloads; validate tenant ownership; avoid secrets in logs; access-controlled audit trail; define authoritative status/source | Does any provider payload contain direct identifiers or extra personal data? What is the minimum reconciliation key? |
| Provider API and webhook messages | Provider event ID, signature/verification result, event timestamp, transaction reference, event type, selected status fields, delivery/replay metadata | Authenticate, deduplicate and process provider events | Verify signature/authenticity; store required normalized fields; tightly control raw payload retention; redact secrets; replay-safe audit trail | Which raw fields must be preserved for dispute/audit? Which can be discarded after normalization? |
| Settlement and reconciliation | Settlement/batch references, merchant/provider amounts, fees, adjustments, timestamps, discrepancy records, operator decisions | Reconcile transaction and settlement records | Segregate financial evidence from general support notes; preserve correction history; restrict exports; retain only under approved legal/accounting rules | Which documents/periods are legally or contractually required? What is the selected funds-flow model? |
| Complaint/dispute case | Case ID, linked transaction/provider reference, category, received time, complainant role, tenant, assigned team, status, communications references, outcome/rationale | Investigate and track complaints/disputes without silently changing a financial state | Tenant-scoped authorization; controlled status history; no invented deadlines; accessible intake; evidence provenance; approved notice and escalation rules | Which legal/provider case types and clocks apply? What is the verified complainant identity standard? |
| Case evidence and attachments | Evidence ID/reference, source/provenance, uploader, time, verification status, sensitivity label, retention class, legal hold; attachment content only where justified | Support investigation, audit and decisions | Prefer references over copied raw data; malware/file controls if implemented; encryption/access restrictions; redaction/derived versions; access/export audit | Are attachments necessary? Which file types and size limits? Who can download? What retention/hold applies? |
| Communications | Channel, recipient role/address reference, sender/actor, timestamp, template/version, delivery state, controlled content reference | Communicate case status and requests | Validate recipient and authorization; minimize content; avoid exposing another tenant's data; distinguish draft/sent/delivered/acknowledged; use approved templates | Which channels are approved? Are full message bodies needed or is a secure reference sufficient? |
| Security, access and audit logs | Actor/service ID, tenant context, action, resource, timestamp, outcome, correlation ID, source IP/device metadata where justified | Detect misuse, investigate incidents, establish accountability | Data minimization; restrict log access; redact secrets/personal content; protect integrity; approved log retention and alerting | Which fields are necessary for fraud/security and for how long? Do logs cross borders or enter vendor systems? |
| Support and privileged access | Support actor, ticket/case ID, reason, approval, scope, start/end, viewed/exported resource, access outcome | Time-limited support and incident resolution | Case-bound least privilege, time limit, reason, approval, access logging and review; no broad standing access without documented justification | Which support vendors/personnel can access data and from which countries? |
| Analytics, product telemetry and monitoring | Event names, performance/error metrics, usage counts, device/session metadata, potentially identifiers | Reliability, capacity and product improvement | Prefer aggregated/pseudonymized metrics; exclude payment credentials, message bodies and raw payloads; document recipients and locations | Are analytics SDKs or third-party telemetry planned? Can the use case work without user-level tracking? |
| Backups, archives and recovery | Copies of in-scope databases/files/logs, backup metadata, restore logs, encryption/access keys metadata | Resilience and recovery | Inventory locations/subprocessors; encrypt and restrict access; define backup expiry, deletion propagation, legal hold and restore controls; prevent unsafe replay | What are the actual backup regions, provider terms, recovery objectives and deletion behavior? |
| Notifications and optional marketing | Contact endpoint, notification preference, delivery status, template, consent/objection record if relevant | Essential service notices and separately authorized optional communications | Separate service messages from marketing; use minimal content; record preferences and lawful basis where required; provide opt-out where applicable | Is marketing in scope at all? Which legal rules and consent/preferences apply? |
| Developer projects, API credentials and webhook configuration (conditional) | Project/key reference, scopes, status, creation/rotation/revocation events, endpoint URL and configuration metadata; **never the secret value** | Operate and secure merchant integrations, if in scope | Separate secret material from ordinary metadata; secret redaction; scoped/revocable credentials; endpoint validation; restrict configuration and access history | Which fields exist in actual UI/API? Where is secret material stored/processed? Is outbound webhook delivery in scope? **Evidence status: UNVERIFIED.** |
| Money-out beneficiaries and destination changes (conditional) | Beneficiary/destination reference, minimum necessary destination attributes, verification result, change history, actor and approval references | Execute an approved money-out workflow only if selected and authorized | Minimize destination data; restrict viewing/changing; step-up/approval controls; notify and audit changes; prefer provider token/reference where supported | Is money-out in scope? What exact destination fields does provider require? What role/retention applies? **Evidence status: UNVERIFIED.** |
| Provider attempts, enquiries and callback receipts | Attempt/reference IDs, provider event ID, correlation ID, event/enquiry timestamps, verification result, normalized status, retry/replay metadata; raw payload only if justified | Correlate and assess external operation evidence | Distinguish operation from attempt and observation; verify callback authenticity; minimize raw payloads; record provenance and conflicting evidence | Which provider schemas and fields actually exist? Which raw fields must be retained? **Evidence status: UNVERIFIED.** |
| Asynchronous jobs and recovery metadata (conditional) | Job/event ID, tenant/operation reference, correlation ID, execution state, attempt count, timestamps, error class, dead-letter/replay/restore markers; embedded payload only if necessary | Process durable work and recover safely, if queues/workers are selected | Avoid secrets/unneeded personal data in jobs/errors; tenant-bound authorization; idempotent processing; controlled replay and restore; approved log/queue expiry | Which queue/outbox/inbox/dead-letter systems will exist? What fields and locations do they contain? **Evidence status: UNVERIFIED.** |
| Merchant verification and capability decisions (conditional) | Verification case/reference, evidence reference, reviewer/decision metadata, reason, source, scope, effective/expiry/review dates, appeal/correction history | Assess merchant eligibility only to the extent required by confirmed policy/provider/legal obligations | Separate raw evidence from decision metadata; limit access; document sources and decisions; do not assume identity-document collection | Which evidence is mandatory and who may view it? What is the decision authority and retention source? **Evidence status: UNVERIFIED.** |
| Reports, exports and derived artifacts (conditional) | Report request/filters, requester, tenant scope, artifact reference, generation time, storage location, download/access events, expiry and deletion status | Provide authorized reporting/export capability if implemented | Tenant-scope every stage; minimize report fields; time-limited access; protect temporary files/links; log access and verify cleanup | Which reports and file formats exist? Where are artifacts stored and when are they deleted? **Evidence status: UNVERIFIED.** |
| Security/privacy incidents and privileged-access approvals | Incident/case reference, affected-resource references, incident timeline, actor, access grant/scope/expiry, approval, notification and remediation records | Investigate incidents and govern exceptional access | Restrict incident detail; separate routine logs from incident records; least privilege; audit access and approvals; legal hold where applicable | What incident and privileged-access workflows exist? Which notifications, recipients and holds apply? **Evidence status: UNVERIFIED.** |
| Financial corrections and reconciliation evidence (conditional) | Import/batch identity, source reference/hash, discrepancy, linked operation/posting, correction/compensating reference, initiator/approver, reason | Reconcile financial records and evidence authorized corrections | Preserve provenance and correction history; prevent duplicate imports/postings; restrict exports; do not let support cases directly mutate financial state | Which correction/refund/reversal/settlement flows are in scope? What are the authoritative accounting and retention requirements? **Evidence status: UNVERIFIED.** |
| API abuse-control and security telemetry (conditional) | Event type, key/tenant reference, timestamp, outcome, rate/quota counters, correlation ID, limited network/device metadata only where justified | Detect misuse, protect availability and manage quotas if required | Define purpose and minimal fields; never log authorization secrets or unrestricted request/response bodies; restrict access and retention | What telemetry is actually collected? Do vendor systems receive it or process it cross-border? **Evidence status: UNVERIFIED.** |
| Privacy-rights requests and privacy complaints | Request/complaint reference, requester and identity-verification status, rights requested, received/decision timestamps, routing, outcome/reasons, recipient-notification evidence | Handle data-subject requests and privacy complaints separately from payment disputes | Dedicated workflow; controlled access; verify identity proportionately; track source-based deadlines; document decisions and downstream actions | Which process and deadlines apply to the actual entity? What current forms/templates are required? **Evidence status: UNVERIFIED.** |
| Secondary storage, copies and downstream systems | System/copy reference for primary DB, replicas, cache/index, object storage, queues, logs/traces, support/analytics vendor, backup/archive and disaster recovery, only where present | Understand all locations and copies to support access control, retention, deletion and recovery | Map each actual copy and region; verify vendor/subprocessor terms; control access; document expiry, deletion propagation and restore behavior | Which systems and countries actually exist? Which vendors receive data? **Evidence status: UNVERIFIED.** |
| Inventory governance and row-level accountability | Data-category ID, source-document reference, data owner, system of record, data-subject group, purpose, recipient, role, location, retention class, deletion/hold rule, evidence reference, reviewer and status | Make the inventory traceable, reviewable and maintainable | Use IN SCOPE / OUT OF SCOPE (with rationale) / UNVERIFIED; require source evidence and named reviewer; no global default retention period | Who owns each row and what evidence supports each decision? **Evidence status: UNVERIFIED.** |

## 3. Data-subject transparency and rights

The DPPA source text identifies several controls that should be reviewed against the real fields and screens:

1. **Specific purpose (s.12):** document a lawful, specific, explicitly defined purpose for each collection/processing activity.
2. **Pre-collection information (s.13):** assess how the person is informed about data categories, responsible party/contact, purpose, mandatory versus discretionary fields, consequences of non-provision, legal authority where relevant, recipients, access/rectification rights and retention period. Do not assume an exception applies without counsel review.
3. **Minimality and accuracy (ss.14–15):** justify each field; avoid excess data; maintain a way to correct inaccurate or outdated data.
4. **Correction/deletion requests (s.16 and Regulations reg.29):** define intake, identity verification, routing, decision, fulfillment/refusal rationale and downstream-recipient notification where required. Regulation 29(2) states a written decision within seven days after receipt. Treat this as source-stated wording pending qualified interpretation, not as an implemented or approved SLA.
5. **Retention and destruction (s.18):** do not choose one global retention duration. Map each data class to its purpose, legal/contractual source, trigger, period, exceptions, hold, deletion/de-identification and evidence.
6. **Cross-border processing (s.19 and Regulations reg.30):** determine where data is processed/stored and verify the applicable adequacy/consent and onward-transfer requirements with counsel.
7. **Security-breach handling (s.23 and Regulations reg.33):** the Regulations state that the required notification is to be made immediately after a breach and specify Form 7 information. Confirm exact roles, recipients and process; incident containment should not wait for paperwork.
8. **Privacy complaints (Regulations regs.40–41):** maintain a privacy complaint-handling system distinct from payment disputes and NPS consumer complaint reporting.
9. **Registration lifecycle (Act s.29; Regulations regs.15–27):** confirm actual role, registration certificate/current validity, renewal, updates to registered particulars and any applicable exception.
10. **Annual PDPO report (Regulation 50):** PDPO guidance describes a report within 90 days after the Government financial year (1 July–30 June), including complaints/status and breaches/actions. Confirm the current template, period and due date applicable to the actual entity before scheduling or submitting anything.

## 4. Data security and payment-data boundary

- Never place API secrets, access tokens, passwords, private keys or authentication material in case notes, analytics, general logs or support exports.
- Do not collect or retain full card details or sensitive authentication data unless a separately reviewed and authorized architecture establishes necessity and compliance. PCI DSS prohibits storage of sensitive authentication data after authorization even if encrypted; confirm the current standard and precise scope with a qualified PCI assessor/acquirer. Prefer a hosted/tokenized provider flow that keeps card data out of HARAI's systems if the eventual provider supports it, but do not assume that capability is available.
- Store evidence references rather than unrestricted copies of raw provider payloads wherever sufficient. Any raw payload retained must have a documented purpose, minimized fields, access policy, retention class and deletion/hold rule.
- Enforce tenant scope on every read/search/export/attachment download and on support tools; a client-supplied tenant identifier is not sufficient authorization.
- Audit access to restricted records and exports. Preserve correction history without silently overwriting records.
- Default to no private evidence in offline/PWA caches unless a separately approved, data-specific risk review explicitly permits it.
- Distinguish data integrity/provenance from truth: a hash can show that content has not changed, but does not prove the source was authentic or its claim correct.

## 5. Retention schedule worksheet — deliberately unfilled

No durations are approved. For every data class, complete the following before implementation:

| Required field | Required answer |
|---|---|
| Data class and examples | Exact fields/documents/logs covered |
| Purpose and legal basis | Specific purpose and counsel-reviewed basis |
| Controller/processor/collector roles | Role per entity and processing activity |
| Source and recipient | Collection source, disclosures and subprocessors |
| Storage/processing locations | Primary, replica, backup, logs, support and disaster-recovery locations |
| Retention trigger | Event that starts the clock |
| Required period | Exact period and authoritative law/contract/provider source, if any |
| Exceptions | Legal hold, open dispute, fraud/security investigation or other documented exception |
| Deletion/de-identification | Action, responsible owner, backup/log behavior and verification evidence |
| Evidence status | UNVERIFIED until source evidence is attached and reviewed; otherwise IN SCOPE or OUT OF SCOPE with rationale |
| Approval | Named privacy/legal reviewer, decision date and version |

Do not infer a universal period from an example, a vendor default, or the convenience of implementation. Do not delete evidence needed for a valid legal hold or required financial audit without qualified review.

## 6. Open evidence and acceptance criteria

The following must be supplied before this inventory can be marked verified:

- Actual screens/forms and API/event schemas showing every personal-data field collected, inferred, generated, transmitted or displayed.
- Data-flow diagram identifying each entity, purpose, recipient, trust boundary, storage system, subprocessors and country/region.
- Provider contract and API documentation confirming payload fields, retention expectations, data roles and permitted disclosures.
- Current PDPO registration certificate/status and details of any renewal or changes, if applicable.
- Privacy notice and data-subject rights process reviewed by qualified Ugandan privacy counsel/DPO.
- Security architecture/control evidence for access, encryption, logging, backup, incident response and support access.
- Per-category retention/hold/deletion schedule tied to applicable legal, accounting, contractual and dispute requirements.
- Qualified assessment of whether a DPIA or other formal assessment is required for the actual activities.
- Independent review evidence with reviewer identity/qualification, date, findings, conditions and disposition.

Each item stays **UNVERIFIED** until evidence is attached and the accountable reviewer accepts it. A blank template or proposed control is not proof of implementation.

## 7. Inventory completion crosswalk and evidence states

For every row above, record these fields in the row itself or in a linked controlled worksheet before it is treated as complete: (1) scope disposition and reason; (2) exact fields and source; (3) data-subject group; (4) purpose and counsel-reviewed legal basis; (5) controller/processor/collector roles per activity; (6) system of record and storage/processing copies; (7) recipients/subprocessors and jurisdiction; (8) access roles and security controls; (9) retention trigger, authoritative source, period and exceptions; (10) deletion/de-identification, backup propagation and legal-hold behavior; (11) named owner; (12) evidence link and qualified reviewer decision/date. Unknowns remain UNVERIFIED. A category is not approved for collection simply because it appears in this worksheet.

## 8. Cross-document validation pointer

DOC-70 compares this preliminary inventory with DOC-41, DOC-42, DOC-43, DOC-49, DOC-56, DOC-57, DOC-58, DOC-59 and DOC-64. It records candidate gaps and evidence needed before any row is treated as in scope. DOC-70 does not authorize adding fields or collecting data; keep unknown processing, locations, recipients and retention marked UNVERIFIED.

## 9. Current disposition

- This document is a candidate inventory and evidence checklist only.
- No actual production data flows or deployment locations were independently inspected.
- No retention durations, legal bases, privacy notices, registration status, cross-border decisions or regulatory deadlines are approved here.
- DOC-64 remains a draft proposal; DOC-65 remains preliminary; AR-022 remains open; FIN-004/SUP-005 remain unverified; G1 remains **NOT PASSED**; G2 remains **NOT APPROVED**.
- No implementation, test execution, provider/regulator outreach, credentials, live financial activity, filing/submission, merge or deployment is authorized by this document.


## 10. Intended V1 scope disposition cross-reference — 2026-10-24

The accountable product owner's previously recorded launch direction is now translated into category-level intended V1 scope in **DOC-71 §2A**. Use that section as the current scope baseline: account/authentication, minimized merchant onboarding, collections, provider-attempt/callback evidence, developer API/webhook configuration, minimum async recovery metadata, limited transaction/reporting views, security/abuse telemetry, complaint/dispute and privacy-rights handling, and inventory governance are intended V1 areas. Financial integrity/reconciliation is required but depends on the selected funds-flow and approved accounting design. Money-out is intended V1; refunds/reversals are included where provider-supported, but execution remains blocked until provider permission, funding/settlement authority, the funds-flow model and qualified reviews are documented. Bulk exports and secondary systems are not assumed beyond evidenced need.

This scope disposition resolves intended product direction only. It does not verify actual processing, approve specific fields, establish lawful basis or retention, prove provider capability, or authorize implementation or collection. EXT-001–EXT-003 remain deferred/conditional future scope. G1 remains **NOT PASSED**; G2 remains **NOT APPROVED**.
