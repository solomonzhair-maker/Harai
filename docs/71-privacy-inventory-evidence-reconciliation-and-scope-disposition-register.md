# DOC-71 — Privacy Inventory Evidence Reconciliation and Scope Disposition Register

**Status:** Internal documentation reconciliation — first pass complete; actual processing and scope remain unverified  
**Review date:** 2026-10-24  
**Canonical branch:** `docs/julypay-discovery-and-requirements`  
**Accountable internal reviewer:** Product owner  
**Parent documents:** DOC-69 (candidate inventory), DOC-70 (cross-document gap register)  
**Compared design proposals:** DOC-41, DOC-42, DOC-49, DOC-56, DOC-57, DOC-58, DOC-59, DOC-64  
**Purpose:** Reconcile the candidate privacy categories against the conceptual workflows and fields actually described in the repository, while distinguishing documented proposals from evidence of real product scope or implemented processing.

> **Critical limitation:** This is a repository-document review, not source-code, deployed-system, provider-payload, hosting-account, contract, or production-log inspection. The compared documents are proposals. A field or workflow mentioned below is a proposed design concept only; it is not evidence that HARAI collects or processes that data. No category is approved for collection by this register.

## 1. Disposition vocabulary

- **DOCUMENTED PROPOSAL:** A named workflow or data concept appears in one or more reviewed design documents.
- **CANDIDATE / SCOPE UNVERIFIED:** The category is plausible and should remain visible for assessment, but there is not enough evidence to decide it is in launch scope.
- **CONDITIONAL / MODEL-DEPENDENT:** The category depends on an unresolved operation family, funds-flow model, provider entitlement, or future architecture decision.
- **NOT EVIDENCED IN REVIEWED DOCS:** The reviewed material does not establish the specific system, field, processor, location, or production processing. This is not proof that it does not exist.
- **OUT OF SCOPE:** Use only after the accountable product owner records a reasoned scope decision. No row is marked OUT OF SCOPE by this review.

The next status must be assigned per concrete workflow and field set, not just per broad category. Unknowns stay **UNVERIFIED**.

## 2. First-pass category-to-source reconciliation

| DOC-69 candidate category | Repository evidence located in proposals | Candidate data concepts to verify (not a production schema) | First-pass disposition | Evidence required to resolve |
|---|---|---|---|---|
| Account and authentication | DOC-49 §§3, 7; DOC-56 ownership/identity domains; DOC-59 credential and dashboard access controls | Merchant/project association; credential identifier, scope, environment and status; actor/session and access events. Never inventory secret values. | DOCUMENTED PROPOSAL; actual fields and identity provider UNVERIFIED | Actual UI/API schemas, authentication architecture, credential store/rotation and audit configuration |
| Merchant onboarding and verification | DOC-50 (onboarding/activation); DOC-56 merchant domain; DOC-64 roles and evidence references where relevant | Application fields, evidence references, eligibility decision, reason, reviewer, capability scope, expiry/review history only if selected | CANDIDATE / SCOPE UNVERIFIED; exact KYC/evidence types not established | Approved eligibility policy, actual forms, provider/legal requirements, evidence storage and qualified retention/access review |
| Collection/payment intent | DOC-49 §4.1 and proposed create/retrieve API; DOC-41 state model; DOC-56 payment/operation concepts | Proposed intent ID, merchant reference, amount/currency, status, description, optional metadata, timestamps, expiry, request/correlation ID | DOCUMENTED PROPOSAL; final schema, enabled methods and production collection UNVERIFIED | Approved requirements/API schema, actual UI fields, provider contract and authorized field mapping |
| Provider attempts, status enquiries and callback receipts | DOC-41 §§2, 5, 5B; DOC-42 §2; DOC-57 callback/worker flow; DOC-58 §§5–6 | Distinct operation/attempt/receipt IDs; provider reference; event identity; timestamps; correlation; authentication/verification result; normalized state; retry/replay markers | DOCUMENTED PROPOSAL; provider-specific payload and retained raw fields UNVERIFIED | Provider/product contracts, authorized sample schemas, adapter mapping and explicit raw-versus-normalized retention decision |
| Ledger, settlement, reconciliation and financial corrections | DOC-42 §§2, 5–7; DOC-41 operation families; DOC-56 financial domains | Posting/entry IDs, amount/currency, source event, source-file identity/hash, match result, discrepancy, correction/compensation link, actor/approval references | CONDITIONAL / MODEL-DEPENDENT; funds-flow model unselected and finance model unapproved | Selected funds-flow model, approved accounting design, real source/report formats, finance review and correction authority |
| Conditional refunds, reversals and money-out / beneficiary data | DOC-41 §3; DOC-49 §5; DOC-56 money-out domain; DOC-58 operation matrix; DOC-47 AR-007/AR-008 | Separate operation ID; original-operation link; beneficiary/destination reference; safe change summary; verification status; funding/limit/approval references; provider outcome | CONDITIONAL / MODEL-DEPENDENT; enabled capabilities and destination fields unverified | Written provider permission/product entitlement, selected funds-flow model, actual provider schema, funding authority and specialist review |
| Developer projects, API credentials and webhook configuration | DOC-49 §§2–3, 5, 7; DOC-59 §§2–3, 9 | Project/environment; key ID/prefix, scope, status and lifecycle events; endpoint URL/configuration; secret rotation/verification metadata. Secret material excluded. | DOCUMENTED PROPOSAL; exact credential model and endpoint fields unapproved | Approved credential and webhook contracts; actual UI/API; secret-management and endpoint-validation design |
| Asynchronous jobs, outbox/inbox and recovery metadata | DOC-57; DOC-58 §5; DOC-41 §§5A–5B; DOC-42 §7.1 | Work/task ID and type/version; linked operation/attempt/event; tenant/project context; created/claimed/attempt timestamps; bounded error class; replay/restore marker | DOCUMENTED PROPOSAL; actual queue, storage and payload contents NOT EVIDENCED | Approved architecture and recovery workflow, actual worker/task schemas, logging redaction and restore/replay evidence |
| Reports, exports, temporary artifacts and derived datasets | DOC-58 operation matrix; DOC-59 §§5, 10; DOC-64 §§6, 8, 10; DOC-56 reporting/read-model domains | Requester, tenant/filter scope, artifact reference, generation time, download/access events, expiry/deletion state, derived-data lineage | DOCUMENTED PROPOSAL / CANDIDATE; exact export features, stores and copies unverified | Screen/API/export contracts, storage/vendor map, download authorization, expiry/cleanup and cross-tenant review |
| Security/privacy incidents and privileged-access approvals | DOC-59 §§10–11; DOC-43 threat model; DOC-64 §§5, 8 | Incident/case ID, affected-resource references, action/decision history, actor/approver, access grant/revocation, notification and evidence references | DOCUMENTED PROPOSAL; actual incident system and fields NOT EVIDENCED | Approved incident and privileged-access procedures, real tools/configuration, restricted-access and retention/hold review |
| API abuse-control and security telemetry | DOC-49 §§3, 7; DOC-58 error taxonomy; DOC-59 §8 and §10 | Minimal event type, key/tenant reference, timestamp, outcome, bounded network metadata if justified, quota/counter. No full secrets or unfiltered bodies by default. | CANDIDATE / SCOPE UNVERIFIED; actual telemetry and necessity unverified | Threat-informed logging design, actual configuration/vendor list, field minimization, access and retention decision |
| Complaints, disputes, communications, privacy-rights requests | DOC-64 §§3, 5–8, 11; DOC-69 initial communication/privacy-rights categories | Case/request ID, verified requester reference, category, received date, owner, decision/reason, communication metadata, evidence reference, downstream-recipient notification where applicable | DOCUMENTED PROPOSAL for transaction case management; privacy-rights workflow and actual channels UNVERIFIED | Qualified privacy/support review, actual intake channels, identity verification, approved routing/deadline sources and communication templates |
| Secondary storage, copies and downstream systems | DOC-57 data-flow boundaries; DOC-59 §5; DOC-64 §§6, 8; DOC-69 backup/recovery requirements | System/copy type; source category; vendor/recipient; region/jurisdiction; access path; retention/expiry; deletion/restore propagation | NOT EVIDENCED at system/configuration level; candidate copy types only | Actual deployment diagram, hosting/vendor agreements, configuration, subprocessor register, backup/restore and deletion evidence |
| Inventory governance and row-level accountability | DOC-69 §7; DOC-70 PIV-12; DOC-68 reviewer question pack | Row owner, source/version, scope decision/reason, field list, purpose/legal-basis review, data roles, system of record, recipient/location, retention/hold/deletion, reviewer/date | REQUIRED CONTROL TEMPLATE; named owners and approved row decisions UNVERIFIED | Completed row-level crosswalk, named accountable owners, source evidence and qualified privacy/legal/finance/security dispositions |


## 2A. Accountable intended-V1 scope disposition — 2026-10-24

**Decision basis:** The product owner has previously accepted a comprehensive Uganda-first gateway launch direction; collections and money-out are intended V1 capabilities, with refunds/reversals where provider-supported. The approved requirements batch is 51 Accept / 3 Defer (EXT-001–EXT-003), with all 32 P0 requirements retained. Option 3 remains the planning direction: compare provider-managed funds and direct integrations before selecting a funds-flow model. This section translates those existing decisions into intended privacy-inventory scope; it does not create new product features or claim production processing exists.

| Candidate category | Intended V1 scope disposition | V1 boundary / conditions |
|---|---|---|
| Account and authentication | **IN SCOPE — EVIDENCE PENDING** | Merchant/user account, role/tenant association, authentication and recovery/security metadata needed for the approved gateway workflows. Exact fields and identity assurance remain unverified. |
| Merchant onboarding and verification | **IN SCOPE — EVIDENCE PENDING, MINIMIZED** | Merchant application, contact/authority and capability decision metadata needed to establish merchant access. Do not presume identity-document collection, screening sources or a particular KYC regime; those remain UNVERIFIED pending provider/legal requirements. |
| Collection/payment intent | **IN SCOPE — EVIDENCE PENDING** | Collections are core intended V1 scope. Keep only necessary intent, merchant reference, amount/currency, method/status, provider reference, timestamps and operational correlation fields after API/provider mapping is approved. |
| Provider attempts, enquiries and callback receipts | **IN SCOPE — EVIDENCE PENDING** | Operational evidence required to establish transaction state and handle retries, callbacks and conflicting/late events. Exact provider payloads and any raw-body retention remain UNVERIFIED; prefer verified/normalized minimum fields and do not retain whole payloads by default. |
| Ledger, settlement, reconciliation and financial corrections | **IN SCOPE — EVIDENCE PENDING; MODEL-DEPENDENT DESIGN** | Financial integrity, reconciliation and auditable correction handling are required for a credible gateway. The ledger/accounting model, settlement responsibilities, record sources and retention require the selected funds-flow model and qualified finance/accounting review. |
| Refunds, reversals, money-out and beneficiary/destination data | **INTENDED V1 CAPABILITY — CONDITIONAL / BLOCKED UNTIL AUTHORIZED** | Money-out is intended V1 scope; refunds/reversals are included where supported. Do not enable or execute any operation until the chosen model, provider product permission, funding/settlement authority, actual field schema and qualified legal/finance/security reviews are documented. Until then, destination fields and actual processing remain UNVERIFIED. |
| Developer projects, API credentials and webhook configuration | **IN SCOPE — EVIDENCE PENDING** | API integration, key lifecycle metadata and merchant webhook configuration are core gateway functions. Never put secret values in the inventory or ordinary logs; exact mechanisms and endpoint controls require approval. |
| Asynchronous jobs, outbox/inbox and recovery metadata | **IN SCOPE — EVIDENCE PENDING, ARCHITECTURE-DEPENDENT** | Only the minimum operational metadata needed for reliable processing/recovery. The actual queue, persistence, payload content, replay authority and deployed implementation are not established. |
| Reports, exports and derived datasets | **IN SCOPE — LIMITED READ/REPORTING; BULK EXPORT UNVERIFIED** | Merchant transaction/status views and operational/reconciliation reports needed for V1 are intended. Do not assume unrestricted bulk exports, long-lived downloadable files or analytics datasets; each requires a specific scope decision and expiry/access design. |
| Security/privacy incidents and privileged-access approvals | **IN SCOPE — EVIDENCE PENDING** | Security event, incident, access-grant and approval records needed to protect the service and investigate misuse. Actual incident tools, fields, owners and retention remain unverified. |
| API abuse-control and security telemetry | **IN SCOPE — MINIMUM NECESSARY ONLY** | Authentication outcomes, bounded rate-limit/abuse events and security audit data may be needed. Network/device metadata and request bodies are not automatically in scope; justify each field and configure redaction/access/retention. |
| Transaction complaints, disputes, support communications and privacy-rights requests | **IN SCOPE — EVIDENCE PENDING** | Transaction complaint/dispute lifecycle is an intended operational requirement (FIN-004/SUP-005), but DOC-64 remains a draft and AR-022 remains open. Privacy-rights intake/response records must also be assessed; actual channels, owners and legally applicable clocks require qualified review. |
| Secondary storage, copies, vendors and downstream systems | **REQUIRED CONTROL / EVIDENCE PENDING; NOT A FEATURE SCOPE DECISION** | Map only systems/copies that actual architecture evidence establishes, including logs, caches, exports, backups, support tools and subprocessors. Do not invent vendors, regions or data transfers. Data deletion/restore propagation must be assessed before approval. |
| Inventory governance and row-level accountability | **REQUIRED CONTROL — IN SCOPE** | Maintain source links, row owner, scope rationale, exact fields, purpose/legal-basis review, roles, recipients/locations, access, retention/holds/deletion, reviewer and evidence status. Named operational owners and qualified approvals remain outstanding. |

### How to interpret these dispositions

- **“IN SCOPE — EVIDENCE PENDING”** means the workflow/category belongs in the intended V1 product or its necessary control design. It does not prove that the data exists in a system, approve any field for collection, establish legal basis, approve retention, or authorize implementation.
- **“INTENDED V1 CAPABILITY — CONDITIONAL / BLOCKED UNTIL AUTHORIZED”** preserves the owner's scope direction for money-out and supported refunds/reversals while keeping actual operations disabled until provider, funds-flow, funding, legal, finance and security gates are met.
- **“LIMITED READ/REPORTING”** does not approve arbitrary exports, extra analytics, marketing profiles or secondary uses.
- **Not included in core V1:** EXT-001 additional methods/currencies beyond the accepted planned MTN MoMo, Airtel Money and cards scope; EXT-002 scheduling, split settlement and marketplace payouts; and EXT-003 regional expansion. These remain deferred/conditional future scope. This does not establish that any named payment method is contractually or technically available.
- No category is marked **OUT OF SCOPE — RATIONALE RECORDED** in this section. Features not expressly included above remain UNVERIFIED rather than silently excluded.
- This is a product-scope disposition by the accountable owner as previously recorded, not qualified legal advice, provider approval, finance/accounting sign-off, a production inventory or implementation authorization.

## 3. Findings from the reconciliation

1. **Conceptual coverage is stronger than evidence coverage.** The proposals distinguish payment intents, provider attempts, callback receipts, ledger postings, reconciliation records, cases, credentials and asynchronous work. These distinctions should be retained in the inventory, but none establishes that the corresponding production system exists.
2. **The merchant API example is not a field inventory for the entire product.** DOC-49 proposes payment-intent fields and illustrative metadata, while explicitly leaving the final schema and checkout behavior open. Do not copy every example field into an assumed production inventory.
3. **Funds-flow-dependent categories cannot be finalized yet.** Ledger/accounting records and money-out/beneficiary information depend on the selected funds-flow model, provider permissions and approved finance design. Keep them conditional, not presumed active.
4. **Security and operational records need source-level verification.** DOC-57–DOC-59 propose queue/event, audit, abuse-control and derived-data safeguards but do not identify deployed vendors, physical storage, regions, configured telemetry fields or actual retention.
5. **Case-management design is not proof of a privacy-rights process.** DOC-64 is a draft for transaction complaints/disputes. Data access/correction/deletion requests and privacy complaints need their own verified workflow and qualified review.
6. **No absence claims are justified.** If this documentation review did not find a system, provider payload, location or field, record NOT EVIDENCED / UNVERIFIED rather than “does not exist.”
7. **No retention period can be inferred from proposed retries, event lifecycles, example timestamps, regulatory research or vendor defaults.** Retention must be assigned to each actual record and copy after qualified review.

## 4. Scope-resolution worksheet and evidence packet

For each category, complete the following before marking it in scope or out of scope:

1. **Workflow proof:** Link the approved requirement and actual screen/API/provider operation that causes the processing. Label each source as approved requirement, proposal, implemented source, provider documentation, contract, or runtime evidence.
2. **Field-level map:** Exact field name, meaning, origin, whether user-provided/observed/inferred/generated, sensitivity, optionality, and whether raw data can be discarded after normalization.
3. **Data subjects and roles:** Identify whose data is involved and document controller/processor/collector roles per activity only after qualified review; do not infer these roles from product terminology.
4. **Purpose and necessity:** State the specific purpose, minimum necessary fields and alternatives considered; obtain qualified review of legal basis and transparency obligations.
5. **System and copies:** Identify system of record, service, datastore, queue, cache/index, object/report storage, logs, support tooling, vendor and backups only where evidenced. Include country/region and recipient.
6. **Access and disclosure:** Name accountable owner and role-based access; identify merchant/provider/staff/vendor recipients and any onward disclosure.
7. **Retention and deletion:** Define start trigger, approved period/source, legal hold, correction, primary deletion, replicas/caches/exports/queues/backups, restore behavior and proof of deletion. Keep durations UNVERIFIED until reviewed.
8. **Review and decision:** Record named owner, qualified reviewer/qualification, decision date, conditions, evidence links and status. Out-of-scope decisions require a reason and revisit trigger.

### Required status values

- **IN SCOPE — EVIDENCE PENDING:** Product owner has explicitly confirmed the workflow is in intended scope, but field-level evidence or qualified review remains outstanding. This is not permission to collect or implement.
- **OUT OF SCOPE — RATIONALE RECORDED:** Product owner has excluded the workflow with a reason, dependencies and trigger for reconsideration.
- **UNVERIFIED:** Scope or evidence is not sufficient to decide.
- **EVIDENCE REVIEWED — PENDING QUALIFIED APPROVAL:** Supporting artifacts exist but specialist/accountable acceptance is outstanding.
- **VERIFIED FOR DOCUMENTATION PURPOSE:** All required evidence and reviews are linked; this status does not itself authorize implementation or collection.

The original evidence-only table in §2 remains a record of what the source documents establish. Intended product scope is now resolved separately in §2A; actual processing, fields and controls remain unverified.

## 5. Ordered next actions

1. Use §2A as the current intended-V1 category disposition. The remaining work is evidence-level resolution: map actual fields, systems, recipients, locations, retention and owners, and record reasoned exclusions for any sub-workflow not included. Do not use the broad “comprehensive launch” ambition as evidence that every operation is enabled.
2. Reconcile each in-scope workflow against actual repository source/API schemas and UI. Where only a proposal exists, list the exact source artifact still missing; do not describe it as implemented.
3. Obtain provider-specific field/payload documentation only through already authorized channels; no provider outreach is authorized by this register.
4. Map actual hosting, subprocessors, regions, queues, logs, exports and backups from deployment/configuration evidence when such inspection is separately authorized.
5. Obtain qualified privacy/legal and finance/accounting review for purposes, roles, retention, rights, complaint/reporting obligations and funds-flow-dependent records; obtain qualified security review for telemetry, secrets, privileged access, exports and derived copies.
6. Update DOC-69 row by row only when evidence is available. Update DOC-70 findings with the precise evidence and disposition; do not close a gap because a category has been named in documentation.

## 6. Governance and current gate status

- DOC-69 remains a preliminary candidate inventory; DOC-70 remains a documentation gap register.
- DOC-64 remains draft; DOC-65 remains preliminary; AR-022 remains open; FIN-004/SUP-005 remain unverified/open for specialist review.
- FIN-004/SUP-005, AR-022, provider capability, legal applicability and data-role questions are not resolved by this document.
- G1 remains **NOT PASSED**; G2 remains **NOT APPROVED**; the funds-flow model remains unselected.
- No implementation, tests, provider/regulator outreach, credential use, live financial activity, regulatory filing/submission, merge or deployment is authorized.
