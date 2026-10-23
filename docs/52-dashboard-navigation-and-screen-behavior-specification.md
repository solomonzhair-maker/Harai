# HARAI — Dashboard Navigation and Screen-Behavior Specification

**Status:** Draft UX/interaction specification — not implemented or approved for implementation  
**Date:** 2026-10-23  
**Related documents:** DOC-03, DOC-31, DOC-34, DOC-43, DOC-49, DOC-50, DOC-51  
**Purpose:** Translate the role and dashboard proposal into screen-level navigation, behaviors, validation, state handling and acceptance criteria.

> This is a product specification, not a running prototype or implementation authorization. Production-first onboarding removes mandatory sandbox usage; it does not remove provider authorization, verification, compliance, finance, security or release gates.

## 1. Experience goals

- Give each business a clear overview of its account, enabled capabilities and next required action.
- Let developers reach API documentation, create a project and request production assessment without first completing a sandbox flow.
- Separate business verification, technical integration readiness and production capability activation so that one cannot be mistaken for another.
- Keep financial status truthful: pending and unknown are not success or failure.
- Make permissions understandable without relying on hidden buttons for security.
- Maintain a consistent responsive shell, accessible interactions and predictable error/recovery behavior.
- Avoid asking merchants for provider API credentials unless a separately approved integration model explicitly requires it.

## 2. Global application shell

### 2.1 Merchant navigation

Primary sidebar, grouped in this order:
1. **Overview** — Home
2. **Payments** — Transactions, Collections / Payment Links, Refunds, Money-out (only when permitted), Reconciliation
3. **Developers** — Projects, API documentation, API keys, Webhooks, Integration diagnostics
4. **Business** — Verification, Team & Roles, Reports, Audit activity, Settings
5. **Help** — Notifications, Service status, Support cases

Navigation is capability- and permission-aware. If a user lacks access, do not show sensitive details. For a capability the owner can request, show a neutral disabled/request state with the reason and permitted next step. Never expose an internal permission name as the only explanation.

### 2.2 Global header

- Active business selector when the user belongs to multiple tenants.
- Current environment/capability context, without implying sandbox is mandatory.
- Global search only across resources the user may access.
- Notifications, help/support, profile and sign-out.
- A persistent service-status indicator linking to relevant incident details.
- On small screens, collapse the sidebar into an accessible navigation drawer; preserve active location and keyboard focus.

### 2.3 Shared interaction rules

- Breadcrumbs identify current location without replacing the page title.
- Primary action is singular and visually distinct; destructive actions are not styled as primary.
- Every page defines loading, empty, populated, stale/partial, error, permission-denied and suspended/blocked states as applicable.
- Preserve entered non-sensitive form values after recoverable validation/network errors. Do not persist secrets or sensitive identity data in browser storage.
- Use confirmation dialogs for destructive or high-impact actions, explaining consequences and whether the action is reversible.
- Every asynchronous financial operation displays its own status and reference. Never infer finality from a successful HTTP response alone.
- Dates/times use a clearly stated timezone; amounts always show currency and formatting policy.
- Avoid color-only status communication. Include text and accessible announcements.
- Support mobile, keyboard-only use, visible focus, semantic labels and screen-reader status updates.

## 3. Navigation and route map

Proposed route names are illustrative and must be aligned with the final application router. Server authorization remains mandatory on every route and API.

| Area | Proposed route | Default landing behavior |
|---|---|---|
| Home | `/dashboard` | Summary cards, latest activity, capability state and required actions |
| Verification | `/business/verification` | Verification progress and evidence tasks |
| Transactions | `/payments/transactions` | Filterable tenant-scoped transaction list |
| Transaction detail | `/payments/transactions/:id` | State history, provider reference where safe, linked operations |
| Collections | `/payments/collections` | Create/view supported collection requests |
| Payment links | `/payments/links` | List, create, expire or disable eligible links |
| Refunds | `/payments/refunds` | Eligible refunds and operation history |
| Money-out | `/payments/payouts` | Available only when the merchant has the relevant approved capability |
| Reconciliation | `/finance/reconciliation` | Settlement evidence, unmatched items and discrepancy cases |
| Projects | `/developers/projects` | Project list, create project, environment/capability status |
| Project detail | `/developers/projects/:id` | API instructions, project settings and safe diagnostics |
| API keys | `/developers/api-keys` | Key metadata and create/revoke/rotate flows |
| Webhooks | `/developers/webhooks` | Endpoint configuration, delivery history and secret rotation |
| Team | `/business/team` | Members, invitations and permitted role changes |
| Reports | `/business/reports` | Permissioned report catalogue and exports |
| Audit activity | `/business/audit` | Read-only, scoped account/security events |
| Settings | `/business/settings` | Profile, preferences and allowed business settings |
| Notifications | `/notifications` | Read/unread notifications with destination authorization |
| Support | `/support/cases` | Own/authorized support cases |
| Internal console | Separate protected namespace | Only for explicitly authorized HARAI staff roles |

A guessed, changed or copied resource ID must never grant access. Unknown and unauthorized resource behavior should avoid disclosing another tenant's resource existence.

## 4. Screen specifications

### 4.1 Home dashboard

**Purpose:** Answer “Can I use HARAI, what is active, and what should I do next?”

**Content**
- Account status: setup required, verification in progress, action required, active with limitations, suspended or closed.
- Capability tiles for collections, money-out, refunds, cards and any future method. Each tile shows Not requested, In review, Enabled, Limited, Suspended or Unavailable as appropriate.
- Collections/transaction summary for a selected period, only when data exists and the user has permission.
- Recent activity with clear status labels and links to authorized details.
- Developer integration status: project exists, keys configured, webhook health if available, recent delivery issues.
- Action list with the most important pending owner tasks.

**Actions:** Complete verification, create project, open docs, request a capability, view transactions, open a support case.

**Rules:** Do not display fake metrics in production. Mark stale/partial data. A pending payment count must not be described as successful revenue. A card's action must be authorized server-side.

### 4.2 Sign-in, recovery and session expiry

**Content:** Sign-in, recovery, MFA/step-up prompts as selected by the approved identity architecture.

**Behavior**
- Rate-limit attempts and use neutral recovery responses that do not reveal account existence.
- Expire and revoke sessions under the approved policy.
- After sign-in, return users only to a safe, authorized destination.
- If access is revoked during a session, stop protected operations and explain that access changed without revealing sensitive details.
- High-impact actions may require fresh authentication; exact factors remain an open decision.

### 4.3 Business setup and verification

**Sections:** Business profile, contact/representative details, required evidence checklist, review status, requests for more information, decision history.

**States:** Draft, Submitted, Action required, Under review, Approved, Rejected, Suspended, Expired.

**Behavior**
- Show only evidence fields required by the approved policy and jurisdiction.
- Explain why an item is needed and acceptable formats without exposing internal risk rules that could enable abuse.
- Confirm submission and provide a case/reference ID.
- Corrections create a traceable new submission/version; prior decisions remain auditable.
- No “approved” state unless an authorized decision is recorded.
- Production activation is separately displayed from business verification.

### 4.4 Transactions list and detail

**List controls:** Date range, status, method, operation type and safe reference search; pagination and export only when authorized.

**Detail sections:** HARAI reference, amount/currency, operation type, merchant-visible lifecycle, timestamps, linked operations, provider reference where safe, event history and available next actions.

**Behavior**
- Distinguish Created, Requires action, Submitting, Pending, Unknown, Succeeded, Failed, Cancelled and Expired according to the approved canonical state machine.
- Explain that Unknown means outcome needs resolution; it is not a definitive failure.
- Keep refund, reversal, payout return and original collection as distinct linked operations.
- Do not expose provider secrets, unnecessary payer data or another tenant's references.
- Refresh/status enquiry is an authorized recovery operation and must be rate-limited and idempotent.

### 4.5 Collections and payment links

**Create form:** amount/currency (only supported combinations), internal reference, description, expiry and optional metadata allowed by policy.

**Behavior**
- Validate amount, currency, length, expiry and merchant capability before submission.
- Show a review step for material details before creating a request.
- Do not call a collection successful merely because a request was accepted.
- Display link status and expiry; avoid exposing personal information in shareable URLs.
- Expiring/disabling a link does not rewrite historical payment records.
- Prevent duplicate side effects under double-click, network retry and concurrent submission through approved idempotency rules.
- Only list payment methods enabled for that merchant and transaction.

### 4.6 Refunds

**List/detail:** original transaction, eligible amount, already-refunded amount, requested amount, reason, status and provider evidence where safe.

**Behavior**
- Clearly separate refund from cancellation, reversal, payout return or dispute.
- Prevent refund totals from exceeding eligible original amounts using server-side atomic checks.
- Require the configured approval workflow for the requested amount/risk class.
- If provider support or original transaction eligibility is unknown, block the action and explain that it needs review.
- A timeout produces Pending/Unknown and recovery, not an automatic second refund.

### 4.7 Money-out and beneficiaries

**Visibility:** Only render actionable money-out features when the merchant's capability and user permission are both enabled. Otherwise show no sensitive beneficiary or payout data.

**Beneficiary flow:** Add/update destination, validate fields, show masked destination, apply step-up and risk review, notify authorized contacts and apply approval/cooling-off policy where approved.

**Payout flow:** Amount, currency, beneficiary, purpose/reference, fee disclosure if confirmed, funding/availability state, review screen, maker-checker approval if required, final submission status.

**Rules**
- Do not show “available balance” unless the approved funds-flow/ledger model defines it and the data is authoritative.
- Never claim settlement time or fee without approved terms.
- Enforce limits and idempotency server-side.
- Protect beneficiary changes from replay and privilege escalation.
- Unknown outcomes must be resolved by enquiry/reconciliation, not blind retry.

### 4.8 Reconciliation

**Content:** reporting period, provider/source, totals, last successful import, unmatched transactions, settlement differences and assigned discrepancy cases.

**Case behavior:** assign owner, add evidence/reference, classify discrepancy, propose a correction and route for separate approval if required.

**Rules**
- Missing reports or delayed provider data must be visibly marked.
- Never silently discard unmatched items or rewrite posted history.
- Corrective action is a linked compensating operation under the approved ledger design.
- Exports require permission, bounded filters and audit logging.
- Do not describe a provider transaction total as a bank settlement total unless evidence supports the relationship.

### 4.9 Developer projects

**Project list:** project name, created date, active state, assigned capabilities, key metadata (never key secret), webhook health and integration status.

**Create project:** name, purpose, optional callback/event preferences and terms acknowledgements where approved.

**After creation:** show HARAI API documentation, API base URLs approved for the current service, authentication guide, request examples, versioning and support path. Environment labels must be accurate.

**Production-first path:** the developer may create a project and request direct production assessment without using sandbox. The screen must clearly distinguish:
- Project created
- Business verification status
- Production-access request submitted
- Provider/processor permission confirmed
- Capability approved
- Production credentials provisioned
- Capability enabled

These are separate milestones. Never collapse them into a single “Live” badge until every applicable gate has passed.

### 4.10 API keys

**List:** safe label, prefix/identifier, scopes, project, created/last-used time, expiry/rotation status and revocation status.

**Create flow:** select project, name, minimum scopes, optional expiry and any approved network restrictions; review scope; confirm.

**Secret display:** show the full secret only once if the credential design supports that model. Explain safe server-side storage. Provide copy action and confirmation; never include the secret in URL, analytics, logs or subsequent list responses.

**Rotate/revoke:** explain impact, confirmation and overlap policy if supported. Record actor/reason and notify authorized owners. IP allowlisting must rely on trusted network configuration; do not trust client-supplied forwarded headers.

### 4.11 Webhooks

**Configure:** HTTPS endpoint, subscribed event types, optional delivery metadata and verification instructions.

**Detail:** masked signing-secret status, endpoint health, recent delivery outcomes, retry state and request/event IDs.

**Behavior**
- Validate endpoint and destination against approved SSRF controls.
- Secret rotation follows an approved transition plan; secret value is not retrievable after initial display.
- Delivery replay is permissioned, rate-limited and does not duplicate financial effects.
- Show delivery outcome separately from business transaction outcome.
- Document event ordering, signature verification, timestamp tolerance, idempotency and recovery behavior.
- Never provide a control that lets users bypass signatures or tenant routing checks.

### 4.12 Team and roles

**Content:** member, status, assigned role, invitation status, last activity where permitted and available actions.

**Behavior**
- Owner/admin may grant only roles within their delegation ceiling.
- Prevent removal/demotion of the last eligible owner without a verified ownership-transfer flow.
- Invite links are short-lived, single-use and bound to the intended process.
- Role changes trigger notification and audit.
- Revocation must promptly affect API and UI authorization, including existing sessions and background tasks.

### 4.13 Reports, exports and audit activity

**Reports:** clear definitions, period, timezone, currency, filters, data freshness and permission requirements.

**Exports:** validate filters, create a short-lived download, log requester/scope/time, prevent cross-tenant access and expire links. Avoid sending sensitive exports to public URLs or browser storage.

**Audit view:** read-only event timeline with actor, action, result, timestamp, target and correlation/reference. Mask secrets and unrelated personal data; users cannot edit audit records.

### 4.14 Notifications and support

**Notifications:** read/unread state, severity, date and authorized destination. Mark-read operations must not cause unintended navigation or access.

**Support cases:** category, description, non-sensitive attachment rules, related HARAI reference, priority, status, assigned team and message history.

**Behavior**
- Warn users not to submit passwords, OTPs, API secrets or full payment credentials.
- Support staff see only case-authorized data; elevated access requires reason, limited scope, time limit and audit.
- Notifications must not reveal sensitive payment/business details on shared devices or lock screens unless an approved preference permits it.

### 4.15 Internal HARAI console

Separate navigation and authorization from merchant routes. Proposed sections:
- Merchant verification queue
- Capability/production eligibility
- Provider adapter health and unresolved operations
- Reconciliation discrepancies
- Support cases and scoped access requests
- Security/abuse alerts
- Approval queue
- Audit/review evidence
- Release and incident status

Every consequential internal action requires explicit permission, reason and audit event. The person who proposes a sensitive financial correction must not approve their own correction where separation is required. Internal staff do not gain blanket access to secrets or full merchant data.

## 5. Forms, validation and confirmation patterns

For every form:
- Identify required/optional fields and why the data is needed.
- Validate on the server; client validation is only a usability aid.
- Preserve safe non-sensitive values on recoverable errors.
- Show field-level errors and a summary for assistive technology.
- Disable duplicate submission while a request is in flight, but rely on server idempotency for correctness.
- Provide a clear confirmation view for high-impact operations.
- Do not silently change amount, currency, beneficiary or permissions after user review.
- If the server returns an ambiguous result, show Pending/Unknown with a recovery path and reference rather than asking the user to repeat the action immediately.

## 6. Status vocabulary and copy rules

Use one shared status dictionary across dashboard, API docs and notifications:
- **Not requested:** no application submitted.
- **In review:** an authorized review is in progress.
- **Action required:** the user must supply information or take a stated step.
- **Enabled:** approved for the named capability and scope.
- **Limited:** only a defined subset is available; show the actual limitation.
- **Suspended:** temporarily unavailable due to an authorized decision or control.
- **Unavailable:** not supported or not eligible under current confirmed policy.
- **Pending:** a process is ongoing.
- **Unknown:** outcome is unresolved and needs authoritative recovery.
- **Succeeded / Failed:** use only when authoritative lifecycle evidence establishes the result.
- **Expired / Cancelled:** use only according to the approved operation lifecycle.

Never use “verified,” “approved,” “production-ready,” “settled” or “paid out” as marketing labels without the corresponding recorded evidence.

## 7. Accessibility and responsive requirements

- All actions are keyboard accessible with visible focus.
- Menus, dialogs and drawers manage focus correctly and return it on close.
- Form fields have programmatic labels, instructions and associated errors.
- Dynamic status changes are announced without overwhelming assistive technology.
- Charts and status indicators have text equivalents.
- Color is never the only status cue.
- Touch targets and spacing work on narrow screens.
- Tables have mobile alternatives or responsive overflow with meaningful headers.
- Timeouts, session expiry and errors provide a clear next action.
- Support reduced-motion preferences and avoid unnecessary animation.
- Final conformance target and supported browsers remain to be selected and reviewed.

## 8. Acceptance criteria and proposed tests

All IDs below are planned test identifiers, not evidence of implemented or executed tests. They must be mapped to stable DOC-31 requirement IDs and actual test files when implementation is authorized.

| Test ID | Acceptance criterion |
|---|---|
| UX-NAV-001 | Navigation and active states are consistent across all merchant routes and screen sizes |
| UX-ROLE-002 | Every screen/action is both UI-gated and server-authorized for the current user, tenant and capability |
| UX-TENANT-003 | Changing route/resource identifiers cannot expose another tenant's data |
| UX-HOME-004 | Dashboard distinguishes verification, capability activation and integration readiness |
| UX-PROD-005 | Project creation and direct production assessment are available without mandatory sandbox use |
| UX-PROD-006 | Production capability remains unavailable until all applicable approval gates are recorded |
| UX-TXN-007 | Pending and unknown outcomes are clearly distinguished from success/failure |
| UX-COL-008 | Duplicate submissions cannot create duplicate financial side effects |
| UX-REFUND-009 | Refund eligibility, amount bounds and approval policy are enforced atomically |
| UX-PAYOUT-010 | Money-out UI and APIs deny access when capability, role, limits or approvals are missing |
| UX-RECON-011 | Unmatched items remain visible until an authorized disposition is recorded |
| UX-KEY-012 | Full API secret is not retrievable after creation; secrets are absent from logs and URLs |
| UX-WEBHOOK-013 | Webhook secret rotation, endpoint validation and replay controls follow approved policy |
| UX-TEAM-014 | Role ceilings and last-owner protections cannot be bypassed |
| UX-EXPORT-015 | Exports are permissioned, tenant-scoped, audited and time-limited |
| UX-SUPPORT-016 | Internal elevated support access is case-bound, limited and audited |
| UX-A11Y-017 | Core journeys pass the selected keyboard, screen-reader and responsive criteria |
| UX-STATE-018 | Loading, empty, stale, error, permission-denied and suspended states are implemented and truthful |
| UX-AUDIT-019 | Sensitive actions create complete audit records without storing secrets |
| UX-SESSION-020 | Revoked membership or session cannot continue protected operations past the approved revocation policy |

Tests should include mobile viewport sizes, keyboard-only navigation, screen-reader checks, direct API calls, tenant switching, stale sessions, concurrent submissions, timeouts, background jobs, retries, export links, and service recovery. Use synthetic data only.

## 9. Required screen delivery checklist

Before a screen is accepted as implemented:
- [ ] Route and role/capability policy documented.
- [ ] Loading, empty, populated, stale, error and blocked states covered as relevant.
- [ ] Client and server validation defined.
- [ ] Destructive/high-impact action confirmation and recovery behavior defined.
- [ ] Audit and notification requirements recorded.
- [ ] Mobile, keyboard and screen-reader behavior reviewed.
- [ ] Negative authorization and cross-tenant tests mapped.
- [ ] Data fields minimized and secrets masked.
- [ ] Requirement IDs and concrete test evidence linked.
- [ ] Product and accountable technical/security/finance reviewers have dispositioned applicable concerns.

## 10. Open decisions

1. Final visual design system, navigation density, iconography and dashboard layout.
2. Identity provider, MFA/step-up approach and session policy.
3. Final router/API architecture and permission enforcement design.
4. Exact production eligibility workflow and named decision owners.
5. Approved provider capability and commercial terms for each payment method.
6. Funds-flow/ledger definitions for balances, settlement and money-out.
7. Refund and payout thresholds, approval quorum, limits and cooling-off policy.
8. Data retention, export expiration, supported browsers and accessibility conformance target.
9. Notification channels and privacy settings.
10. Named QA, product, technical, security, finance and compliance reviewers.

## 11. Current disposition

DOC-52 is a detailed screen-level proposal linked to DOC-51's role and permission model. It is not a clickable prototype or implemented UI. G1 remains NOT PASSED, G2 remains NOT APPROVED, the proposed tests have not been run, and creating this document does not authorize coding, provider contact, live credentials, transactions, merge or deployment.
