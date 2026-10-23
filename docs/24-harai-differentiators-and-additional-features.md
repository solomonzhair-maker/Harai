# DOC-24 — HARAI Differentiators and Additional Feature Proposals

**Status:** Proposed — for product-owner review  
**Purpose:** Identify capabilities HARAI should consider beyond the JulyPay interface reference. These are original product proposals, not claims about JulyPay or confirmed V1 commitments.

## 1. Design boundary

JulyPay is a visual and workflow reference only. HARAI's internal architecture, source code, database, payment orchestration, security controls, provider adapters and operating model will be designed independently. Use original HARAI branding, copy and implementation. Do not infer hidden internals from visible screens.

A feature is not approved for V1 merely because it appears here. Each proposal must be assessed for customer value, provider feasibility, legal/commercial prerequisites, risk, implementation cost and acceptance criteria.

## 2. Recommended differentiators

### DF-01 — Developer-first integration experience
- A clearly separated sandbox and live mode with unmistakable environment indicators.
- Test credentials and simulated provider outcomes that cannot move real money.
- API explorer, copyable examples, SDKs where useful, versioned OpenAPI contract and migration guides.
- Webhook test console, sample events, signature verification helper and delivery replay in a safe, permission-checked workflow.
- Integration health checks that identify invalid configuration without exposing secrets.

**Why:** Reduce integration time and make HARAI reusable by Shigona, Herupu and future merchant systems.

### DF-02 — Payment status transparency and recovery
- A transaction timeline showing request, provider attempt, callback/status query, state changes and webhook delivery.
- Explicit pending/unknown states; never label an ambiguous timeout as failure.
- Safe status refresh/reconciliation workflows and clear next steps for a merchant.
- Correlation IDs across API, provider attempts, jobs, ledger and webhooks.
- An operations queue for unresolved outcomes and stale pending transactions.

**Why:** Prevent duplicate payments and reduce support uncertainty.

### DF-03 — Provider capability and routing layer
- A capability matrix by provider, country, currency, collection/payout direction, environment and feature.
- A provider adapter contract with normalized internal states and preserved provider references.
- Configurable routing/fallback only where safe and authorized; never silently retry a potentially completed payment through a second rail.
- Health and error-rate visibility per provider, with manual disable/kill switch.
- Provider-specific limits, fees, maintenance windows and eligibility checks.

**Why:** Add providers without coupling the entire platform to one provider's assumptions. Smart routing is conditional on reliable status resolution and explicit policy.

### DF-04 — Financial ledger and reconciliation centre
- Immutable, auditable financial entries and explicit compensating entries for corrections.
- Clear separation of gross amount, fees, net amount, provider settlement and merchant-available funds.
- Daily/provider reconciliation, exception queues, aging and documented resolution.
- Downloadable reports with consistent filters and permission controls.
- No editable "balance" field as the source of financial truth.

**Why:** Build trust and support accurate finance operations from the start.

### DF-05 — Strong merchant and team controls
- Multiple users per merchant with configurable least-privilege roles.
- Separate permissions for viewing transactions, managing credentials, issuing refunds and initiating/approving payouts.
- Optional maker-checker/dual approval for high-risk operations, configurable limits and step-up authentication.
- Session/device management, MFA, security notifications and audited role changes.
- Time-limited, reason-coded support access with audit trail and no routine access to secrets.

**Why:** Protect growing merchants and reduce the risk of one compromised account causing financial loss.

### DF-06 — Secure credentials and webhook operations
- API keys with explicit scopes, environment binding, last-used metadata, expiry, rotation and immediate revocation.
- Optional IP allowlisting with safe recovery and clear warnings about dynamic IPs.
- Webhook endpoint verification, signing-secret rotation, replay protection and per-attempt delivery history.
- Bounded retries, exponential backoff, dead-letter queue and controlled replay.
- SSRF protection for merchant-supplied callback URLs; redact secrets and sensitive payload fields from logs.

**Why:** Make integrations safer and easier to operate.

### DF-07 — Risk, limits and operational safeguards
- Configurable transaction limits and velocity controls, subject to provider and compliance requirements.
- Duplicate-payment and anomalous-pattern alerts with explainable reasons.
- Manual review queues and explicit decision records; avoid automatic irreversible action based only on a weak signal.
- Emergency provider/method/merchant kill switches with authorization and audit.
- A risk register that tracks mitigation, owner, evidence and residual risk.

**Why:** Create layered protection without promising that software alone prevents fraud.

### DF-08 — Merchant experience and accessibility
- Responsive dashboard designed for mobile browsers and installable PWA use if approved by architecture.
- Useful empty, loading, pending, failed, unavailable and partial-data states.
- Search/filter by transaction reference, date, status, method and amount, subject to access rights.
- Clear fees, payer instructions, expiry, refund eligibility and support path.
- Accessible keyboard navigation, contrast, screen-reader labels and localized dates/time zones.

**Why:** Make the platform usable by small merchants as well as technical teams.

### DF-09 — Notifications and support diagnostics
- Configurable operational/security notifications for failed integrations, repeated webhook failures, credential changes and reconciliation exceptions.
- Merchant-visible support cases linked to safe transaction references.
- Redacted diagnostic bundles with correlation IDs, timestamps and provider error categories.
- Incident/status communication and documented escalation ownership.
- Notification preferences and delivery audit; do not put secrets or unnecessary personal data in messages.

**Why:** Reduce time to resolution without leaking payment data.

### DF-10 — Reports, exports and merchant analytics
- Consistent transaction, fee, settlement and reconciliation reports.
- CSV exports with role checks, audit events, size limits and protection against spreadsheet formula injection.
- Dashboard metrics whose definitions are documented and reconcile to underlying records.
- Separate transaction date, provider settlement date and payout date where applicable.
- Avoid claiming predictive analytics until sufficient lawful, reliable data exists.

**Why:** Give merchants useful financial visibility without misleading totals.

### DF-11 — Reliability and recoverability
- Idempotency for money-moving requests and durable event/outbox patterns where appropriate.
- Queue retry limits, dead-letter handling, worker restart recovery and back-pressure.
- Database backups, restore drills, migration checks and a tested disaster-recovery plan.
- Monitoring for provider latency, pending-age, callback lag, webhook backlog, ledger imbalance and reconciliation delay.
- Defined service objectives and incident runbooks before production.

**Why:** Payment correctness must survive retries, outages and partial failures.

### DF-12 — Reusable integration platform
- Tenant-scoped configuration so Shigona, Herupu and future products can integrate without sharing secrets or data.
- Versioned API contracts and stable internal payment model.
- Per-integration keys, scopes, usage limits and audit logs.
- Test fixtures and SDK/examples generated from approved contracts.
- Explicit separation of platform operator, merchant and integrated application identities.

**Why:** Make HARAI a reusable platform rather than a one-off checkout.

## 3. Additional capabilities to evaluate, not assume for V1

- Multi-currency and cross-border support.
- Card acquiring, bank collections and bank payouts.
- Refunds, reversals, disputes and chargeback evidence.
- Scheduled payouts and beneficiary management.
- Merchant subscriptions/recurring billing.
- Split payments or marketplace settlements.
- Automated settlement forecasting and fee optimization.
- Multi-language dashboard and payer checkout.
- Public status page and merchant-facing incident history.

Each item depends on provider support, legal/commercial permissions, product scope and operational capacity. Do not advertise or expose a capability until verified and approved.

## 4. V1 prioritization proposal

**Foundational / strongly recommended for V1 design:** tenant isolation, role permissions, API key lifecycle, idempotency, canonical transaction states, audit trail, signed webhooks, reconciliation model, safe pending/unknown handling, provider capability matrix, sandbox/live separation, observability, backup/restore plan and support diagnostics.

**V1 implementation candidates, subject to provider access and scope:** MTN MoMo and Airtel Money collections, hosted checkout/payment links, transaction timeline, developer docs, webhook delivery console and financial reports.

**Conditional or likely later:** card acquiring, bank methods, cross-border/multi-currency, scheduled payouts, marketplace split settlement, subscriptions and advanced automated risk scoring.

This prioritization is a proposal only. Final V1 scope is recorded in DOC-00 after evidence and explicit approval.

## 5. Acceptance and traceability requirements

For each accepted differentiator, add:
1. A stable requirement ID and accountable owner.
2. User story, permissions and preconditions.
3. Normal, failure, timeout, retry and recovery paths.
4. Data classification, audit and retention needs.
5. Provider/commercial/legal dependencies.
6. Acceptance criteria and linked automated/manual tests.
7. Threat/risk review where it affects funds, credentials or personal data.
8. Decision record for inclusion in V1, later release or rejection.

## 6. Explicit non-goals

- Do not copy JulyPay source code, private assets, internal implementation or branding.
- Do not reverse-engineer hidden behavior or treat UI observations as proof of provider capability.
- Do not perform live financial actions during discovery.
- Do not enable real payouts, refunds or other money movement without provider authorization, approved controls and explicit launch approval.
- Do not describe proposed features as implemented or available.
