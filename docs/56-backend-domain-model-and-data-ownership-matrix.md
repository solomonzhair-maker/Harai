# DOC-56 — Backend Domain Model and Data Ownership Matrix

**Status:** Draft domain-model proposal — not an approved schema or implementation specification  
**Branch:** docs/julypay-discovery-and-requirements  
**Related documents:** DOC-10, DOC-11, DOC-12, DOC-15, DOC-16, DOC-17, DOC-28, DOC-31, DOC-34, DOC-40, DOC-41, DOC-42, DOC-43, DOC-45, DOC-46, DOC-48, DOC-49, DOC-50, DOC-51, DOC-52, DOC-54, DOC-55  
**Purpose:** Identify the core business domains, conceptual records, ownership, relationships, tenant boundaries, integrity invariants and unresolved design decisions needed before selecting a concrete database schema.

> **Authority boundary:** This document is conceptual. It does not approve table names, ORM models, column names/types, primary-key formats, database technology, migrations, indexes, event-bus design, or service deployment topology. It does not authorize backend/payment-engine implementation, tests, provider contact, live credentials, financial activity, merge or deployment. G1 remains NOT PASSED and G2 remains NOT APPROVED.

## 1. Modeling principles

1. **Model business meaning before storage layout.** A conceptual domain entity is not automatically a database table or service.
2. **Separate tenant-owned data from platform-owned data.** Every merchant-facing read or mutation must enforce tenant isolation on the server.
3. **Separate intent, execution attempts, financial records and external settlement evidence.** One record must not be overloaded to represent all lifecycle layers.
4. **Keep money auditable.** Financial records must have exact amount/currency semantics, explicit references, immutable history where required and reviewed correction workflows.
5. **Prefer append-only evidence for consequential decisions.** Corrections should preserve the prior record and reason; never silently rewrite a financial or approval history.
6. **Treat provider data as untrusted external evidence.** Store only what is needed, with provenance, timestamps, verification status and sensitive-data controls.
7. **Use explicit lifecycle transitions.** The data model must support pending and ambiguous outcomes without guessing success/failure.
8. **Do not store secrets as ordinary domain attributes.** Credentials require a separately reviewed secret-storage and access design.
9. **Record provenance and accountability.** Important actions need actor, time, correlation, reason and approval context as appropriate.
10. **Avoid premature universal abstractions.** Similar labels do not mean payment operations, webhook deliveries, approvals, cases and reconciliation runs share one lifecycle.
11. **Do not infer provider, legal or accounting rules.** Provider-specific capability and funds-flow assumptions remain unconfirmed until supported by approved evidence.
12. **Use synthetic data for prototypes and tests.** Real merchant evidence, customer data, credentials and transaction details must not be placed in this repository.

## 2. Ownership vocabulary

| Label | Meaning |
|---|---|
| Tenant-owned | Belongs to one merchant/business tenant; every access path must enforce that boundary |
| Tenant-scoped association | Relates a tenant to a shared platform resource; access must validate both the association and resource scope |
| Platform-owned | Belongs to HARAI operations or shared platform governance, not to a merchant tenant |
| Financially sensitive | Can affect balances, fees, refunds, payouts, settlement, reconciliation or financial reporting |
| Security-sensitive | Contains or controls credentials, access, identity evidence, risk signals or privileged actions |
| External evidence | Data received from a provider, processor, bank or other external system; source and trust level must be retained |
| Derived/read model | Computed or denormalized data used for display/reporting; source, freshness and rebuild rules must be defined |
| Immutable/append-only candidate | Expected to preserve history; exact retention and correction rules need domain review |
| Unresolved | Requires explicit product, architecture, security, finance, legal or provider decision |

These labels describe proposed ownership, not a finalized physical data architecture.

## 3. Domain boundaries at a glance

| Domain | Primary responsibility | Typical records | Main owner / authority | Key boundary |
|---|---|---|---|---|
| Identity and access | Authenticate people and authorize actions | Principal reference, session metadata, role grants, permissions | Identity/security architecture | A browser role claim never grants access |
| Tenant and merchant | Business profile, membership and lifecycle | Tenant, merchant profile, membership, verification case | Merchant domain; review authority for decisions | All merchant data is scoped to the tenant |
| Developer platform | Projects, API key metadata, webhook configuration | Project, key metadata, endpoint configuration | Developer platform/security | Secret material is separated from ordinary metadata |
| Capability and eligibility | Record what a merchant may use and under what conditions | Capability assessment, evidence reference, approval decision | Platform policy plus provider/legal gates | Product intent does not equal live permission |
| Collection operations | Merchant request to collect money | Payment intent/collection operation, hosted link/session | Payments domain | Operation state is separate from provider attempts |
| Provider integration | Track external submissions and observations | Provider attempt, enquiry result, callback receipt | Provider adapter/operations | External response does not automatically prove finality |
| Refunds and reversals | Track a distinct return/correction request | Refund/reversal operation, original-operation association | Payments plus finance/provider rules | Separate lifecycle, eligibility and accounting effect |
| Money-out | Payout/withdrawal requests and outcomes | Payout operation, beneficiary reference, approval | Money-out plus finance/risk | Capability, funding, destination and approval gates |
| Financial records | Record economic effects and fees | Ledger account, journal/posting, fee assessment | Finance/accounting owner | Immutable balanced records and controlled corrections |
| Settlement and reconciliation | Compare external and internal evidence | Settlement batch/statement, reconciliation run, discrepancy | Finance/reconciliation | A successful operation does not prove settlement |
| Merchant event delivery | Deliver signed events to merchant systems | Event/outbox item, delivery attempt | Developer platform/operations | Delivery outcome is separate from business-operation outcome |
| Support and disputes | Track requests, complaints and investigations | Case, message, assignment, evidence reference | Support/compliance | Support access is limited, audited and tenant-safe |
| Audit and security | Preserve security and consequential-action evidence | Audit event, security alert, investigation reference | Security/operations | Audit evidence must not be editable by ordinary business flows |
| Reporting and export | Present authorized data for analysis | Report request, export job, generated artifact reference | Reporting/privacy | Exports are permissioned, bounded and time-limited |
| Operational governance | Manage approvals, incidents and releases | Approval request/decision, incident, release record | Platform governance/operations | Separation of duties and explicit authorization |

## 4. Conceptual entity catalogue

### 4.1 Identity, tenant and merchant lifecycle

| Conceptual entity | Ownership | Relationships | Integrity and access requirements | Decisions still open |
|---|---|---|---|---|
| Principal reference | Platform/identity-owned; identity provider may own canonical identity | Relates to memberships, sessions and audit actors | Store only necessary identity attributes; avoid copying provider secrets; disabled principals lose access according to approved policy | Identity provider, principal identifier and lifecycle contract |
| Session/authentication context | Security-sensitive; owned by chosen identity/session mechanism | Relates to principal and security events | Session expiry/revocation, CSRF/origin protection where applicable, no secrets in logs or URLs | Cookie/token transport, MFA/step-up, revocation and recovery |
| Tenant | Platform-managed record for a merchant business | Parent for merchant profiles, projects, memberships, operations and reports | Stable internal reference; lifecycle and data-isolation boundary; no cross-tenant reads | Whether tenant and merchant profile are one entity or separate |
| Merchant profile | Tenant-owned business details | Belongs to tenant; relates to verification, capabilities, settlement configuration and support cases | Minimize personal/business data; version important changes; permission fields by role | Required fields, jurisdiction-specific data and retention |
| Membership | Tenant-scoped association between principal and tenant | Links principal, tenant and role/permission grants | Enforce unique active membership rules as approved; prevent unauthorized self-escalation; audit role changes | Invitation flow, multiple roles, delegated administration and last-admin safeguards |
| Role/permission definition | Platform-owned policy metadata | Assigned through membership or internal staff assignment | Stable permission identifiers, least privilege, server-side enforcement; avoid embedding policy only in frontend | RBAC/ABAC combination, policy versioning and review cadence |
| Verification case | Tenant-scoped case with restricted evidence | Relates to tenant, evidence references, reviewers, decisions and capability eligibility | Explicit review state, actor/time/reason, versioned decisions; restricted access to evidence | KYC policy, professional/legal obligations, evidence retention and appeal path |
| Evidence reference | Security/privacy-sensitive; may reference controlled object storage | Relates to verification case, dispute, approval or audit event | Store metadata and secure reference rather than public URL; access logging, malware scanning where applicable, retention/deletion policy | Storage provider, encryption, size/type rules and deletion schedule |
| Capability assessment | Platform-owned decision scoped to tenant, method, environment and operation | Relates to verification, provider eligibility evidence, approval and current policy | Record decision source, scope, expiry/review date and approver; never treat a frontend flag as authority | Capability vocabulary, effective dates, provider-specific granularity and override authority |

### 4.2 Developer platform and credentials

| Conceptual entity | Ownership | Relationships | Integrity and access requirements | Decisions still open |
|---|---|---|---|---|
| Developer project | Tenant-owned | Belongs to tenant; relates to API credentials, webhook endpoints, operations and project settings | All access checked against tenant membership and project scope; project state and environment explicit | Project/environment model and supported configuration |
| API key metadata | Tenant/project-scoped; security-sensitive | Belongs to project and issuing actor; relates to scopes, usage, rotation and revocation audit | Store only a safe identifier and approved verifier/hash or secret-manager reference; never expose stored secret again; scope and expiry enforce server-side | Key format, verifier/storage design, one-time reveal and rotation overlap |
| API secret material | Security-sensitive secret-store responsibility, not ordinary domain data | Linked indirectly to key metadata through a non-secret reference | Encryption, least-privilege retrieval, rotation, revocation and access audit; never commit or log secret | Secret manager, encryption/key management and recovery policy |
| Webhook endpoint configuration | Tenant/project-owned | Relates to subscribed event types, signing-secret reference and delivery attempts | Validate destination, enforce HTTPS and network abuse controls as appropriate; restrict access to endpoint secrets | Allowed URL policy, DNS/IP protections, subscription schema and update rules |
| Webhook signing-secret reference | Security-sensitive secret-store responsibility | Associated with endpoint version/rotation state | Secret not readable from normal list/detail APIs; rotation and overlap explicit; access audited | Signing algorithm/version and secret rotation behavior |
| API usage/rate-limit policy | Platform-owned policy with tenant/project scope | Applies to API keys, projects and operations | Server-enforced quotas; prevent one tenant from exhausting shared resources; audit abuse exceptions | Numeric thresholds, burst policy, per-operation limits and fairness |

### 4.3 Collection and provider execution

| Conceptual entity | Ownership | Relationships | Integrity and access requirements | Decisions still open |
|---|---|---|---|---|
| Collection operation/payment intent | Tenant-owned and financially sensitive | Created by tenant/project; may have one or more provider attempts and related refunds, events and ledger postings | Unique stable reference; exact amount/currency; approved state machine; idempotency; canonical state transitions and actor/source evidence | Public API mapping, creation fields, expiration, allowed methods and state contract |
| Hosted payment link/session | Tenant-owned, associated with a collection operation or checkout session | Relates to collection operation, expiry/use policy and public checkout access | Unguessable reference; limited public fields; lifecycle/usage constraints and concurrency safety; no private tenant data in public URL | Link reuse, expiry, cancellation and checkout-session relationship |
| Provider account/configuration reference | Platform-owned and provider-specific; credentials held separately | Relates to provider adapter, capability evidence and merchant/provider relationship | Record environment and provider account/relationship identity without storing secret material; strict access control | Contracted aggregator model, per-merchant registration, credential ownership and provider accounts |
| Provider attempt | Platform-owned execution record associated with a tenant operation | Belongs to one operation and provider configuration; may have enquiry/callback evidence | Each submission attempt traceable; preserve request/response summaries safely; never treat transport acceptance as payment finality | Retry classes, provider idempotency, attempt numbering and retention |
| Provider enquiry/observation | External evidence attached to operation/attempt | Relates to provider reference, observed status, source and time | Preserve source/time and normalized mapping version; late/out-of-order observations cannot silently overwrite stronger evidence | Evidence precedence, polling cadence, status mapping and provider contracts |
| Provider callback receipt | Platform-owned external-ingress record | Relates to provider attempt/operation after validated correlation | Authenticate before trust; deduplicate/replay-protect; preserve receipt time and validation result; payload minimization | Provider signature/identity verification, retention and out-of-order handling |
| Operation idempotency record | Platform-owned safety record scoped to caller and operation type | Links caller/project, idempotency key fingerprint and canonical operation/result | Atomic uniqueness, defined retention, same-key/same-request semantics and conflict on incompatible reuse | Exact key scope, retention and response replay contract |
| Operation audit/correlation reference | Platform-owned cross-domain correlation | Links request, operation, provider attempts, webhook events and financial entries | Non-secret; uniqueness/propagation policy; never use as authorization by itself | Identifier format, trace propagation and retention |

### 4.4 Refunds, reversals and money-out

| Conceptual entity | Ownership | Relationships | Integrity and access requirements | Decisions still open |
|---|---|---|---|---|
| Refund operation | Tenant-owned and financially sensitive | References original eligible collection and provider attempt; relates to approvals, provider attempts, ledger and audit events | Enforce original-operation eligibility and cumulative refund bounds atomically; separate status and accounting impact | Partial/multiple refunds, fees, provider-specific finality and policy |
| Reversal/return operation | Tenant or platform-scoped according to source of reversal | References original collection/payout and provider evidence | Keep distinct from refund and payout cancellation; preserve reason, source and financial effect | Exact provider meanings, who may initiate and how to represent unsolicited reversals |
| Beneficiary/destination reference | Tenant-owned, highly sensitive | Used by payout operations; may have verification and change history | Mask by default; protect create/change/deactivate; validate ownership; changes may require step-up and approval | Supported destination types, verification, cooling-off and provider requirements |
| Money-out/payout operation | Tenant-owned and financially sensitive | References beneficiary, approvals, funding/limit checks, provider attempts, ledger and reconciliation | Explicit eligibility, funding, limits, idempotency, approval and recovery; ambiguous outcomes never trigger blind resubmission | Provider-supported payout flow, funding model, approval thresholds and finality |
| Approval request | Tenant- or platform-scoped according to action | References proposed action/resource, requester, policy and required approvers | Snapshot material request details; requester/approver separation; expiry and version check; immutable decision history | Approval thresholds, quorum, delegation, emergency exception and expiry |
| Approval decision | Append-only candidate | Belongs to request and approver; relates to audit | Actor/time/outcome/reason captured; duplicate/conflicting decisions handled explicitly | Retention, signature/step-up and appeal process |

### 4.5 Ledger, fees, settlement and reconciliation

| Conceptual entity | Ownership | Relationships | Integrity and access requirements | Decisions still open |
|---|---|---|---|---|
| Ledger account | Platform accounting domain; may be logically attributed to tenant/provider/fee/funding categories | Receives postings from financial events | Account purpose and normal balance defined by reviewed accounting model; no direct UI editing | Chart of accounts, account ownership and accounting policy |
| Journal/financial event | Financially sensitive, append-only candidate | Records the economic event and links to operation/provider/settlement evidence | Immutable after posting; source reference, currency, effective/recorded times and idempotent posting key | Accounting event taxonomy, posting timing and correction policy |
| Ledger posting/entry | Financially sensitive, append-only candidate | Belongs to journal/event and ledger account | Balanced entries per currency according to approved model; integer minor units or exact decimal; no floating point; no cross-currency netting | Schema, debit/credit semantics, constraints and transaction boundary |
| Fee assessment | Financially sensitive | Relates to operation, effective-dated commercial schedule and journal | Fee payer, calculation basis, currency, rounding, tax and schedule version retained; no hard-coded guessed pricing | Written provider costs, customer pricing and tax treatment |
| Commercial schedule | Platform-owned, effective-dated configuration | Applies to provider/method/merchant cohort as authorized | Versioned, approved, auditable changes; historic operations preserve applied schedule | Provider terms, pricing approval, taxes and effective dates |
| Settlement batch/statement | External evidence and finance-owned reconciliation data | Relates to provider, period, transactions, fees and bank/funds evidence | Preserve source, period, completeness and import provenance; reconcile totals independently | Provider reports, settlement route, file/API access and retention |
| Reconciliation run | Finance/operations-owned process record | Compares internal operations/ledger with provider/settlement evidence | Record run scope, source versions, start/end, result and operator; repeatable without duplicate postings | Run scheduling, source completeness and operational owner |
| Reconciliation discrepancy | Financially sensitive case | Links mismatched/missing records, evidence, assignment and resolution | Cannot silently delete or overwrite; resolution requires reason, evidence and authorized reviewer | Categories, SLA, assignment, escalation and closure authority |
| Financial correction/compensating event | Append-only candidate | References original posting/event and authorized correction decision | Preserve original and correction; enforce balanced effect and approval; never rewrite history to force a match | Permitted correction types, approval thresholds and accounting review |

**Accounting caveat:** This is a list of conceptual responsibilities, not a finished chart of accounts or a claim that HARAI will hold, custody or settle funds. The funds-flow model remains undecided and requires provider evidence plus qualified Uganda legal/compliance and finance review.

### 4.6 Merchant events, support, audit and operations

| Conceptual entity | Ownership | Relationships | Integrity and access requirements | Decisions still open |
|---|---|---|---|---|
| Domain event/outbox item | Platform-owned event record with tenant scope where applicable | References source operation and event version; feeds delivery processing | Commit event intent consistently with source change; deduplicate; preserve event type/version and safe payload policy | Outbox/queue topology, schema evolution and retention |
| Webhook delivery attempt | Tenant/project-scoped delivery history managed by platform | Relates to event and endpoint configuration version | Delivery attempt result does not change payment state; bounded retries, signed payload, replay protection and sanitized response data | Retry schedule, delivery retention and manual replay policy |
| Support case | Tenant-scoped; internal access controlled | Relates to merchant, operation, evidence, messages and assigned queue | Access reason/audit for privileged access; redact sensitive payloads; case transitions controlled | Case categories, SLA, dispute process and access expiry |
| Support message/note | Tenant-scoped or internal-only with explicit visibility | Belongs to case and author | Distinguish merchant-visible reply from internal note; immutable edit history or audited edits | Visibility model, retention and attachment rules |
| Audit event | Platform-owned append-only candidate | References actor, action, target, time, correlation and result | Tamper resistance, controlled access, redaction, retention and reliable capture of consequential actions | Storage, integrity protection, retention and query access |
| Security alert/investigation | Platform/security-owned, may reference tenant | Relates to detection, evidence, case and disposition | Restricted visibility; preserve evidence provenance; distinguish alert from confirmed incident | Detection taxonomy, access policy and response runbooks |
| Report/export request | Tenant-scoped or platform-scoped according to report | References query scope, requester, generated artifact and expiry | Permission checked at request and download; bounded scope; avoid leaking other tenants; audit access | Report definitions, async thresholds and retention |
| Export artifact reference | Sensitive derived data, stored separately from ordinary records | Belongs to export request | Private access, short-lived download access, expiry/deletion, no public object URL | Storage, encryption, expiry and export limits |
| Incident record | Platform operations-owned | Relates to affected services/releases, timeline, actions and post-incident review | Record impact, decisions, timestamps, owner and recovery evidence; no secret values in notes | Severity model, notification rules and review timeline |
| Release record | Platform operations-owned | Relates to version, approval, rollout, rollback and incident | Explicit approvers and evidence; deployment action separate from documentation status | Release authority, deployment pipeline and rollback thresholds |

## 5. Relationship and cardinality sketch

This is a conceptual relationship map only; cardinality, optionality and physical keys must be decided during schema design.

- One tenant has one or more memberships, projects, merchant operations and support/reporting scopes.
- A principal may belong to multiple tenants only if that product policy is approved; each membership carries its own permissions.
- A project belongs to a tenant and may have multiple API-key metadata records over time, with separate secret material and rotation history.
- A collection operation may have multiple provider attempts and multiple provider observations; it may emit multiple domain events and webhook delivery attempts.
- A refund or reversal references its original operation and may have its own provider attempts and accounting effects.
- A money-out operation references a destination, may require one or more approval decisions, and may have provider attempts, ledger events and reconciliation evidence.
- A financial journal may contain multiple postings; posting totals must balance according to the approved accounting model and currency.
- A reconciliation run compares scoped internal and external records and may create multiple discrepancies; resolving a discrepancy does not directly rewrite financial history.
- A domain event may be delivered to multiple subscribed endpoints; each endpoint delivery has its own attempt history.
- An audit event can reference many kinds of target through a typed target reference or equivalent design, but must not become a reason to bypass domain authorization.
- A verification case may collect multiple evidence references and decisions over time; a decision should identify the evidence/policy version it relied on.
- A capability assessment may depend on merchant verification, provider/method/environment eligibility and internal approval; the resulting effective capability must remain traceable to that evidence.

## 6. Tenant isolation and authorization invariants

1. Every tenant-owned record must have a server-enforceable tenant ownership path, direct or through an authorized parent.
2. Tenant ownership must not be inferred from a request body alone. The server validates the principal's current membership and permission for the target resource.
3. List queries, search, detail endpoints, updates, exports, background jobs, webhooks, support tools and operational recovery paths all require tenant-scope review.
4. Cross-tenant foreign-key relationships must not be accepted unless an explicitly approved platform-owned relationship permits them.
5. Resource references are not secrets and do not grant authorization.
6. Background jobs must carry verified tenant/resource context and re-check that the operation remains eligible when executing delayed work.
7. Internal staff access must use separate privileged permissions, purpose/assignment checks and audit evidence; support access is not universal data access.
8. Revoked memberships, keys, sessions or capabilities must take effect according to approved revocation and cache-invalidation rules.
9. Authorization must be checked again for sensitive transitions and exports, not just when the UI initially loads.
10. Negative tests must cover guessed IDs, altered tenant/project references, cross-tenant filters, exports, webhook replay and background-job context before any production release.

## 7. Financial integrity and lifecycle invariants

- **Operation vs attempt:** one business operation may have multiple external attempts; do not make an attempt row the canonical operation.
- **Pending vs unknown:** retain uncertainty until authoritative enquiry/reconciliation resolves it.
- **Success evidence:** transport-level acceptance, redirect, callback receipt or merchant webhook delivery alone does not prove final payment success.
- **Idempotency:** duplicate requests must resolve according to an approved server-side contract, not just disabled UI buttons.
- **Refund bounds:** concurrent refund requests must not exceed the original operation's eligible refundable amount under the approved accounting/provider policy.
- **Money-out safety:** beneficiary changes, funding checks, approval decisions and provider submissions must be separately traceable.
- **Immutable accounting:** posted entries are corrected through approved compensating events, not silent edits or deletes.
- **Exact monetary representation:** no binary floating-point accounting; currency precision and rounding rules require approval.
- **Currency isolation:** never net entries across currencies without an explicitly approved conversion and accounting model.
- **Settlement distinction:** payment operation success is not proof that a settlement batch has arrived or reconciled.
- **Webhook separation:** webhook delivery failure must not reverse or rewrite the payment operation state.
- **External evidence provenance:** retain provider/source reference, observation time, normalization version and confidence/validation status as appropriate.
- **Audit completeness:** security and financial changes need durable, searchable evidence with appropriate privacy minimization.
- **Recovery safety:** retries must be based on operation-specific idempotency and provider semantics; ambiguous money movement is not blindly resubmitted.

## 8. Data classification and retention planning

Exact retention periods are not selected. Each entity family needs an approved data inventory before implementation.

| Classification | Examples | Minimum planning controls |
|---|---|---|
| Public | Public integration docs, product descriptions | Versioning and accuracy; do not disclose unsupported capabilities |
| Internal | Release metadata, non-sensitive operational configuration | Role-based access and change audit |
| Tenant-confidential | Merchant profile, transactions, support cases, reports | Tenant isolation, least privilege, export controls and retention |
| Personal/special-category evidence | Identity documents, personal contact details or regulated evidence where applicable | Data minimization, restricted access, encryption, purpose limitation and qualified retention/deletion review |
| Financially sensitive | Amounts, payouts, refunds, ledger entries, settlement evidence | Exact representation, audit, integrity constraints and finance-approved retention |
| Secret material | API secrets, webhook signing secrets, provider credentials, session tokens | Dedicated secret handling, least privilege, rotation, no repository/log/browser persistence |
| Security-sensitive | Audit logs, abuse signals, investigation evidence | Tamper resistance, restricted access, provenance and incident-retention policy |

Retention/deletion must reconcile privacy obligations with financial recordkeeping, disputes, audit and provider contracts. Do not invent a universal retention duration.

## 9. Consistency and concurrency controls to decide

For each domain, schema and transaction design must specify:

- Unique constraints for stable public references, provider references in their proper provider/account scope, idempotency scope and other genuine uniqueness invariants.
- Foreign-key and tenant-consistency constraints so records cannot accidentally reference a resource from another tenant.
- Atomic checks for refund limits, payout limits/funding, approval version and duplicate operation creation.
- Isolation/locking or optimistic concurrency semantics for concurrent edits, approvals and money-affecting requests.
- Transaction boundaries for operation creation, idempotency reservation, outbox event creation and ledger posting where those changes must be atomic.
- Safe handling of duplicate, delayed, missing and out-of-order callbacks or status observations.
- Versioning of state mappings, fee schedules, capability decisions, API contracts and event schemas.
- Backfill and migration strategy, including auditability and rollback limitations for financial records.
- Indexing based on approved query patterns, data volume, retention and privacy—not speculative schema convenience.
- Rebuild/reconciliation rules for derived summaries and read models.
- Deletion/anonymization behavior where lawful, including how to preserve non-identifying financial integrity and audit requirements.

No particular SQL constraint, index, ORM decorator or isolation level is prescribed by this document.

## 10. Open architecture decisions

| Decision | Question requiring an explicit answer | Related documents / evidence |
|---|---|---|
| Identity ownership | Which identity provider and session architecture will own principals and sessions? | DOC-40, DOC-43, DOC-51, DOC-55 |
| Tenant/merchant boundary | Is merchant profile separate from tenant, and can a person belong to multiple tenants? | DOC-31, DOC-50, DOC-51 |
| Data-store architecture | One relational database, schema separation, row-level security, service-owned stores, or a reviewed combination? | DOC-16, DOC-17, DOC-40, DOC-43 |
| Physical schema | What identifiers, keys, columns, types, indexes, constraints and migration approach are appropriate? | DOC-16, DOC-40, DOC-41, DOC-42 |
| Service boundaries | Which responsibilities remain modular in one application and which, if any, need independent services? | DOC-17, DOC-40, DOC-48 |
| Funds-flow model | Who receives, holds, controls and settles funds; how are payout funding and returns represented? | DOC-28, DOC-30, DOC-37, DOC-38, DOC-42 |
| Provider account relationship | Are provider accounts platform-level, merchant-level, or a combination under written authorization? | DOC-29, DOC-37, DOC-38, DOC-48 |
| Ledger policy | Chart of accounts, posting timing, fee/tax treatment, correction events and finance review | DOC-11, DOC-42 |
| State finality | Provider-specific evidence precedence, unknown resolution, late callbacks and reversals | DOC-10, DOC-41, DOC-48 |
| Secret storage | Which secret manager, encryption/key ownership, rotation and emergency revocation process? | DOC-13, DOC-43, DOC-49 |
| Audit integrity | Storage, tamper resistance, retention, search and privileged access | DOC-15, DOC-43, DOC-45 |
| Evidence retention | Legal/provider/financial retention periods and deletion/anonymization rules | DOC-30, DOC-37, DOC-45 |
| Reconciliation sources | Which statements/APIs, settlement evidence and independent bank/funds evidence are available? | DOC-28, DOC-37, DOC-38, DOC-42 |
| Event/outbox design | Atomicity, queue ownership, event versioning, retries and poison-message handling | DOC-14, DOC-17, DOC-18, DOC-48 |
| Approval policy | Named accountable roles, thresholds, separation of duties, emergency path and expiry | DOC-31, DOC-33, DOC-46, DOC-51 |
| Data residency/privacy | Applicable jurisdiction, cross-border processing and professional review requirements | DOC-30, DOC-43 |
| Reporting architecture | Synchronous queries vs derived/read models and asynchronous export jobs | DOC-18, DOC-49, DOC-55 |
| Testing and evidence | Which proposed scenarios become actual tests, and how are results independently reviewed? | DOC-31, DOC-34, DOC-45 |

## 11. Proposed model-review checklist

Before approving a concrete schema:

- [ ] Every conceptual entity has an accountable domain owner.
- [ ] Ownership is explicit: tenant, platform, association, external evidence or derived view.
- [ ] Every tenant-owned entity has a validated tenant-scope path.
- [ ] Every relationship has defined cardinality, optionality, deletion behavior and integrity rules.
- [ ] Money, provider attempts, callbacks, refunds, payouts, ledger and settlement evidence are separate where semantics differ.
- [ ] Lifecycle state and finality rules match DOC-41 and approved provider contracts.
- [ ] Idempotency, concurrency and uniqueness requirements are explicit.
- [ ] Ledger invariants and correction model receive finance/accounting review.
- [ ] Secret material and personal evidence have dedicated handling and retention policies.
- [ ] Audit events cover privileged and consequential actions without leaking secrets.
- [ ] Derived summaries and exports have source/freshness, authorization and rebuild rules.
- [ ] Schema/migration/backup/restore implications are reviewed by the technical and operations owners.
- [ ] Tenant-isolation and integrity negative scenarios are mapped to DOC-31/DOC-34 and DOC-45.
- [ ] Provider, legal/compliance, finance and security assumptions are evidenced rather than inferred.
- [ ] G1/G2 approvals and explicit implementation authorization are recorded before coding.

## 12. Proposed verification themes (not tests)

These themes must later be mapped to stable DOC-31 requirement IDs and concrete tests under DOC-34/DOC-45. They are not test identifiers or evidence of execution.

- Tenant isolation for direct reads, list filters, foreign-key relationships, exports, jobs and support tools.
- Role revocation, key revocation, session expiry and capability changes taking effect safely.
- Idempotent operation creation under concurrent duplicate requests.
- Multiple provider attempts remaining linked to one canonical operation.
- Duplicate/out-of-order provider callbacks not causing duplicate financial postings.
- Unknown outcomes remaining unknown until authoritative recovery evidence is received.
- Refund limits remaining valid under concurrent refund requests.
- Payout approval and beneficiary change controls preventing unauthorized execution.
- Balanced ledger posting and immutable correction/compensation behavior.
- Reconciliation discrepancies remaining visible until evidence-backed resolution.
- Webhook delivery failures not mutating canonical payment state.
- Audit events correlating requests, operations, provider attempts and financial records without storing secrets.
- Secret material absent from API responses, logs, URLs, exports and repository files.
- Retention and deletion policies preserving required financial evidence without unnecessary personal data.
- Migration/restore exercises preserving referential integrity and financial invariants when explicitly authorized.

## 13. Current disposition

DOC-56 is a conceptual domain model and ownership matrix only. It is not a physical schema, database migration, selected service topology or accounting approval. It deliberately preserves unresolved choices around identity, tenancy structure, storage, provider account relationships, funds flow, ledger policy, retention and service boundaries. DOC-55 remains the conceptual frontend/API data-binding plan; DOC-41 and DOC-42 remain design proposals pending accountable review. G1 is NOT PASSED, G2 is NOT APPROVED. No tests were run and no backend/payment-engine implementation, provider contact, credential action, financial transaction, merge or deployment occurred as part of this documentation work.


## 14. Tenant-boundary enforcement contract — proposed clarification

The following rules make the existing ownership invariants operationally testable without selecting a database, queue, cache or object-storage product:

1. **Establish context at a trusted boundary.** For a merchant request, authenticate the principal/credential, resolve the active tenant membership and project/environment from server-side records, then create an immutable request authorization context. A body, query parameter, URL, header, callback field or client-generated correlation ID may identify a requested resource but cannot grant tenant authority.
2. **Carry identity, not authority-by-assertion, through async work.** A job/event must carry a stable internal resource reference, expected tenant/environment, initiating operation and correlation identifiers. On execution, load the authoritative record and verify that its stored ownership matches the job context and that current lifecycle/capability/revocation conditions still permit the action. Reject or quarantine mismatches; never silently substitute a tenant from the payload.
3. **Bind relationships at write time.** Every tenant-owned child and tenant-scoped association must be checked against its parent. Where the chosen store supports it, use database constraints or row-level policies as defence in depth; do not treat those mechanisms as selected until the architecture decision is recorded.
4. **Scope derived and stored artifacts.** Cache keys, search indexes, exports, object-storage references, generated download links, idempotency records, webhook subscriptions/deliveries, report queries and audit views must preserve tenant, environment and resource scope. Private artifacts must not be reachable through public object URLs; download authorization must be checked when the artifact is retrieved, not only when generated.
5. **Treat provider ingress separately.** Authenticate/validate the provider protocol, correlate the event to a previously persisted provider attempt, and derive tenant ownership from that attempt and its stored operation. Never route by an untrusted tenant ID supplied in a callback. An unmatched or conflicting callback goes to a restricted investigation path without mutating a merchant-visible outcome.
6. **Make privileged access explicit.** Platform support access requires a distinct permission path, a justified purpose/case, least privilege, time bounds where feasible, and auditable actor/target/action. Tenant support access must not inherit universal access; consequential financial corrections require the separately approved authorization and separation-of-duties policy.
7. **Fail closed and preserve evidence.** Missing, stale, contradictory or unverifiable tenant context denies the requested action and records a safe security/audit event. Do not leak whether another tenant's resource exists. Never put secrets or unnecessary personal/payment data in logs.
8. **Verification matrix (planned only):** guessed/altered IDs; tenant/project/environment substitution; revoked membership/key during queued work; forged callback tenant field; wrong-tenant parent/child relationship; cross-tenant cache collision; expired export link; support privilege expiry; replay after restore; and mismatched job context. Each scenario must eventually have an executed artifact and disposition before a release claim.

These are proposed design controls, not implementation facts. They do not select PostgreSQL RLS, a queue topology, a cache technology or a support-access product. G2 and implementation authorization remain separate gates.

## 15. Tenant-isolation decision package — product-owner-approved control direction, enforcement choices open

The product owner approves the internal security direction on 2026-10-24: tenant authority must be derived from trusted server-side identity and persisted ownership, revalidated at each consequential boundary, and fail closed on missing or contradictory context. Async work, provider callbacks, derived data and privileged support must not use caller-supplied tenant fields as authorization. This is not a claim that any control is implemented or independently verified.

**Required control contract**

1. **Request boundary:** authenticate the caller; resolve current tenant/project/environment and capability from authoritative server-side records; authorize the specific resource and action. A tenant ID in a body, path, query, header, callback or job payload is an untrusted selector only.
2. **Parent/child ownership:** validate every tenant-owned association at write and read time. Where possible, encode tenant-consistent relationships in schema constraints as defence in depth; the exact schema and database enforcement mechanism remain a technical decision.
3. **Database enforcement decision:** compare application-level scoped repositories, database constraints, row-level security (if supported by the selected database), or a layered combination. Evaluate bypass risk, migrations, worker/service identities, connection pooling/session context, admin access, query tooling, testability and operational failure modes. Do not declare RLS or any specific enforcement mechanism selected until a decision and independent security review are recorded.
4. **Async work:** persist a stable internal resource/operation ID, expected tenant/environment and correlation metadata. Workers reload the source record and revalidate stored ownership, current lifecycle, capability, revocation and idempotency before side effects. Ownership mismatch means stop/quarantine and audit; never silently rebind.
5. **Provider callbacks:** authenticate under provider-specific rules, correlate to a persisted provider attempt, and derive tenant ownership through the attempt-to-operation relation. Callback-supplied tenant/merchant fields are comparison evidence only. Unmatched or conflicting callbacks go to a restricted quarantine/investigation path and cannot directly mutate a merchant-visible final state or post the ledger.
6. **Derived artifacts:** cache keys, search/read models, exports, object storage, download links, idempotency records, webhook subscriptions/deliveries, reports and audit views preserve tenant/environment/resource scope. Private artifact access is checked again at retrieval; public object URLs are not authorization.
7. **Revocation consistency:** define effective revocation timing and propagation for API credentials, sessions, memberships, project state, provider capabilities and temporary support grants across API nodes, caches, queues and workers. If the system cannot establish current authorization safely, consequential work fails closed. Do not promise instantaneous revocation without a selected and verified consistency design.
8. **Privileged support:** require distinct permissions, purpose/case, target scope, least privilege, time bounds where feasible and immutable audit evidence. Read permission does not imply mutation. Financial correction remains within the approved operation/correction workflow and applicable maker-checker rules.
9. **Recovery:** after restore, queue redrive or cache rebuild, consequential work remains paused until tenant/resource ownership, authorization versions, idempotency and provider-attempt states are checked. Replay re-enters the same authorization gates.
10. **Verification evidence:** eventual negative tests must cover every applicable path: altered tenant/project/environment selectors, guessed IDs, wrong parent-child links, queued work after revocation, forged/misbound callbacks, cross-tenant cache collisions, export/download leakage, support-grant expiry, restore/replay and privileged mutation. Each result must be linked to its threat and requirement IDs with artifact, commit, environment, timestamp, result and reviewer; no tests are claimed here.

**Decisions and accountable roles**

| Decision | Accountable role | Exact question | Blocker |
|---|---|---|---|
| Data-store enforcement | Technical architecture owner + independent security reviewer | Which combination of schema constraints, scoped access layer and database policy prevents cross-tenant reads/writes across app, worker and admin identities, including connection pooling and migrations? | No live tenant data until reviewed design and later verification |
| Async envelope and fencing | Technical architecture owner + operations owner | What immutable references/version/fencing fields are persisted, and how are stale, replayed or mismatched jobs rejected? | Consequential worker actions remain gated |
| Callback correlation | Provider integration owner + technical owner | What evidence binds each provider callback to one stored attempt and handles missing/colliding references per provider/product/version? | Ambiguous callbacks quarantined; no final-state mutation |
| Revocation/cache consistency | Security owner + technical owner + operations owner | What is the maximum accepted propagation delay, how is invalidation enforced, and what happens during cache/identity-provider outage? | Do not claim immediate revocation or allow consequential work on stale authorization |
| Derived artifacts | Technical/platform owner + privacy/security reviewer | Which tenant-scoped key, object policy, retrieval authorization and expiry/revocation controls apply to caches, reports, exports, search and webhook delivery? | Private data exports/artifacts remain disabled until approved controls |
| Privileged support | Product/security owner + operations owner | Which roles, purpose/case fields, time bounds, audit, step-up and separation-of-duties rules apply? | No broad support access or direct financial override |
| Restore/replay | Operations owner + technical owner + finance reviewer | Who authorizes resumption and what ownership/authorization/financial checks are required after restore? | Consequential processing remains paused until authorized |

**Disposition:** the control direction is approved as product intent. Concrete enforcement choices, named accountable owners, independent security review and later authorized execution evidence remain outstanding. AR-010 and G2-04 remain OPEN; G1/G2 gates are unchanged.

