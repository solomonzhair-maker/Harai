# DOC-02 — Product Scope and Requirements Framework

**Status:** Draft — requires product-owner review  
**Purpose:** Define how HARAI requirements are captured and how scope is controlled before implementation.

## 1. Product statement

HARAI is intended to be a reusable payment gateway for the owner's current and future systems. It should provide a consistent API, merchant dashboard, hosted checkout, event/webhook delivery and, if approved and supported, money-out operations.

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
- Potential bank/card methods, subject to scope and provider validation.
- API credentials, IP allowlisting, rate limits and API versioning.
- Webhook delivery, transaction states and reconciliation.
- Ledger, fees, settlement records and financial reporting.
- Payouts, withdrawals, refunds and reversals if supported and approved.
- Audit trails, risk controls, monitoring, support and recovery.

### Not assumed
- HARAI will hold or custody customer funds.
- Every provider supports every direction, country, currency, refund or status query.
- Card processing, cross-border settlement, escrow or stored-value wallet functionality is V1.
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

## 8. Open decisions

Initial markets/currencies; merchant segments; custody/settlement model; V1 methods; pricing and limits; verification policy; team roles and dual control; service/recovery targets; data retention; and provider-specific acceptance/reconciliation sources. Record each in DOC-00 rather than silently assuming an answer.
