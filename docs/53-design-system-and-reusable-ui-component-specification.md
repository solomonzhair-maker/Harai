# HARAI — Design System and Reusable UI Component Specification

**Document ID:** DOC-53  
**Status:** Draft proposal — design tokens and component behavior require product/design/accessibility review  
**Date:** 2026-10-23  
**Depends on:** DOC-31, DOC-34, DOC-43, DOC-45, DOC-51 and DOC-52  
**Implementation gate:** This specification does not authorize application coding, a Figma build, payment-engine implementation, tests, merge or deployment.

## 1. Purpose and scope

Define a consistent, accessible, premium enterprise interface for HARAI across merchant and internal staff experiences. This is a reusable design-system contract: it describes visual principles, token candidates, component responsibilities, states, behavior, accessibility and acceptance criteria so a future implementation can be consistent rather than screen-by-screen improvised.

The baseline direction is a dark navy navigation sidebar, bright workspace, restrained blue accent, clear information hierarchy, professional tables and explicit status indicators. HARAI must use original branding and its own component implementation; JulyPay is only a workflow reference.

This is a proposed specification, not a rendered mock-up or clickable prototype. No colors, tokens, component API, accessibility target or implementation technology are approved by this document alone.

## 2. Design principles

1. **Clarity before decoration:** prioritize task, status, amount, currency, owner and next action.
2. **Truthful financial UI:** never imply that a request, callback, HTTP response or visual animation proves payment success or settlement.
3. **Permission-aware by design:** hide irrelevant actions for usability, but enforce authorization on the server. UI visibility is never the security boundary.
4. **Consistent status language:** one shared status dictionary across dashboard, transaction details, notifications and developer tools.
5. **Progressive disclosure:** show the essential facts first and put advanced metadata, raw events and technical details behind clearly labelled disclosure controls.
6. **Accessible by default:** keyboard, focus, semantic labels, contrast, screen-reader announcements and reduced-motion behavior are component requirements.
7. **Responsive, not merely compressed:** mobile layouts change interaction patterns when tables, navigation or multi-column forms no longer fit.
8. **Recoverable interaction:** preserve safe form inputs after recoverable failures; provide a clear next step for stale, pending, unknown and failed outcomes.
9. **Sensitive-data minimization:** mask keys, signing secrets, beneficiary details and unnecessary personal data; never expose secrets in tooltips, analytics or URLs.
10. **Predictable controls:** the same component has the same meaning, behavior and focus handling wherever it appears.

## 3. Proposed visual language and tokens

The following values are initial candidates for design review, not final brand decisions. Confirm them with contrast testing and a visual review before implementation.

### 3.1 Color roles

| Token role | Proposed starting point | Usage and constraints |
|---|---|---|
| Navigation background | Deep navy, e.g. #101B33 | Sidebar and selected navigation context; verify text/icon contrast |
| Workspace background | Near-white, e.g. #F7F9FC | Page canvas and large neutral surfaces |
| Surface | White, e.g. #FFFFFF | Cards, tables, forms and dialogs |
| Primary accent | Accessible medium blue, e.g. #2457D6 | Primary actions, links, focus accents; test every foreground/background pairing |
| Primary text | Deep neutral, e.g. #172033 | Headings and body text |
| Secondary text | Muted neutral, e.g. #5C667A | Supporting information; must still meet contrast requirements |
| Border | Light neutral, e.g. #DCE2EC | Dividers, field borders and table boundaries |
| Success | Dark green foreground on pale green surface | Confirmed positive state only; always include text/icon |
| Warning | Dark amber foreground on pale amber surface | Attention or pending review; always include text/icon |
| Danger | Dark red foreground on pale red surface | Destructive action, failure or suspension where semantically correct |
| Information | Dark blue foreground on pale blue surface | Informational message, not proof of approval |
| Disabled | Neutral foreground and surface | Disabled controls remain understandable; do not rely on opacity alone |

Do not use a color alone to communicate a status. Do not assign green to “pending” or “unknown” merely to make the interface look reassuring. Theme variants and dark-mode support remain open decisions.

### 3.2 Typography

Proposed type hierarchy; final font family is open.

| Role | Intended use | Proposed behavior |
|---|---|---|
| Page title | One primary title per screen | Clear, compact, responsive; not oversized |
| Section heading | Major card/page section | Consistent level hierarchy |
| Body | Main explanatory content | Comfortable reading size and line height |
| Label | Form labels and metadata labels | Persistent, explicit labels; never placeholder-only |
| Numeric emphasis | Amounts, totals and key metrics | Tabular numerals where supported; include currency and period |
| Caption | Help text, timestamps and secondary metadata | Remains readable at zoom and mobile sizes |
| Code | API keys prefixes, references and snippets | Monospaced, wrap/scroll safely; mask secrets |

Avoid communicating a metric through font size alone. Always label metric value, currency/unit, time window and data freshness where applicable.

### 3.3 Spacing, layout and shape

- Use a consistent spacing scale based on a small repeatable unit; proposed increments: 4, 8, 12, 16, 24, 32 and 40 px.
- Use a consistent content grid and alignment; avoid arbitrary one-off margins.
- Cards and dialogs use restrained corner radii and subtle elevation; do not use shadows as the only boundary cue.
- Inputs, buttons and interactive targets have consistent heights and hit areas.
- Keep dense data tables compact but readable, with clear row separation and sufficient line height.
- Do not force desktop multi-column layouts onto narrow screens.
- Exact breakpoints, maximum content width, radii, elevations and density modes require design review.

## 4. Application shell and navigation components

### 4.1 Application shell

**Responsibilities:** provide the navigation region, top bar, page title/breadcrumb area, content container and global feedback region.

**Required behavior**
- Distinguish merchant workspace from the protected internal HARAI console.
- Show current account/project context when multiple contexts exist.
- Keep navigation state synchronized with the current route.
- Provide a clear route title and optional breadcrumbs for nested detail pages.
- Preserve safe content and focus when the navigation drawer opens/closes.
- Do not show sensitive account data in shared-device notifications or global chrome without an approved policy.

### 4.2 Sidebar and mobile navigation

**Variants:** merchant sidebar, internal-console sidebar, compact/collapsed sidebar; mobile drawer or bottom navigation only if user testing supports it.

**States:** expanded, collapsed, active route, focus, hover, unavailable destination, mobile open/closed.

**Rules**
- Active route is indicated by more than color (e.g. shape, icon treatment or text weight).
- Navigation is derived from permissions/capabilities, but direct-route authorization remains server-enforced.
- Group headings and nested items are semantically structured.
- On mobile, opening the drawer traps focus appropriately, exposes a close control and returns focus to the trigger on close.
- No inaccessible hover-only menus.
- Do not place high-risk financial actions in persistent global navigation as one-click operations.

### 4.3 Breadcrumbs and page header

- Breadcrumbs represent actual hierarchy and are keyboard navigable.
- The current page is identified as current, not as a misleading link.
- Page headers support title, short explanation, contextual actions and optional status.
- Avoid duplicate primary actions in the page header and body unless the duplication has a clear usability rationale.

## 5. Action components

### 5.1 Buttons and links

**Button variants:** primary, secondary, tertiary/quiet, destructive, and icon-only with an accessible name.

**Required states:** default, hover, focus-visible, pressed, loading, disabled and permission-blocked where useful.

**Rules**
- One visually dominant primary action per coherent action group where possible.
- Destructive buttons use explicit labels such as “Revoke key” or “Submit refund request”; avoid ambiguous “OK” or “Continue” for irreversible operations.
- Loading state prevents duplicate UI submission but must not replace server idempotency.
- Disabled state has a reason available in nearby explanatory text or an accessible description.
- Links navigate; buttons perform actions. Do not style a link as a button while giving it conflicting semantics.
- Icon-only controls require accessible names and visible focus.
- External links are identifiable and do not silently discard unsaved changes.

### 5.2 Menus and action menus

- Use menus for secondary actions, not as the only place to find a critical next step.
- Support keyboard navigation, escape-to-close and predictable focus return.
- Separate destructive actions from routine actions.
- Do not render actions the user cannot perform, but keep the server-side permission check authoritative.
- Menu item text states the target/action clearly, particularly for transaction and API-key operations.

## 6. Form components

### 6.1 Text input, textarea, select, date/time and amount fields

Every field must support a persistent label, optional/required indicator, help text, error text, disabled/read-only states and programmatic association between label, hint and error.

- Placeholder text is an example, not a substitute for a label.
- Do not silently trim or transform meaningful values without explaining the rule.
- Validate on the server; client-side validation is a convenience only.
- Preserve safe inputs when a recoverable request fails.
- Show date, time zone, decimal precision and currency rules explicitly.
- Amount controls use exact decimal/integer-safe representations; do not imply floating-point arithmetic.
- Unsupported currency/method combinations must not be selectable.
- Errors should explain how to correct the field without revealing sensitive risk rules.

### 6.2 Form layout and error summary

- Group related fields under meaningful headings.
- Put errors near the relevant field and provide a summary for long forms.
- On submission failure, focus or announce the error summary appropriately.
- Avoid clearing a complete form after an unrelated network error.
- Clearly distinguish validation errors, permission denials, provider rejection and ambiguous/unknown outcomes.
- For high-impact changes, show a review step that summarizes the exact target, amount/currency or permission change before submission.

### 6.3 Sensitive input patterns

- API secrets and webhook signing secrets are write-only after initial display unless an approved credential design explicitly says otherwise.
- Passwords, OTPs, full card credentials and provider credentials must not be requested in support forms.
- IP allowlist entries need format help, safe removal/recovery behavior and trusted-proxy rules explained in the relevant technical documentation.
- Beneficiary/destination details should be masked after entry wherever practical.
- Copy controls must not write secrets to logs, analytics or browser-visible URLs.

## 7. Data display components

### 7.1 Cards and metric tiles

- Every metric has a meaningful label, unit/currency, time range and freshness context.
- Show loading, empty, partial/stale, error and populated states.
- Do not fabricate data for visual balance.
- Use comparisons only when periods and definitions are comparable.
- Never label transaction request totals as settled funds unless the approved funds-flow model and authoritative evidence support it.
- A card is not clickable unless the whole card has a clear, accessible action.

### 7.2 Tables, filtering and pagination

**Required features as appropriate:** semantic column headers, sorting with announced state, filter controls, pagination, result count, loading/empty/error state, row selection where authorized and export only where permissioned.

- Keep important identifying fields visible on narrow layouts; use a labelled responsive detail view or accessible horizontal overflow when necessary.
- Do not rely on row color to communicate status.
- Provide stable row actions and prevent a row click from conflicting with buttons inside the row.
- Filters must be reflected in the displayed result context and included in export scope.
- Empty results distinguish “no records exist” from “no records match these filters.”
- Pagination and filtering must not leak cross-tenant information through counts, suggestions or cached results.
- Bulk actions require a clear selected count, scope and confirmation for high-impact effects.

### 7.3 Status badge and status panel

Use a shared semantic status component with a visible text label and optional icon. Color is supplementary.

It must support at minimum the documented product vocabulary where applicable: Not requested, In review, Action required, Enabled, Limited, Suspended, Unavailable, Pending, Unknown, Succeeded, Failed, Cancelled and Expired.

- Status must be derived from authoritative application state, not guessed from client timing.
- “Unknown” must explain that the outcome remains unresolved and show the safe recovery path.
- “Enabled” must identify the capability/scope if ambiguity is possible.
- “Settled,” “paid out,” “verified” and “approved” require their corresponding evidence and authorized record.
- Status history should be available for consequential lifecycle changes where permissions allow.

### 7.4 Tabs, accordions and disclosure panels

- Use tabs only for peer sections with clear labels and predictable keyboard behavior.
- Use accordions for optional supporting detail, not to hide required warnings or the next required action.
- Preserve open/selected state when safe and useful.
- Announce expanded/collapsed state programmatically.
- Sensitive raw payloads and provider diagnostics require explicit permission and redaction.

### 7.5 Timeline and event history

Display timestamp with timezone, event label, actor/source where safe, result, and correlation/reference identifier. Distinguish business transaction events, provider attempt events, webhook delivery events and administrative audit events. Do not imply that receiving a webhook proves the underlying payment succeeded.

### 7.6 Amount, currency and percentage display

- Always show currency beside material monetary amounts.
- Use approved locale-aware formatting and consistent minor-unit precision.
- Negative amounts, fees, gross/net totals and reversals must be labelled unambiguously.
- Avoid summing unlike currencies.
- Show time window and definition for rates or percentages.
- If a value is unavailable or stale, say so rather than rendering zero.

## 8. Feedback, overlays and system states

### 8.1 Inline alert and toast

**Alert variants:** information, success, warning and error. Use success only after authoritative confirmation of the action being reported.

- Important errors and unresolved financial outcomes must persist in the relevant page; a transient toast alone is insufficient.
- Toasts are for short-lived, non-critical feedback and must be announced accessibly.
- Do not expose secrets, full provider payloads or unnecessary personal data in notifications.
- Provide a clear next step when user action is required.

### 8.2 Dialogs and drawers

- Use a dialog for focused decisions and a drawer for supplementary context or task flows that benefit from retaining the underlying page.
- Provide accessible title and description, close behavior, focus management and predictable focus return.
- Escape may close a non-destructive informational overlay; unsaved/high-impact workflows need an explicit safe exit policy.
- Confirmations state the exact action and target, material amount/currency if relevant, and whether the action can be reversed.
- Never use a confirmation dialog as a substitute for server authorization, step-up authentication or maker-checker approval.
- Avoid stacked modal dialogs.

### 8.3 Loading, skeleton, empty, stale, error and offline states

Every data-driven screen/component must define relevant states:
- **Loading:** communicate what is loading; avoid layout shifts where practical.
- **Skeleton:** only when it helps predict layout; respect reduced motion.
- **Empty:** explain why the state is empty and offer a permitted next action.
- **Stale/partial:** identify that the data may not be current and provide a safe refresh path.
- **Error:** explain what failed, whether user input was preserved, and what can be done next.
- **Pending/unknown:** show an operation reference and recovery route; do not invite immediate duplicate submission.
- **Permission denied:** explain access is unavailable without disclosing protected resource existence.
- **Offline/unavailable service:** distinguish local connectivity from server/provider status. Do not cache private verification evidence or secrets for offline display by default.

## 9. HARAI-specific reusable components

### 9.1 Capability card

Fields: capability name, current evidence-backed state, allowed scope, outstanding requirement and safe next action. Must distinguish “not requested,” “under review,” “approved but not provisioned,” “provisioned,” and “enabled” where those are separate lifecycle steps.

### 9.2 Transaction lifecycle panel

Show canonical state, operation type, amount/currency, timestamps, reference, linked operations and recovery guidance. Keep PaymentIntent, provider attempt, refund, reversal, payout and payout-return relationships distinct according to the approved domain model.

### 9.3 Approval queue item

Show requested action, requester, target, amount/currency or permission delta, policy/risk context permitted to the reviewer, deadline/age and available decision actions. Prevent self-approval where separation of duties applies; require reason and audit for approve/reject/return-for-information actions.

### 9.4 API key creation and reveal panel

Show scope summary, project, optional expiry/network restrictions and consequences before creation. Reveal a secret only once when supported; provide copy action, explicit storage warning and a non-secret identifier after the reveal closes. Do not put the secret in DOM attributes beyond what is required for display/copy, logs, analytics or navigation state.

### 9.5 Webhook delivery inspector

Show event ID, endpoint label, attempt number, timestamp, response classification, retry status and safe redacted payload preview. Distinguish delivery result from payment result. Replay must be authorized, rate-limited, audited and safe against duplicate financial effects.

### 9.6 Reconciliation discrepancy panel

Show source, period, amount/currency, classification, age, assigned owner, evidence references and proposed disposition. Corrections are linked, reviewed actions; the UI must not offer silent editing of historical financial records.

### 9.7 Audit event viewer

Read-only timeline with actor, action, target, timestamp, result and correlation ID. Mask secrets and unrelated personal information. Provide bounded filters and permissioned exports; never allow editing or deleting audit history from the interface.

### 9.8 Developer code sample and API reference blocks

- Label language and operation; provide copy control with accessible feedback.
- Use synthetic values and clearly marked placeholders.
- Never include real credentials or imply sample code is production-safe without the corresponding authentication, idempotency, signature-verification and error-handling guidance.
- Wrap long code horizontally or allow safe scrolling without breaking the page.
- Version and base URL labels must match the documented API environment.

## 10. Responsive behavior

Proposed breakpoints are design candidates, not fixed decisions: narrow mobile around 360–479 px, larger mobile/small tablet around 480–767 px, tablet/small desktop around 768–1023 px, and desktop at 1024 px and above. Choose final breakpoints based on content fit, not device names.

- Navigation becomes a drawer or another accessible compact pattern on narrow screens.
- Multi-column forms collapse to one column when fields become cramped.
- Tables use responsive alternatives or labelled horizontal overflow; do not hide critical status or amount without a discoverable detail view.
- Primary actions remain reachable without requiring precise pointer hover.
- Dialogs and drawers fit the viewport, support scroll, and keep action controls reachable above mobile keyboards.
- Avoid horizontal page overflow at common zoom levels.
- Test reflow at 200% zoom and narrow viewport widths.
- Touch targets must meet the selected accessibility standard; exact size is to be reviewed.
- Chart summaries have a textual/table alternative.

## 11. Accessibility and motion

Target WCAG 2.2 AA as a proposed baseline for review; the final conformance target and applicable legal obligations must be confirmed by accountable reviewers.

- Semantic HTML and correct headings/landmarks.
- Full keyboard operation with visible focus that is not obscured by sticky navigation.
- Programmatic names, roles, values and descriptions for controls.
- Contrast testing for text, icons, borders, focus indicators and disabled states.
- Screen-reader announcement for meaningful async status updates without announcing every background refresh.
- Error identification and correction suggestions; do not rely on color or position alone.
- Focus trapping/restoration for modal interactions and accessible mobile navigation.
- Reduced-motion preference support; avoid flashing, excessive animation and motion that conveys essential information without a static equivalent.
- Do not require drag-only, hover-only or timed interaction when a reasonable alternative exists.
- Provide text alternatives for charts, icons and non-text indicators.
- Ensure language, currency, timezone and date format are not ambiguous.

## 12. Component state and implementation contract

Every component specification/implementation record should include:
1. Name and purpose.
2. Accepted inputs/properties and emitted user actions.
3. Variants and supported combinations.
4. Visual and interaction states.
5. Keyboard and assistive-technology behavior.
6. Responsive behavior.
7. Permission and sensitive-data constraints.
8. Loading/error/recovery behavior.
9. Test IDs and mapped DOC-31 requirement IDs.
10. Owner, version/change notes and approval status.

Do not create near-duplicate components for the same semantic purpose without documenting the reason. A design-system change that affects financial status, permissions, money-out, refunds, keys, webhooks or audit behavior must include impact analysis against DOC-31, DOC-34, DOC-43 and DOC-52.

## 13. Proposed component inventory and test identifiers

These are planning IDs only. They do not claim that components or tests exist or have run.

| Proposed test ID | Acceptance criterion |
|---|---|
| DS-TOKEN-001 | Text, surface, border, focus and semantic color tokens pass reviewed contrast checks |
| DS-NAV-002 | Sidebar, active route, breadcrumbs and mobile navigation work with keyboard and assistive technology |
| DS-BTN-003 | Buttons expose correct names/states; loading and disabled states prevent misleading duplicate interaction |
| DS-FORM-004 | Labels, hints and errors are programmatically associated; server validation remains authoritative |
| DS-FORM-005 | Recoverable failures preserve safe form values and clearly distinguish validation from ambiguous outcomes |
| DS-TABLE-006 | Tables support keyboard/screen-reader use, clear empty states, mobile behavior and tenant-safe filtering |
| DS-STATUS-007 | Status labels include text and are sourced from authoritative state; unknown is never styled as success |
| DS-DIALOG-008 | Dialogs/drawers manage focus, escape/close policy and focus restoration correctly |
| DS-FEEDBACK-009 | Critical errors persist in context; toasts are accessible and do not falsely claim success |
| DS-RESP-010 | Core screens reflow at agreed widths and 200% zoom without losing required actions or data |
| DS-A11Y-011 | Core components meet the approved accessibility conformance checklist |
| DS-SECRET-012 | API key/webhook secret controls avoid URL/log/analytics exposure and enforce one-time reveal policy |
| DS-TXN-013 | Transaction lifecycle component distinguishes payment state, provider attempt, webhook delivery and settlement |
| DS-APPROVAL-014 | Approval component enforces permission, reason capture, audit and separation-of-duties rules |
| DS-AUDIT-015 | Audit viewer is read-only, permissioned, redacted and export-controlled |
| DS-CODE-016 | API examples use synthetic placeholders and provide accessible copy feedback |
| DS-MOTION-017 | Reduced-motion settings are respected and essential information remains available without animation |
| DS-STALE-018 | Stale/partial data and unavailable services are clearly distinguished from zero values and payment outcomes |

Before implementation, map every proposed test to stable DOC-31 requirement IDs in DOC-34. During authorized implementation, add concrete test-file references and execution evidence under DOC-45's evidence rules.

## 14. Design review and acceptance checklist

- [ ] Product owner accepts the visual direction and original brand treatment.
- [ ] Design reviewer approves token values, typography, spacing, density and responsive behavior.
- [ ] Accessibility reviewer confirms the target standard and checks contrast, keyboard, screen reader, zoom and reduced motion.
- [ ] Security reviewer checks secret handling, permission-aware actions, redaction, internal console separation and sensitive overlays.
- [ ] Finance/operations reviewer validates financial states, amounts, freshness, reconciliation and recovery copy.
- [ ] DOC-51 role/permission model and DOC-52 screen behavior remain consistent.
- [ ] Proposed tests are mapped to DOC-31/DOC-34 and later to actual test artifacts.
- [ ] Empty, loading, stale, error, unknown, permission-denied and suspended states are reviewed for relevant components.
- [ ] No real customer information, provider secrets or live transactions are used in visual/test fixtures.
- [ ] Approval status and remaining decisions are recorded before implementation authorization.

## 15. Open decisions

1. Final brand palette, typography, icon library and logo usage.
2. Exact spacing scale, breakpoints, maximum content width, radii, elevation and compact/dense table mode.
3. Whether dark mode is required for initial release.
4. Final accessibility conformance target, supported browsers and assistive-technology test matrix.
5. Charting/data-visualization conventions and export behavior.
6. Exact component framework/API conventions after the application stack is approved.
7. Localization, timezone and currency formatting policy.
8. Which components require formal design sign-off versus implementation-owner review.
9. Final mapping of proposed DS test IDs to stable DOC-31 requirements and actual test artifacts.

## 16. Current disposition

DOC-53 is a design-system proposal to be used alongside DOC-51's permissions and DOC-52's screen behavior. It is not a visual prototype and has not been implemented or tested. G1 remains NOT PASSED; G2 remains NOT APPROVED. The component and accessibility checks are proposed only. This document does not authorize coding, Figma work, provider contact, live credentials, transactions, merge or deployment.
