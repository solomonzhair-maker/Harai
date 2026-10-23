# HARAI — Frontend Screen-to-Component Implementation Blueprint

**Document ID:** DOC-54  
**Status:** Draft implementation-planning blueprint — not approved for coding  
**Date:** 2026-10-23  
**Depends on:** DOC-31, DOC-34, DOC-45, DOC-47, DOC-48, DOC-49, DOC-50, DOC-51, DOC-52 and DOC-53.  
**Implementation gate:** This document does not authorize application coding, payment-engine implementation, tests, provider contact, live credentials, real transactions, merge or deployment.

## 1. Purpose

Translate the proposed HARAI screen inventory, permissions and reusable design system into a screen-by-screen implementation reference. It identifies proposed routes, shared components, data concepts, user actions, capability and permission dependencies, screen states, audit needs and acceptance criteria.

This is a planning document, not final API schema, frontend code, clickable prototype or test evidence. Exact API fields, component props, routes, framework and state-management approach must come from approved contracts and architecture decisions rather than assumptions.

## 2. Source-of-truth rules

1. DOC-31 is the stable launch-requirement and acceptance-criteria source.
2. DOC-34 is the requirement-to-owner/test/evidence mapping source; proposed mappings are not test evidence.
3. DOC-51 is the proposed role, permission, tenant and sensitive-action policy.
4. DOC-52 is the proposed navigation, route and screen behavior.
5. DOC-53 is the proposed component, token, responsive and accessibility specification.
6. DOC-41–DOC-48 contain draft architecture, lifecycle, ledger, threat and provider-adapter proposals; they remain subject to review.
7. If documents conflict, record the conflict and block affected behavior rather than inventing a resolution.
8. Frontend guards improve usability but never replace server-side authorization, tenant checks, capability checks, idempotency, financial validation or approval rules.
9. Do not fabricate metrics, provider statuses, balances, fees, settlement dates, successful payments, verification decisions or test results.
10. Use synthetic fixtures only. Do not put credentials, session tokens, real payer details or provider secrets in examples, screenshots, logs or test data.

## 3. Proposed frontend boundaries

- **Merchant shell:** merchant navigation, account/project context, breadcrumbs, page title, global notices, content region and accessible mobile navigation.
- **Internal console shell:** separate protected namespace and navigation; internal staff do not automatically receive broad access to merchant data.
- **Shared UI package:** design tokens, buttons, inputs, tables, status components, dialogs, alerts, timelines and other DOC-53 components.
- **Screen feature modules:** route-level composition, loading/data/error states, validation and action orchestration.
- **API client boundary:** typed calls against approved HARAI API contracts, safe error classification and correlation/reference handling.
- **Access policy boundary:** current session and server authorization determine allowed operations; UI visibility is not a security boundary.
- **Audit/telemetry boundary:** approved event schema with no secret or unnecessary personal-data leakage.
- **Test fixture boundary:** synthetic, clearly labelled data only; no provider production credentials or live financial operations.

Exact framework, package structure, routing library, query/cache library and state-management library remain open until architecture approval.

## 4. Standard screen-state contract

Every data-driven screen must define the states relevant to its function.

| State | Expected behavior |
|---|---|
| Loading | Stable layout or suitable skeleton; never use false zero values as placeholders |
| Populated | Authorized, validated data with clear units, status and freshness context |
| Empty | Explain whether no records exist or no records match filters; offer a permitted next action |
| Filtered empty | Keep filters visible and provide a safe clear/reset action |
| Partial/stale | Identify incomplete or potentially outdated data and a safe refresh/recovery path |
| Recoverable error | Preserve safe input; explain next step and reference where available |
| Permission denied | Explain unavailable access without leaking protected resource existence |
| Capability blocked | State which capability is unavailable and the approved request/review path |
| Pending | Process is ongoing; do not claim final outcome |
| Unknown | Outcome unresolved; show reference and safe authoritative recovery path; prevent blind duplicate action |
| Suspended/closed | Show recorded state and permitted support/review path |
| Offline/unavailable | Distinguish connectivity failure from provider/payment status; do not expose sensitive cached data |

### Standard layout

Unless a screen has a documented reason to differ:
1. Breadcrumbs and page title.
2. Concise purpose statement or status context.
3. Primary permitted action(s).
4. Relevant filters or context controls.
5. Main content: cards, table, form or detail panel.
6. Associated loading, empty and error states.
7. Audit/status/history or support reference where relevant.

Avoid duplicating the same action in multiple places without a clear usability reason.

## 5. Route-to-screen implementation matrix

Routes are proposals aligned to DOC-52 and must be reconciled with the final router. A route existing in this inventory does not mean it is available to every account or role.

| ID | Proposed route | Screen | Shared components and screen-specific composition |
|---|---|---|---|
| FE-001 | /sign-in | Sign in | FormField, Button, InlineAlert; sign-in form and approved challenge/step-up slot |
| FE-002 | /recover-access | Recover access | FormField, Button, InlineAlert; neutral recovery request and result |
| FE-003 | /business/home | Merchant dashboard | AppShell, PageHeader, MetricCard, StatusBadge, DataTable; capability summary, recent activity and action list |
| FE-004 | /business/verification | Business verification | StepList, StatusBadge, attachment guidance, Timeline; evidence checklist and review history |
| FE-005 | /business/transactions | Transaction list | FilterBar, DataTable, Pagination, StatusBadge; filters, result count and authorized export |
| FE-006 | /business/transactions/:id | Transaction detail | PageHeader, StatusPanel, Timeline, Tabs; lifecycle, provider-attempt summary, linked operations and permitted recovery actions |
| FE-007 | /business/collections | Collections and links | FilterBar, DataTable, StatusBadge; collection/link state, expiry and supported method |
| FE-008 | /business/collections/new | Create collection/link | FormLayout, AmountField, Select, DateTimeField, ReviewPanel; amount, supported method, reference and expiry |
| FE-009 | /business/refunds | Refund list | FilterBar, DataTable, StatusBadge; refunds linked to original collections |
| FE-010 | /business/refunds/:id | Refund detail/request | AmountSummary, StatusPanel, Timeline, ConfirmationDialog; eligibility, refundable amount, reason and approval state |
| FE-011 | /business/money-out | Money-out list | FilterBar, DataTable, StatusBadge; capability-gated payout/withdrawal records |
| FE-012 | /business/money-out/new | Initiate money-out | FormLayout, AmountField, Select, ReviewPanel, ConfirmationDialog; beneficiary, limits/funding evidence and approvals |
| FE-013 | /business/beneficiaries | Beneficiaries | DataTable, FormField, MaskedValue, ConfirmationDialog; add/update destination and approved safety steps |
| FE-014 | /business/reconciliation | Reconciliation overview | MetricCard, FilterBar, DataTable, StatusBadge; source/period summary and unmatched items |
| FE-015 | /business/reconciliation/:id | Discrepancy detail | AmountSummary, Timeline, FormField, ReviewPanel; evidence, owner, proposed disposition and approval |
| FE-016 | /developers/projects | Developer projects | PageHeader, DataTable, StatusBadge, EmptyState; project list, capability and integration status |
| FE-017 | /developers/projects/new | Create project | FormLayout, FormField, ReviewPanel; project name, purpose and approved configuration |
| FE-018 | /developers/projects/:id | Project detail | PageHeader, Tabs, StatusBadge, CodeBlock; docs, approved base URL/version and project status |
| FE-019 | /developers/api-keys | API key inventory | DataTable, StatusBadge, MaskedValue, ConfirmationDialog; scope, expiry, rotation and revocation |
| FE-020 | /developers/api-keys/new | Create API key | FormLayout, Select, ReviewPanel, SecretRevealPanel; project, minimum scopes, expiry and one-time reveal |
| FE-021 | /developers/webhooks | Webhook configuration | PageHeader, DataTable, StatusBadge, FormField; endpoint, event subscriptions and masked secret state |
| FE-022 | /developers/webhooks/:id | Webhook detail | PageHeader, Timeline, DataTable, CodeBlock; delivery attempts, redacted payload and authorized replay |
| FE-023 | /business/team | Team | PageHeader, DataTable, StatusBadge, Select; role assignments, invitations and delegation ceilings |
| FE-024 | /business/reports | Reports and exports | PageHeader, FilterBar, DataTable, EmptyState; definitions, periods, freshness and export jobs |
| FE-025 | /business/audit | Merchant audit | PageHeader, FilterBar, Timeline, DataTable; read-only scoped event history |
| FE-026 | /business/settings | Settings | PageHeader, FormLayout, FormField, ConfirmationDialog; profile and permitted settings |
| FE-027 | /notifications | Notifications | PageHeader, FilterBar, List, StatusBadge; read/unread state and authorized destinations |
| FE-028 | /support/cases | Support case list | PageHeader, FilterBar, DataTable, StatusBadge; own/authorized cases |
| FE-029 | /support/cases/new | Create support case | FormLayout, FormField, InlineAlert; safe description and related HARAI reference |
| FE-030 | /support/cases/:id | Support case detail | PageHeader, Timeline, FormField, StatusBadge; authorized messages and case state |
| FE-031 | /internal/verification | Internal verification queue | InternalShell, FilterBar, DataTable, StatusBadge; assignments and evidence-based review |
| FE-032 | /internal/capabilities | Production eligibility | InternalShell, DataTable, StatusPanel, Timeline; separate approval, provisioning and enablement |
| FE-033 | /internal/operations | Unresolved operations | InternalShell, FilterBar, DataTable, Timeline; pending/unknown queue and safe recovery |
| FE-034 | /internal/reconciliation | Internal reconciliation | InternalShell, FilterBar, DataTable, StatusBadge; discrepancy ownership and corrections |
| FE-035 | /internal/support | Internal support | InternalShell, FilterBar, DataTable, StatusBadge; case-bound access and scoped views |
| FE-036 | /internal/security | Security/abuse alerts | InternalShell, FilterBar, DataTable, StatusBadge; triage, assignment and disposition |
| FE-037 | /internal/approvals | Approval queue | InternalShell, FilterBar, DataTable, ReviewPanel, ConfirmationDialog; maker-checker decisions |
| FE-038 | /internal/audit | Internal audit | InternalShell, FilterBar, Timeline, DataTable; read-only, permission-scoped evidence |
| FE-039 | /internal/releases | Release/incident status | InternalShell, StatusPanel, Timeline, InlineAlert; operational notices and runbook links |

## 6. Shared component contracts

These contracts describe expected behavior, not final TypeScript props or code.

### 6.1 AppShell and InternalShell
**Inputs:** current route, authorized navigation entries, account/project context and global notices.  
**Responsibilities:** layout, active route, responsive navigation, landmarks and page heading.  
**Must not:** infer permissions from route names, expose internal screens to merchant users or cache sensitive content beyond approved policy.  
**Checks:** keyboard navigation, mobile drawer, focus restoration, session expiry and access revocation.

### 6.2 PageHeader and ActionGroup
**Inputs:** title, optional description/breadcrumbs/status and actions filtered for usability.  
**Responsibilities:** consistent hierarchy and action placement.  
**Must not:** imply that hiding an action replaces server authorization.

### 6.3 StatusBadge and StatusPanel
**Inputs:** authoritative status, label, optional timestamp/reference and approved explanatory copy.  
**Responsibilities:** shared vocabulary, accessible text/icon and appropriate emphasis.  
**Must not:** derive payment success from HTTP success, callback arrival, optimistic UI or elapsed time. Unknown is visibly non-final and includes safe recovery guidance.

### 6.4 FilterBar, DataTable and Pagination
**Inputs:** authorized columns, filters, sorting, page state and result data from approved APIs.  
**Responsibilities:** accessible controls, visible filter context, stable row actions and truthful empty states.  
**Must not:** leak cross-tenant counts/records, use client values as authorization or export broader data than the authorized query.

### 6.5 FormLayout, FormField and ReviewPanel
**Inputs:** approved schema, safe current values, field errors and submission state.  
**Responsibilities:** labels, hints, errors, summary, review and safe recovery.  
**Must not:** rely on client validation, silently alter material values or repeat financial side effects after an ambiguous result.

### 6.6 ConfirmationDialog
**Inputs:** exact action/target, amount/currency or permission change, reversibility statement and required reason.  
**Responsibilities:** informed confirmation and accessible focus management.  
**Must not:** substitute for step-up authentication, server authorization, approval quorum or idempotency.

### 6.7 Timeline and EventInspector
**Inputs:** normalized event category, timestamp/timezone, safe actor/source, outcome, reference and redacted details.  
**Responsibilities:** distinguish transaction lifecycle, provider attempt, webhook delivery, approval and audit events.  
**Must not:** equate webhook delivery with payment success or expose secrets/raw sensitive payloads by default.

### 6.8 SecretRevealPanel and MaskedValue
**Responsibilities:** one-time reveal when supported, safe copy feedback, storage guidance and masking after dismissal.  
**Must not:** put secrets in URLs, analytics, console logs, telemetry, audit descriptions or later list responses.

### 6.9 CapabilityCard
**Responsibilities:** show capability, evidence-backed state, scope, limitation and permitted next action. Distinguish request, review, provider/processor approval, credential provisioning and enablement. Never show generic “Live” unless every applicable gate is satisfied.

### 6.10 ApprovalQueueItem
**Responsibilities:** show requester, action, target, amount/currency or permission delta, permitted policy context and available decisions. Prevent self-approval where required, reject stale/duplicate decisions and capture reasons/audit identity.

## 7. Screen data and action matrix

This is domain-level planning. Exact API fields, endpoint names and response schemas must be approved separately.

| Screen family | Required data concepts | User actions | Critical dependency |
|---|---|---|---|
| Authentication/recovery | Authentication outcome, safe challenge/recovery state, session state | Sign in, request recovery, complete approved step-up | Approved identity/session architecture |
| Home | Account state, capability states, authorized summaries, recent activity, pending tasks | Navigate to permitted setup, docs, transactions or support | Authoritative data and freshness; no fabricated metrics |
| Verification | Business profile, required evidence, submission versions, review state | Save draft, submit, correct, view decision | Approved KYC/data-minimization policy and review authority |
| Transactions | HARAI reference, operation type, amount/currency, lifecycle, timestamps, linked references | Filter, view, permitted refresh/export | DOC-41 lifecycle, API contract and tenant scope |
| Collections | Supported amount/currency/method, reference, expiry and link state | Create request/link, view, permitted disable/expire | Capability, idempotency and provider operation mapping |
| Refunds | Original collection, eligible/refunded amount, reason, approval/status | Request refund, view linked operation | Atomic amount bounds, provider support and approval policy |
| Money-out/beneficiaries | Masked destination, funding/limit evidence if defined, fees only if confirmed | Add/update destination, initiate/approve request | Approved funds-flow, capability, limits and dual control |
| Reconciliation | Provider/source, period, totals, unmatched records, discrepancy ownership/evidence | Assign, classify, propose disposition | DOC-42, authoritative source and immutable correction rules |
| Developer projects | Project metadata, capability state, environment labels, integration status | Create project, view docs/settings | Approved API/version/base URL contract |
| API keys | Non-secret key ID/prefix, scopes, expiry, rotation/revocation state | Create, rotate, revoke | Credential model, one-time reveal, authorization and audit |
| Webhooks | Endpoint label, event subscriptions, secret status, delivery attempts | Configure, rotate, inspect, authorized replay | SSRF controls, signatures, replay/idempotency and redaction |
| Team | Membership, role, invitation and delegation constraints | Invite, change role, revoke membership | Role matrix, last-owner and revocation controls |
| Reports/exports | Definition, period/timezone/currency, filters, freshness, export-job state | Generate/view/export within scope | Permission, bounded scope, expiration and audit |
| Audit/notifications/support | Event/case/notification state, actor, time and safe reference | Mark read, open authorized detail, reply to case | Destination authorization, privacy and scoped support |
| Internal queues | Assigned case, evidence, capability/operation state, permitted review context | Assign, review, approve/reject where allowed, recover safely | Explicit staff role, audit, separation of duties and runbooks |

## 8. Role and capability gating

The following is a frontend presentation guide, not the final permission source.

| Action family | UI prerequisites | Server-side enforcement required |
|---|---|---|
| View merchant data | Active session and relevant tenant/resource access | Membership, tenant scope, object authorization and data minimization |
| View transaction | Permission for operation/resource | Object-level authorization and safe handling of guessed IDs |
| Create collection | Collection capability, suitable project/account and permission | Capability, amount/currency/method, idempotency, risk limits and tenant scope |
| Request refund | Refund capability, eligible original operation and permission | Atomic eligible amount, provider support, policy approval and idempotency |
| Initiate money-out | Capability, beneficiary access and initiation permission | Funding/limit checks, beneficiary validation, risk policy, idempotency and approval |
| Approve money-out/refund/correction | Approver permission and independent reviewer where required | Separation of duties, quorum, freshness, reason, limits and audit |
| Create/rotate/revoke API key | Project access and key-management permission | Scope ceiling, credential lifecycle, step-up if required and audit |
| Configure/replay webhook | Project/webhook permission and authorized endpoint | SSRF controls, secret policy, replay limits, idempotency and audit |
| Invite/change role | Team-management permission and delegation ceiling | Role grant ceiling, last-owner protection, invitation integrity and revocation |
| Export report/audit | Export permission and approved filters | Tenant scope, data minimization, bounded export, expiration and audit |
| Internal review/support | Explicit internal role and queue/case scope | Internal namespace authorization, reason, time-bounded access and audit |
| Capability activation | Authorized decision role and complete evidence | Provider/processor permission, verification, compliance/security/finance/operations gates and recorded decision |

## 9. Critical user journeys

### 9.1 Create a collection/payment link
1. Verify session, account, project and collection capability.
2. Load only supported currency/method combinations.
3. Collect amount, currency, reference/description and expiry under the approved contract.
4. Validate server-side and return safe field-level errors.
5. Show a review panel with exact material values.
6. Submit using the approved idempotency contract.
7. If the result is ambiguous, show Pending/Unknown with a reference and safe recovery action; do not blindly resubmit.
8. Show success only when authoritative evidence establishes the payment outcome; creating a request/link is not proof of payment.

### 9.2 View a transaction
1. Confirm resource access on the server.
2. Show canonical lifecycle state and freshness context.
3. Separate HARAI operation, provider attempt, webhook delivery and settlement/reconciliation facts.
4. Show linked refunds/reversals/payout returns as separate operations.
5. Offer refresh/enquiry only when supported and authorized.
6. For Unknown, provide the approved recovery path and suppress duplicate-action affordances.

### 9.3 Request and approve money-out
1. Hide actionable controls unless capability and user permission are enabled.
2. Load masked/authorized beneficiary details and authoritative funding/limit data only if the approved model defines it.
3. Validate amount/currency and show confirmed fees only.
4. Show review and require step-up/approval where policy demands.
5. Prevent requester self-approval when required.
6. On an ambiguous submission, show Pending/Unknown and do not initiate another transfer blindly.
7. Record status, actor, approvals and references through the approved audit contract.

### 9.4 Create and revoke API key
1. Verify project and key-management permission.
2. Ask for minimum scope, optional expiry and approved network restriction.
3. Explain consequences and review scope before creation.
4. Reveal the secret once only if the credential design supports it.
5. Copy feedback must not expose the secret in telemetry; afterward show only a non-secret identifier.
6. Revocation/rotation follows the approved policy and creates an audit event.

### 9.5 Internal approval
1. Verify internal role and assigned queue scope.
2. Load only data required for the decision.
3. Show requester, target, amount/currency or permission delta, evidence and permitted policy context.
4. Prevent self-approval and reject stale/already-decided requests.
5. Require a reason where configured and submit with the approved decision reference.
6. Display the authoritative result and audit reference.

## 10. Error and recovery mapping

| Failure class | Expected UI treatment | Prohibited behavior |
|---|---|---|
| Field validation | Field-level error and summary; preserve safe values | Generic success or silent mutation |
| Session expiry | Safe sign-in/recovery path; protect safe unsaved work where possible | Continue protected operations with stale session |
| Permission denial | Clear access message and permitted support path | Reveal another tenant's resource existence |
| Capability not enabled | Explain blocked capability and approved request/review path | Imply production approval or bypass through hidden call |
| Rate limit | Explain restriction and retry guidance from API contract | Rapid automatic retries |
| Provider definitive rejection | Normalized safe reason and operation reference | Treat every provider error as permanent or expose raw secrets |
| Timeout/ambiguous result | Pending/Unknown and authoritative enquiry/reconciliation path | Blind retry of money-moving operation |
| Duplicate/idempotent request | Reuse existing operation per approved contract | Create a second financial effect |
| Partial/stale data | Mark freshness and missing sections | Replace missing values with zero or success |
| Service unavailable | Distinguish HARAI availability from payment state | Declare payment failed solely because UI cannot load |
| Export failure | Preserve scope, show retry/reference, prevent partial sensitive export leakage | Publish permanent public download URL |
| Unexpected client error | Safe error reference and approved telemetry | Show stack traces, tokens or provider credentials |

Exact API error codes and retry semantics remain unconfirmed until contracts are approved.

## 11. Data loading and state ownership

- Server-returned status is authoritative; local optimistic state must never manufacture financial success.
- Cache keys and invalidation must include tenant/resource scope under the approved session model.
- Do not persist secrets, verification evidence or private financial data in browser storage by default.
- When access is revoked, clear protected client state under the approved revocation/session policy.
- Refresh must not bypass rate limits or create duplicate side effects.
- Mutations represent submitting, pending, unknown, definitive success and definitive failure distinctly.
- Background refresh must not reset input or unexpectedly move focus.
- Any offline behavior requires explicit design/review; private verification evidence and credentials are not cached offline by default.

## 12. Proposed acceptance and regression tests

IDs below are proposals only. Map them to stable DOC-31 requirements in DOC-34, then to concrete test files and execution evidence under DOC-45 after implementation is explicitly authorized.

| Proposed test ID | Acceptance criterion |
|---|---|
| FE-MAP-001 | Approved routes resolve to the intended screen and namespace |
| FE-NAV-002 | Navigation, breadcrumbs, active states and mobile menu work with keyboard and assistive technology |
| FE-ROLE-003 | UI reflects permission/capability state while direct API authorization remains authoritative |
| FE-TENANT-004 | Cross-tenant/guessed resource IDs reveal no protected data |
| FE-STATE-005 | Data-driven screens implement relevant loading, empty, stale, error and blocked states |
| FE-STATUS-006 | Pending/Unknown are never shown as definitive outcome without authoritative evidence |
| FE-COL-007 | Duplicate submissions and retry races cannot create duplicate collection effects |
| FE-REFUND-008 | Refund bounds, original-operation relationship and approval are enforced |
| FE-PAYOUT-009 | Money-out is denied when capability, permission, funding/limits or approvals are missing |
| FE-APPROVAL-010 | Self-approval and stale/duplicate decisions are rejected where policy forbids them |
| FE-KEY-011 | API secrets follow approved one-time reveal and are absent from URLs/logs/analytics |
| FE-WEBHOOK-012 | Webhook replay/configuration respects authorization, SSRF, signing, redaction and idempotency |
| FE-EXPORT-013 | Exports are scoped, permissioned, time-limited and audited |
| FE-SESSION-014 | Session/membership revocation blocks protected operations within approved policy |
| FE-A11Y-015 | Core journeys pass selected keyboard, screen-reader, contrast, zoom and reduced-motion checks |
| FE-RESP-016 | Core screens work at agreed viewport widths and 200% zoom |
| FE-ERROR-017 | API failures do not expose secrets, stack traces or cross-tenant existence |
| FE-TRACE-018 | Each accepted screen/component maps to requirement IDs and concrete evidence |
| FE-AUDIT-019 | Sensitive actions create required audit evidence without storing secrets |
| FE-FRESH-020 | Stale/partial data and unavailable service differ from zero values and transaction outcomes |
| FE-CAP-021 | Request, approval, provisioning and capability enablement are separate milestones |
| FE-DATA-022 | Synthetic fixtures contain no live credentials, real payer details or real transaction information |

Future test categories should include component/unit, route integration, API contract, authorization-negative, cross-tenant, concurrency/idempotency, timeout/recovery, accessibility, responsive, browser and regression tests. No tests are claimed as written or run by this document.

## 13. Screen acceptance checklist

Before each screen is accepted for implementation:
- [ ] Stable DOC-31 requirement IDs are listed.
- [ ] DOC-34 has proposed test ID, responsible role, dependency and required evidence.
- [ ] DOC-51 permissions/capability gates are documented.
- [ ] DOC-52 route and screen behavior are consistent.
- [ ] DOC-53 components, states and accessibility requirements are identified.
- [ ] Exact API contract/data fields and error mapping are approved; no speculative schema is hardcoded.
- [ ] Loading, empty, stale, pending/unknown, permission-denied and failure states are defined as applicable.
- [ ] Sensitive data is minimized and redacted.
- [ ] Audit, notifications, support and recovery are specified for consequential actions.
- [ ] Concrete test files/results are added only when implementation is authorized and execution occurs.
- [ ] Product, technical, security, finance/operations and compliance reviewers disposition relevant concerns.

## 14. Proposed implementation sequence

This is dependency-oriented planning, not a scheduled commitment.

**Stage A — Contract and policy readiness**
1. Resolve G1 requirement decisions and map screen acceptance IDs to DOC-31.
2. Complete G2 architecture/security review and close applicable DOC-47 findings.
3. Approve identity/session architecture, route policy and API contracts.
4. Confirm funds-flow, transaction state, ledger/reconciliation, provider and production-capability boundaries.
5. Approve design tokens, accessibility target and component API conventions.

**Stage B — Shared frontend foundation**
1. App shells, route/namespace boundaries and session/access-denied states.
2. Tokens, buttons, form fields, alerts, status components, tables and dialogs.
3. API client/error normalization and safe loading/empty/stale/error patterns.
4. Synthetic fixtures, accessibility checks and route-level smoke coverage.

**Stage C — Account and developer journeys**
1. Project list/create/detail and documentation entry points.
2. API-key list/create/reveal/revoke after credential policy approval.
3. Webhook configuration/inspection after endpoint/signature/replay policy approval.
4. Team, settings, notifications, support and audit views under approved permission boundaries.

**Stage D — Financial journeys**
1. Transaction list/detail and lifecycle panels.
2. Collections and payment-link UI.
3. Refund workflows.
4. Money-out, beneficiaries, approval queues and reconciliation only after funds-flow and financial controls are approved.
5. Production eligibility/activation backed by recorded authority and evidence.

**Stage E — Hardening and release readiness**
1. Requirement-to-test coverage and negative authorization/cross-tenant tests.
2. Responsive/accessibility/browser regression.
3. Failure injection, concurrency/idempotency and ambiguous-outcome recovery.
4. Security/privacy review, operational monitoring and release evidence.
5. Explicit release decision under DOC-23 after all gates are satisfied.

Do not skip a stage merely because a screen can be rendered without its underlying security or financial contract.

## 15. Open decisions

1. Approved frontend stack, router, API client and state/cache strategy.
2. Final route names and merchant/internal namespace boundaries.
3. Identity/session/MFA and revocation behavior.
4. API contracts and normalized error codes for each screen family.
5. Final permission names, role delegation ceilings and maker-checker rules.
6. Funds-flow model, balance/availability definitions, provider capabilities and commercial permissions.
7. Design tokens, component props, accessibility target and supported browsers.
8. Data retention, export expiration, notification privacy and audit visibility.
9. Named product, frontend, technical, security, finance, operations and compliance reviewers.
10. Mapping from proposed FE test IDs to DOC-31 requirements and later concrete test evidence.

## 16. Current disposition

DOC-54 is a route/component/data/action/state planning blueprint connecting DOC-51, DOC-52 and DOC-53. It does not define final API schemas, prove permission correctness, provide a clickable prototype or constitute implementation/test evidence. G1 remains NOT PASSED; G2 remains NOT APPROVED. No coding, tests, provider contact, live credentials, real transactions, merge or deployment are authorized by this document.
