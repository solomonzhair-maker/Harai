# DOC-02 — Product Scope and Requirements Framework

**Status:** Draft — requires product-owner review  
**Purpose:** Define how HARAI requirements are captured and how scope is controlled before implementation.

## 1. Product statement

HARAI is intended to be a reusable payment gateway for the owner's current and future systems. It should provide a consistent API, merchant dashboard, hosted checkout, event/webhook delivery, collections and a planned money-out workstream, with refunds/reversals where explicitly supported and authorized.

JulyPay is a reference for discovering useful merchant workflows. HARAI must use original code and branding and authorized provider integrations. A similar screen does not prove that an underlying API, commercial permission or legal operating model is identical.

## 2. Intended actors

| Actor | Need | Access principle |
|---|---|---|
| Merchant owner | Configure merchant account, view activity, manage authorized operations | Strong authentication and least privilege |
| Merchant developer | Integrate APIs and test webhooks | Scoped, revocable credentials; test/live separation |
| Merchant finance operator | Review collections, fees, settlement and reconciliation | Read access by default; privileged actions separately authorized |
| HARAI support operator | Investigate incidents and merchant questions | Minimized, time-limited and audited access |
| Risk/verification reviewer | Review verification, limits and exceptions | Explicit permission and recorded decisions |
| Platform administrator | Manage platform access and configuration | Strong controls; no routine direct mutation of financial records |
| Payer | Complete a payment with clear merchant, amount, currency and result | Consent, privacy and clear status |
| Integrated merchant system | Create requests and consume status/webhook events | Tenant-bound identity and idempotent requests |

Exact role names and permission matrices belong in a separate access-control specification.

## 3. Scope boundaries

### In scope for discovery
- Merchant onboarding and account security.
- Collections, hosted checkout and payment links.
- Provider adapters for supported mobile-money rails.
- A planned multi-provider payment-method package that includes both MTN MoMo and Airtel Money, card payments through an eligible processor/acquirer, and additional suitable methods. Each integration and capability remains gated by method-specific eligibility, contracts, technical access, cost, legal/compliance review and operational readiness.
- API credentials, IP allowlisting, rate limits and API versioning.
- Webhook delivery, transaction states and reconciliation.
- Ledger, fees, settlement records and financial reporting.
- Payouts and withdrawals are part of the intended launch-package requirements, but each method remains blocked until provider capability, contractual permission, legal/compliance requirements, funding/settlement flow, and operational controls are verified. Refunds and reversals are included where the selected provider explicitly supports and authorizes them.
- Audit trails, risk controls, monitoring, support and recovery.

### Not assumed
- HARAI will hold or custody customer funds.
- Every provider supports every direction, country, currency, refund or status query.
- Card processing is an intended payment-method objective, but the specific processor/acquirer, card schemes, eligibility and launch availability must be verified. Cross-border settlement, escrow and stored-value wallet functionality are not assumed to be in V1.
- API keys alone are sufficient authorization for money-out actions.
- A provider timeout means the payment failed.
- A dashboard balance is authoritative without a defined ledger and reconciliation model.

## 4. Requirement record format

Every requirement must include:
- Stable ID, title and one testable behavior.
- Status: Draft, Under Review, Approved, Blocked or Superseded.
- Source: observed workflow, official provider documentation, business decision, risk finding or proposal.
- Actor, permissions, preconditions and dependencies.
- Normal flow and failure/edge cases.
- Data and audit requirements.
- Security, privacy and financial impact.
- Objective acceptance criteria and verification method.
- Open questions and accountable owner.

A requirement cannot be approved while a consequential ambiguity remains unresolved.

## 5. Core product principles

### Financial correctness
- Represent money using integer minor units or a rigorously defined decimal strategy; never binary floating point for authoritative money calculations.
- Store currency explicitly and reject unsupported combinations.
- Define fees, rounding, net amounts and settlement rules before implementation.
- Use auditable financial events/entries; corrections should be explicit compensating entries, not silent edits.
- Reconcile internal records against provider records and surface discrepancies.
- Never infer final payment status solely from a browser redirect or unverified callback.

### Reliability
- Use idempotency for requests that can create financial operations.
- Treat provider timeouts and ambiguous outcomes as pending/unknown until resolved.
- Authenticate callbacks, deduplicate them and tolerate duplicate/out-of-order delivery.
- Use bounded retries with backoff and a defined dead-letter/manual-review path.
- Define safe recovery after process, queue, database or provider failure.

### Security and privacy
- Enforce tenant isolation in API queries, mutations, queues, webhooks and admin workflows.
- Keep secrets out of source control, logs, URLs, client-side bundles and documentation.
- Minimize personal data and define retention/deletion rules.
- Apply least privilege, session protection, rate limits and audit logging.
- Require explicit authorization for sensitive credential and money-out actions.
- Never expose full API secrets after creation; define rotation and revocation.

### Provider abstraction
- Use a canonical internal model with explicit provider capability/status mapping.
- Do not pretend providers have identical features.
- Preserve provider references and safe diagnostic codes for reconciliation.
- Maintain a capability matrix by provider, country, currency, direction and environment.

## 6. Canonical lifecycle principles

DOC-10 will define the exact state machine. The model must distinguish request validation, created/awaiting, pending/processing, confirmed success, confirmed failure, expiry/cancellation where supported, reversal/refund where applicable, and unknown outcome requiring reconciliation.

Every state transition needs a source, timestamp, idempotency behavior and audit record. An unauthenticated callback must never directly set a transaction to success.

## 7. V1 definition of done

V1 is not ready until:
1. Product scope and provider access are approved.
2. Every in-scope requirement maps to acceptance criteria and tests.
3. Tenant isolation and role permissions pass positive and negative tests.
4. Duplicate requests, retries, callback replay, out-of-order events and ambiguous timeouts are tested.
5. Ledger, fee calculation, reconciliation and correction workflows are reviewed.
6. API credentials, key rotation, IP allowlisting and webhook signing are reviewed.
7. Monitoring, alerts, support procedures, backup and recovery are demonstrated.
8. Provider approvals, commercial agreements and required legal/compliance reviews are completed by accountable parties.
9. Staging verification uses provider-approved test environments.
10. Production launch has explicit authorization, rollback plan and named operational owner.

## 8. Product differentiation proposals

DOC-24 records proposed HARAI differentiators beyond the reference interface, including developer-first sandbox tooling, transaction timelines and unknown-outcome recovery, provider capability/routing controls, reconciliation and ledger visibility, granular team permissions and dual approval, safer credential/webhook operations, risk controls, accessible mobile UX, support diagnostics, trustworthy exports, recoverability and reusable integrations for multiple products. These are proposals, not automatically approved V1 scope. Evaluate each against provider feasibility, commercial/legal requirements, risk, cost and testable acceptance criteria.

## 9. Capability catalogue and launch-package requirements

The following is the complete discovery baseline for the intended launch package. It is not evidence that every capability can legally or commercially be enabled at launch. Each capability must receive a stable requirement ID and testable acceptance criteria before implementation approval.

### 9.1 Platform and merchant management
- Merchant registration, invitation, profile, business category, contact details and status.
- Verification submission, review, rejection, resubmission, expiry and audit history; sensitive evidence access must be restricted.
- Tenant-isolated workspaces, team membership, role assignment, least privilege and access revocation.
- Merchant activation gates that prevent live operations until required approvals and provider configuration are complete.
- Merchant settings, supported methods, environment separation, notification preferences and account recovery.
- Support cases, status communications and controlled support access with audit records.

### 9.2 Collections and payer experience
- API-created collection/payment intents with server-side validation of merchant, amount, currency, purpose/reference and permitted method.
- Hosted checkout and payment links with expiry, revocation, usage rules and clear merchant identity, amount and currency.
- Payer consent/authorization flow as required by the provider.
- Provider-specific method/country/currency capability checks before presenting or accepting a method.
- Transaction status retrieval and payer-facing pending, success, failure, expired/cancelled and unknown states as applicable.
- Duplicate submission protection, idempotency and safe handling of provider timeouts.
- Payment receipts/notifications only from verified status, with delivery outcome recorded.

### 9.3 Money-out, refunds and reversals
- Payout/withdrawal initiation, beneficiary validation, destination eligibility, funding-source validation, limits, fees, approvals and status tracking.
- Role-based authorization and dual control for sensitive operations where required by risk policy, provider terms or law.
- Provider submission reference, status lookup, timeout/unknown outcome recovery, reconciliation and incident escalation.
- Refund eligibility tied to a confirmed original transaction; support full or partial amounts only where the provider permits them.
- Reversal/return handling that preserves original transaction history and records provider evidence.
- Prevention of over-refunds, duplicate execution, replay, race conditions and unauthorized beneficiary changes.
- No money-out operation may be treated as successful solely because a request was accepted; finality must follow an approved status-resolution rule.
- All money-out, refund and reversal capabilities remain gated by verified provider support, legal/commercial authority, funding and settlement design, and testing.

### 9.4 Financial records, fees and reconciliation
- Explicit currency and precise monetary representation; no binary floating-point for authoritative financial calculations.
- Defined gross amount, fee payer, fee components, rounding, net amount, taxes where applicable, settlement and timing semantics.
- Auditable transaction history and a reviewed ledger design; corrections use compensating entries rather than silent mutation.
- Reconciliation against provider transactions, settlement reports and payout records, with discrepancy queues, ownership and resolution evidence.
- Separate display balances from authoritative accounting records unless their derivation and meaning are defined.
- Financial reports and exports with permission checks, clear time zone/date ranges, reproducible totals and privacy controls.
- Fee schedules, limits and method availability must be configuration-controlled, versioned and traceable to an approved source.

### 9.5 Developer platform and integrations
- Versioned API contracts, authentication, scoped keys, idempotency, consistent error schemas, pagination and correlation IDs.
- Sandbox/live separation, test credentials and fixtures, API documentation, integration examples and a safe API explorer.
- API key creation, display-once secret handling, rotation, expiry, revocation, scope changes and audit trail.
- Optional IP allowlisting with trusted-proxy rules, lockout-safe recovery and clear CIDR validation.
- Signed webhooks with event IDs/timestamps, secret rotation, bounded retries, deduplication, delivery logs and safe replay.
- Provider adapters that normalize only genuinely common behavior while preserving provider-specific capabilities, references, errors and limitations.
- No provider credential, private key, personal evidence or sensitive payload may be placed in source control or exposed in logs.

### 9.6 Risk, security and operational controls
- Authentication, MFA/step-up where appropriate, secure sessions, recovery, brute-force protection and account notifications.
- Tenant isolation across API requests, database access, background jobs, webhook delivery, exports and support tools.
- Rate limits, abuse detection, transaction limits, anomaly review, method-level kill switches and controlled emergency disablement.
- Audit logs for privileged access, merchant verification decisions, configuration changes, credential lifecycle, payouts, refunds, reversals and financial corrections.
- Monitoring for provider errors, pending-duration anomalies, callback delays, queue backlog, reconciliation breaks and service health.
- Incident response, provider outage procedures, unknown-outcome playbooks, backup/restore, disaster recovery, retention and deletion processes.
- Accessible responsive dashboard and checkout states, including loading, empty, pending, failure and recovery paths.
- Defined support ownership, escalation routes, service/recovery targets and customer complaint handling.

### 9.7 Payment-method coverage and conditional extensions
The intended multi-provider payment-method package includes both **MTN MoMo and Airtel Money**, **card payments** through an eligible processor/acquirer, and additional suitable payment methods selected through product and feasibility review. The system must not be designed around an either/or choice between MTN and Airtel. It should support method-specific provider adapters and a capability matrix so each method can be enabled independently when its gates pass.

Bank transfers, other local/international payment methods, additional currencies, cross-border transactions, subscriptions, scheduled payouts, split settlements/marketplace payouts, advanced automated risk scoring and regional expansion require method- or feature-specific requirements, provider/processor permission, legal/compliance review, operational controls and acceptance criteria. Listing a method as intended scope does not mean that a processor is selected, a contract exists, or the method is launch-ready.

## 10. Requirement priority and traceability

Use these priority labels during the detailed requirements pass:
- **P0 — launch gate:** security, tenant isolation, authorization, money correctness, idempotency, verified status, auditability, reconciliation, required compliance/provider permissions, backup/recovery and safe operations.
- **P1 — core launch capability:** approved collections, intended money-out workflows, merchant onboarding, APIs, webhooks, reporting, support and essential developer experience, subject to the hard gates in this document.
- **P2 — conditional enhancement:** capabilities that add value but are not required to make the core package safe and usable.
- **Blocked — external evidence/authority required:** cannot be implemented or advertised until the named dependency is resolved.

Every requirement must link to its owner, decision, dependency, acceptance criteria and test evidence. Unknowns must remain explicitly unknown; do not turn an ambition into a claim of support.

## 11. Open decisions

Initial launch currencies and settlement expectations; exact merchant segments and eligibility; custody/settlement model; provider and intermediary permissions; provider-specific collection, money-out, refund and reversal capabilities; pricing and limits; verification policy; team roles and dual control; service/recovery targets; data retention; taxes and fee presentation; and authoritative reconciliation sources. Record each in DOC-00 rather than silently assuming an answer.

## 12. Review checklist before requirements approval

- [ ] Every launch capability has a stable ID, actor, preconditions, behavior and acceptance criteria.
- [ ] Every provider-dependent behavior is backed by current official evidence or marked blocked.
- [ ] Funds flow, legal responsibility, settlement account ownership and money-out funding source are explicit.
- [ ] Tenant boundaries and sensitive-action permissions are defined.
- [ ] Pending/unknown outcomes, retries, duplicate requests and callback reordering are specified.
- [ ] Ledger, fees, reconciliation and correction rules have accountable finance review.
- [ ] Privacy, retention, incident response and support obligations are recorded.
- [ ] Requirements map to tests and release gates.
- [ ] Product owner and relevant independent reviewers have explicitly approved the baseline.

This document remains a draft. It does not authorize implementation, provider onboarding, live credentials, real transactions, merge or deployment.
