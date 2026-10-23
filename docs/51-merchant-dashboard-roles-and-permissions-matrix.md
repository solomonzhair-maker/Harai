# HARAI — Merchant Dashboard, Roles and Permissions Matrix

**Status:** Draft proposal — not implemented; identity architecture and G1/G2 reviews remain open  
**Date:** 2026-10-23  
**Related documents:** DOC-31, DOC-34, DOC-43, DOC-45, DOC-46, DOC-49, DOC-50  
**Decision boundary:** This document defines a proposed product and access-control model. It does not authorize implementation, provider access, live credentials, financial actions, merge, or deployment.

## 1. Purpose

Define the merchant-facing dashboard and internal HARAI operations console as a consistent, least-privilege system. It covers roles, permissions, tenant boundaries, sensitive actions, screen inventory, approval controls, support access, auditability, accessibility, and acceptance-test planning.

The dashboard must make it easy for a legitimate business or developer to register with HARAI, understand its activation status, integrate through the HARAI API, and operate only the capabilities explicitly enabled for its account. Merchant users should not need provider API credentials merely to call HARAI's API; any provider-side sub-merchant registration, settlement relationship, KYC, or authorization remains dependent on the actual provider contract and approved operating model.

Production-first onboarding means sandbox is optional, not that security, verification, provider authorization, finance/compliance review, or release gates can be bypassed.

## 2. Design principles

1. **Tenant isolation by default:** Every merchant-owned resource is scoped to a verified tenant on the server. A tenant ID supplied by the browser is never sufficient authorization.
2. **Least privilege:** Users receive only the permissions needed for their role and assigned scope. No default all-powerful merchant role beyond the verified owner.
3. **Server-side authorization:** UI visibility improves usability but is never the security boundary. Every API request and object access must be authorized server-side.
4. **Capability-based activation:** Collections, money-out, refunds, cards, reports, webhooks and other features are independently gated by policy and verified eligibility.
5. **Safe production-first flow:** Users can read docs, create a project and request production assessment without mandatory sandbox usage. Real-money capabilities remain disabled until all applicable gates pass.
6. **Separation of duties:** High-impact financial, security, ownership, payout-destination and access-control changes require step-up verification and, where configured, a second authorized approver.
7. **No secret re-display:** API secrets and webhook signing secrets are displayed only at creation/rotation where technically appropriate; stored values are not retrievable by ordinary users.
8. **Auditable administration:** Sensitive actions record actor, tenant, target, time, reason, outcome, correlation ID and approval trail without logging credentials or unnecessary personal data.
9. **Support access is scoped and temporary:** Internal support staff do not silently impersonate merchants or gain blanket access to sensitive values.
10. **Accessible, responsive UI:** Core workflows work on mobile and desktop, support keyboard navigation and assistive technology, and present clear loading, empty, error, pending and blocked states.

## 3. Role model

Roles are proposed templates, not named people or granted access. Permissions should be implemented as explicit capabilities and resource scopes rather than relying only on role-name checks.

### 3.1 Merchant roles

| Role | Intended purpose | Default boundaries |
|---|---|---|
| Merchant Owner | Account owner and primary business authority | Manage merchant profile and members; nominate admins; request capabilities; view financial records; approve high-impact actions subject to policy |
| Merchant Admin | Day-to-day account administration | Manage permitted users, projects and settings; cannot change legal ownership or bypass owner/approval gates |
| Finance | Reconciliation, transactions, refunds and financial reporting | Read financial data and initiate permitted finance workflows; money-out/refunds require specific capability and approval policy |
| Developer / Integration | API integration, keys, webhook endpoints and technical diagnostics | Manage assigned projects and technical settings; no default access to beneficiary changes, money-out execution, or full financial exports |
| Analyst / Read-only | Business visibility and reporting | Read only explicitly scoped reports and transaction summaries; no mutations, secrets or unrestricted exports |
| Support Contact | Raise and track support cases | View own cases and explicitly shared case information; no broad transaction or member access by default |

Each merchant can have multiple users. The initial verified owner must be established through the approved identity and onboarding process. Ownership transfer must be a controlled, audited workflow; an ordinary admin cannot silently replace the owner.

### 3.2 Internal HARAI roles

| Role | Intended purpose | Restrictions |
|---|---|---|
| Support Agent | Triage and respond to cases | Scoped case access; masked data; no direct financial mutation or unlogged impersonation |
| Operations Analyst | Monitor service health, queues and unresolved transactions | Read operational metadata; sensitive merchant data masked unless specifically justified |
| Finance / Reconciliation Operator | Investigate settlement and reconciliation exceptions | Case-bound financial access; corrections require a separately authorized workflow |
| Compliance / Risk Reviewer | Review verification, risk flags and capability eligibility | Access only the records required for the review; decisions and reasons are audited |
| Security Operator | Investigate security events and credential abuse | Security-scoped access; no routine business financial privileges |
| Release / Platform Operator | Manage service reliability and approved releases | Infrastructure permissions separated from merchant business approvals |
| Internal Auditor | Read audit evidence and control outcomes | Read-only, tamper-evident audit access; no operational mutations |
| Superadministrator / Break-glass | Recover platform access during exceptional incidents | Disabled for routine use; time-limited, reason-coded, step-up authenticated, alerted and independently reviewed |

No internal role automatically has permission to move money, alter ledger entries, change a merchant's payout destination, reveal a secret, or approve its own sensitive action.

## 4. Permission catalogue

Use stable permission identifiers in implementation and policy tests. The following names are proposals and should be aligned with the final identity architecture.

| Permission family | Example permissions | Baseline grant |
|---|---|---|
| Merchant profile | `merchant.profile.read`, `merchant.profile.update` | Owner; limited Admin |
| Members and roles | `merchant.members.read`, `merchant.members.invite`, `merchant.members.role.update`, `merchant.members.revoke` | Owner; Admin within allowed role ceiling |
| Projects and API | `project.read`, `project.create`, `api_key.create`, `api_key.revoke`, `api_key.rotate` | Owner/Admin; Developer within assigned projects |
| Webhooks | `webhook.read`, `webhook.configure`, `webhook.secret.rotate`, `webhook.delivery.replay` | Owner/Admin/Developer as scoped; replay is separately audited and rate-limited |
| Transactions | `transaction.read`, `transaction.export` | Owner, Finance, scoped Analyst; export separately gated |
| Refunds | `refund.request`, `refund.approve` | Finance/Owner subject to enabled capability and approval policy |
| Money-out | `payout.create`, `payout.approve`, `beneficiary.create`, `beneficiary.change` | Not granted by default; require provider/model eligibility, risk controls and explicit policy |
| Reconciliation | `reconciliation.read`, `reconciliation.case.assign`, `reconciliation.propose_correction`, `reconciliation.approve_correction` | Finance/authorized internal finance; proposer cannot approve own correction |
| Reports | `report.read`, `report.export` | Scope-limited; exports logged and filtered |
| Verification | `verification.read`, `verification.submit`, `verification.review`, `capability.request` | Merchant submits; qualified internal reviewer decides |
| Support | `support.case.create`, `support.case.read`, `support.case.assign`, `support.access.request` | Scoped to requester, assigned case or authorized support role |
| Audit | `audit.merchant.read`, `audit.internal.read` | Narrowly granted read-only access |
| Platform policy | `platform.policy.read`, `platform.policy.change` | Explicit platform governance roles; dual control for high-impact changes |

### 4.1 Permission evaluation rules

Authorization should combine:
- **Role permissions** — baseline actions allowed for a role.
- **Resource scope** — tenant, project, case, report or transaction scope.
- **Capability state** — whether the merchant and requested operation are activated.
- **Risk / context controls** — step-up authentication, recent credential change, unusual destination, velocity limit, device/session risk or other approved controls.
- **Approval state** — whether a required second approver has independently approved the exact action and immutable payload.
- **Explicit deny rules** — suspended users/merchants, revoked sessions, blocked capabilities and prohibited actions override role grants.

The default decision is deny when required policy or evidence is missing, contradictory or unavailable.

## 5. Separation-of-duties and sensitive actions

The following require explicit policy and tests before implementation:

| Action | Proposed control |
|---|---|
| Change legal business ownership or primary owner | Strong identity verification, documented evidence, independent review, notification to existing owner where safe, audit trail |
| Enable production capability | Verification and eligibility decision plus relevant provider, legal/compliance, security, finance and operations gates; no self-approval by applicant |
| Create or change payout beneficiary | Step-up authentication, cooling-off or risk review where appropriate, out-of-band notification, second approval for configured high-risk cases |
| Initiate money-out | Capability must be enabled; enforce limits, available funds, idempotency, beneficiary policy and required maker-checker approval |
| Approve refund or financial correction | Separate initiator and approver where policy requires; validate original transaction and provider support; immutable reason/evidence |
| Rotate/revoke API or webhook secret | Step-up authentication, confirmation, notification, audit event; provide overlap/rotation procedure where safe |
| Change user roles or invite privileged users | Enforce role ceiling; prevent privilege escalation; notify owner; audit before/after permissions |
| Export financial or personal data | Permission, least-data selection, export audit, short-lived download and applicable privacy controls |
| Internal support access to sensitive records | Case-bound justification, minimum scope, time limit, access log and review; no silent impersonation |
| Break-glass access | Justification, limited duration, alerts, all actions audited and independent retrospective review |
| Correct ledger/reconciliation records | Never edit posted history; use a linked, authorized compensating operation under DOC-42 review |

The exact monetary thresholds, approval quorum, cooling-off periods, and step-up factors remain open decisions. Do not invent numeric defaults in code.

## 6. Tenant isolation and data access

1. Resolve the user's active tenant from authenticated server-side membership, not a client-supplied tenant ID alone.
2. Check tenant ownership on every object read and write, including nested objects, exports, webhook deliveries, file references, support cases and background jobs.
3. Use database constraints and scoped queries as defense in depth. If row-level security is selected, document connection-pool/session-context safety and test it; do not assume RLS alone is sufficient.
4. Never use sequential IDs, unguessable IDs or hidden UI controls as authorization.
5. Ensure search, pagination, counts, dashboards, caches, background jobs and exports cannot leak cross-tenant existence or metadata.
6. When a user belongs to multiple businesses, require explicit tenant switching and show the active business context prominently.
7. On role revocation, tenant suspension or account closure, invalidate or re-evaluate sessions and tokens promptly.
8. Test every sensitive endpoint using a second tenant's identifiers and confirm that responses reveal neither data nor unintended existence.
9. Support access must be authorized by case/purpose and expire; internal access is not a shortcut around tenant controls.
10. Audit logs must avoid secrets, authentication tokens, full payment credentials and unnecessary personal data.

## 7. Dashboard screen inventory

### 7.1 Merchant-facing screens

| Screen | Key content and actions | Important states / controls |
|---|---|---|
| 1. Sign-in and account recovery | Sign-in, MFA/step-up where required, secure recovery | Rate-limited, locked, recovery pending; do not reveal whether arbitrary emails exist |
| 2. Business setup / onboarding | Business details, owners/representatives, required documents, progress and requests for missing evidence | Draft, submitted, action required, under review, approved, rejected, suspended |
| 3. Home dashboard | Collections summary, transaction status, enabled capabilities, service notices, integration health | No data, delayed data, provider incident, limited capability; distinguish pending from success |
| 4. Transactions | Search/filter, details, provider references where safe, status history, linked refunds | Pending/unknown/expired/failed/succeeded clearly differentiated; masked sensitive fields |
| 5. Collections and payment links | Create supported payment request/link, inspect status and expiry | Only enabled methods; do not imply payment completion before authoritative evidence |
| 6. Money-out | Beneficiaries, payout request, approval queue, status and returns | Hidden/disabled until authorized; step-up, limits, funding and approval blockers explained |
| 7. Refunds and reversals | View eligible operations, request refund, track result | Separate refund, cancellation, reversal and return; provider-specific availability |
| 8. Reconciliation and settlements | Settlement summaries, unmatched items, discrepancy cases, export if authorized | Data freshness and missing provider reports visible; no silent discrepancy dismissal |
| 9. Developer projects | Create project, API docs, environment/capability status, integration checklist | Production-first path; optional sandbox; clear distinction between HARAI API key and provider credentials |
| 10. API keys and access controls | Create/revoke/rotate scoped keys, last-used metadata, IP restrictions if enabled | Secret shown only once; no key in logs; warn on rotation impact |
| 11. Webhooks | Configure HTTPS endpoint, signing-secret rotation, delivery history, safe replay | Signature status, attempts, response codes, deduplication; secrets masked |
| 12. Team and roles | Invite/revoke members, assign allowed roles, review pending invitations | Role ceiling, owner protections, audit history |
| 13. Reports and exports | Filtered financial and operational reports | Permission checks, export audit, time-limited files, clear timezone/currency |
| 14. Notifications and support | Capability changes, security alerts, service status, support cases | Priority, ownership, history, safe disclosure |
| 15. Business settings | Legal profile, settlement configuration where permitted, notification preferences | Sensitive changes require review; show immutable audit history |
| 16. Audit activity | Merchant-visible security, key, role and settings activity | Read-only; redact secrets and unrelated users' personal data |

### 7.2 Internal HARAI console

| Screen | Intended audience | Controls |
|---|---|---|
| Merchant queue and verification cases | Compliance/Risk | Evidence checklist, decision reason, reviewer separation, history |
| Capability and provider eligibility | Authorized operations/compliance | Per-capability status, evidence links, expiry/review dates; no manual bypass without authorized workflow |
| Provider health and operation queues | Operations | Adapter status, latency/error trends, pending/unknown age, safe recovery actions |
| Reconciliation and discrepancy workbench | Finance/Operations | Match evidence, case ownership, proposed correction, separate approval |
| Support case workspace | Support | Case-bound access, masking, reason for elevated access, time-limited grants |
| Security and abuse console | Security | Alerts, key revocation, rate-limit signals, incident linkage |
| Audit and approval history | Auditor / authorized reviewers | Read-only evidence, actor/approver chain, filters and export controls |
| Platform configuration and release gates | Platform/release roles | Change proposal, review, versioned configuration, rollback evidence; separate from financial approval |

## 8. Merchant API key and provider credential boundary

- Merchant developers authenticate requests to HARAI using HARAI-issued credentials scoped to their project and permissions.
- HARAI's server-side provider adapters use provider credentials held in an approved secrets system, if and when a lawful and contractually authorized integration exists.
- Do not ask every merchant to paste their own MTN/Airtel API secret into HARAI unless a separately approved direct-merchant integration model explicitly requires it.
- A merchant may still require KYC, provider-side sub-merchant registration, a merchant relationship identifier, an approved settlement arrangement, or other onboarding obligations. These are not the same as requiring each merchant to own separate API integration credentials.
- No provider credentials may be used for third-party processing until written provider authority and the exact operating model are established.
- Never expose provider credentials in the dashboard, API responses, support tooling, logs, browser storage, repository or client-side bundles.

## 9. UI states and interaction standards

Every data-driven screen must account for:
- Initial loading and refresh.
- Empty state with a safe, role-appropriate next step.
- Partial data or stale provider information.
- Pending / unknown outcome that requires enquiry or reconciliation.
- Permission denied without leaking hidden resource details.
- Capability unavailable, not requested, under review, suspended or expired.
- Validation errors with field-level explanation.
- Retryable technical failure versus non-retryable business rejection.
- Destructive action confirmation and clear result.
- Session expiry and revoked access.
- Offline/poor-network indication without claiming an action succeeded.
- Mobile responsive layout, keyboard operation, visible focus, accessible labels and status announcements.

Never represent a timed-out payment request as failed or successful without authoritative lifecycle evidence. A retry must preserve idempotency rules and avoid creating duplicate financial effects.

## 10. Notifications and audit events

Notify affected users through configured channels for security-sensitive events, subject to safe delivery and privacy controls:
- New member invitation, role change, removal and ownership change.
- API-key creation, rotation, revocation and suspicious use.
- Webhook secret change and endpoint security change.
- Production capability request, decision, suspension and reinstatement.
- Payout beneficiary changes and high-impact money-out actions.
- Refund/correction approvals and completion or unresolved outcomes.
- Export generation and privileged support access.
- Password/MFA recovery and account security events.

Audit records should include event ID, timestamp, actor, actor role, tenant, target resource, action, result, reason, approval reference, correlation ID and safe before/after diff. Exclude raw secrets, bearer tokens, full identity-document contents and unnecessary personal data. Audit retention and access policy require privacy/legal review.

## 11. Acceptance criteria and proposed test plan

These IDs are plans, not implemented or executed tests. Map them to stable DOC-31 requirement IDs before treating the traceability matrix as complete.

| Proposed test ID | Acceptance intent |
|---|---|
| IAM-DASH-001 | Each role sees only its permitted navigation and actions; server APIs enforce the same policy |
| IAM-TENANT-001 | Cross-tenant reads, writes, searches, exports and nested-resource access are denied without leaking existence |
| IAM-ROLE-002 | A user cannot grant a role or permission beyond their own delegation ceiling |
| IAM-OWNER-003 | Ownership transfer requires the documented verified workflow and produces an auditable approval record |
| IAM-MAKER-CHECKER-004 | A sensitive action cannot be approved by its initiator where separation is required |
| IAM-CAPABILITY-005 | Unapproved/suspended capabilities cannot be used through UI, API, background jobs or direct endpoint calls |
| IAM-KEY-006 | API secrets are shown only at creation/rotation, cannot be retrieved later, and never appear in logs or browser persistence |
| IAM-WEBHOOK-007 | Webhook secret rotation and delivery replay are permissioned, audited and safe against duplicate side effects |
| IAM-SUPPORT-008 | Internal support access is case-bound, time-limited, masked and logged; expired access is denied |
| IAM-AUDIT-009 | Security/financial events produce complete, tamper-resistant audit records without secrets |
| IAM-EXPORT-010 | Unauthorized exports are denied; permitted exports are scoped, audited and expire safely |
| IAM-SESSION-011 | Role revocation, merchant suspension and session expiry invalidate access within the approved policy window |
| IAM-PROD-012 | Developers can read docs, create a project and request direct production assessment without mandatory sandbox use; activation still requires all applicable gates |
| IAM-STATE-013 | Pending/unknown transactions are never presented as confirmed success/failure without authoritative evidence |
| IAM-UX-014 | Core workflows pass agreed mobile, keyboard, screen-reader and responsive acceptance checks |
| IAM-BREAKGLASS-015 | Break-glass use requires justification, time limit, alerting, full audit and independent review |

Test cases should include positive and negative cases, concurrency, stale sessions, role changes during an active session, background workers, cached data, API direct calls, export/download URLs, support tooling and recovery after service restart. Use synthetic fixtures only. Do not claim test execution until results, commit, environment, date and reviewer are recorded.

## 12. Open decisions

1. Identity provider, authentication methods, MFA/step-up strategy and session lifetime.
2. Whether role grants are strictly role-based or policy-based with attributes; final permission schema.
3. Named accountable owners and independent reviewers.
4. Which merchant capabilities and financial operations are enabled in the first production release.
5. Exact approval thresholds, maker-checker quorum, cooling-off periods and emergency controls.
6. Whether internal support can use a limited impersonation-like view; default recommendation is no silent impersonation.
7. Audit retention, data residency, export expiry and privacy obligations after qualified review.
8. Tenant database enforcement approach, including whether PostgreSQL RLS is selected.
9. Notification channels, delivery policy and security of out-of-band approvals.
10. Final design system, visual layout, accessibility standard and browser support.
11. Mapping each proposed test to DOC-31 IDs and eventual concrete test artefacts.

## 13. Review checklist and exit criteria

Before this proposal can be approved for implementation:
- [ ] Product owner reviews merchant roles, screen inventory and production-first journey.
- [ ] Technical lead validates the permission model against the chosen identity and service architecture.
- [ ] Independent security reviewer evaluates tenant isolation, support access, secrets, sessions and maker-checker controls.
- [ ] Finance/reconciliation reviewer validates financial action permissions and separation of duties.
- [ ] Qualified privacy/legal review checks identity evidence, retention, exports and support access.
- [ ] DOC-31 and DOC-34 mappings are updated with stable requirement IDs and proposed test IDs.
- [ ] Each sensitive action has an explicit policy, owner, evidence requirement and denial behavior.
- [ ] No claim is made that provider access, production readiness, tests or implementation exist without evidence.

**Current disposition:** Draft proposal only. G1 is not passed, G2 architecture is not approved, tests have not been run as part of this document, and no implementation or provider action is authorized by its creation.
