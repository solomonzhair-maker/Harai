# DOC-01 — JulyPay Feature Discovery Inventory

**Status:** Draft — initial pass  
**Evidence type:** Read-only inspection of the authenticated app and visible navigation/route labels.  
**Boundary:** HARAI may learn from observed workflows but will use original code, branding and authorized integrations.

## 1. Observation labels

- **Observed:** a page or navigation item was visible during discovery.
- **Partially observed:** the area was visible but the complete workflow or all states were not inspected.
- **Unconfirmed:** not sufficiently verified.
- **Proposed for HARAI:** a product requirement under consideration, not a claim about JulyPay.

Account-specific values, transaction records, customer details, identifiers and credentials are intentionally excluded.

## 2. Initial feature inventory

| Feature area | Status | HARAI implication | Remaining discovery |
|---|---|---|---|
| Dashboard | Observed | Operational overview and recent activity | Metric definitions, date filters, empty/loading/error states |
| Wallet / transaction summary | Observed | Display summaries must not replace an authoritative ledger | Available vs pending vs reserved vs settled balance semantics |
| Transaction history | Observed | Searchable transactions with details and status history | Filters, pagination, export and safe provider references |
| Deposits / collections | Observed | Define collection initiation, confirmation and failure handling | Method-specific workflows and lifecycle |
| Withdrawals / money-out | Observed | Privileged, risk-controlled workflow | Limits, approval, cancellation, reversals and notifications |
| Scheduled withdrawals | Observed | If implemented, scheduling needs authorization and execution audit | Timezones, cancellation, retry and failure behavior |
| Payment links | Observed | Consider hosted collection links | Creation, expiry, amount rules, revocation and payer journey |
| Mobile-money STK Push | Partially observed | Provider-specific mobile-money adapter(s) | Countries, products, consent, callbacks, timeout and reconciliation |
| Bank transfer / deposit | Partially observed | Bank methods require explicit provider and settlement design | Supported rails, references and reconciliation source |
| API keys | Observed | Credential lifecycle, scopes, revocation and rotation | Scope details, reveal-once, audit and last-used behavior |
| API documentation | Observed | Versioned API contracts and examples | Endpoint inventory, authentication, errors and rate limits |
| Webhooks | Observed | Signed, retryable event delivery and observability | Events, signing, retries, replay and delivery history |
| Profile and security | Observed | Account settings and recovery controls | MFA, sessions, recovery and notification workflows |
| Identity / business verification | Partially observed | Merchant activation may depend on verification and review | Required evidence, reviewer authority and resubmission |
| Support tickets | Observed | Support lifecycle with minimized diagnostic access | Ticket states, attachments, SLA and user notifications |
| Dedicated reports page | Unconfirmed | Reporting needs require a separate HARAI decision | Re-check navigation and exports |
| Dedicated refunds page | Unconfirmed | Refund/reversal requirements are needed regardless of UI discovery | Provider support, partial refunds and authorization |
| Team management / roles | Unconfirmed | HARAI needs an explicit role and permission design | Invites, roles, session controls and admin audit |
| Card payments | Unconfirmed | Do not promise until provider and compliance prerequisites are validated | Acquirer, supported markets, authentication and disputes |
| Bank payout rails | Unconfirmed | A bank deposit option does not prove bank payout support | Direction of funds, provider, fees and settlement |
| Disputes / chargebacks | Unconfirmed | Decide whether in scope, especially if cards are enabled | Evidence, deadlines, statuses and accounting treatment |

## 3. Read-only discovery protocol

1. Navigate only to ordinary read-only pages and lists.
2. Do not submit create/update/delete forms.
3. Do not initiate, approve, cancel or retry a payment, withdrawal, refund or payout.
4. Do not generate, reveal, copy or export credentials.
5. Do not reproduce customer names, phone numbers, account identifiers, balances, transaction IDs, tokens or secrets in this repository.
6. Record page purpose, actor, available action categories, input/output categories, status transitions, error paths, permission boundaries and confidence.
7. Mark a feature unconfirmed if inspecting it would require a consequential action or expose sensitive information.

## 4. Questions for each workflow

- Who can view and initiate the action?
- What authorization, step-up authentication or second-person approval is required?
- What are the preconditions and validation rules?
- What are the canonical statuses and permitted transitions?
- How are duplicate submissions prevented?
- What happens when a provider times out but may have completed the operation?
- How are callbacks authenticated and deduplicated?
- How are customer consent, amount, currency and payment authorization represented?
- How are fees, net amount and rounding determined?
- What audit evidence is retained?
- How is the final state reconciled against provider records?
- What does the merchant/customer see for pending, failed, reversed and delayed cases?

## 5. Candidate HARAI capability map

### Candidate V1 capabilities — pending approval
- Merchant dashboard and transaction search.
- Collection API and hosted checkout/payment links.
- MTN MoMo and Airtel Money adapters, subject to verified access and market support.
- Merchant API keys, scopes, revocation, rotation and optional IP allowlisting.
- Signed webhooks with retry and delivery history.
- Canonical transaction state machine and reconciliation tools.
- Auditable financial records and operational support workflow.
- Merchant profile, verification status and security controls.

### Scope decisions still required
- Bank collections and bank payouts.
- Card acquiring and disputes/chargebacks.
- Refunds and reversals.
- Scheduled payouts.
- Team invites, granular roles and dual approval.
- CSV exports and advanced reporting.
- Multiple countries, currencies and settlement accounts.

## 6. Next discovery actions

1. Revisit each workflow to record inputs, outputs, statuses and permission boundaries without performing actions.
2. Inspect public/developer documentation for API behavior, provider names, webhook signing, rate limits and error conventions.
3. Verify MTN/Airtel capability against official documentation and authorized access.
4. Convert confirmed workflows into uniquely identified requirements and acceptance criteria.
5. Keep unknowns in DOC-00; never fill gaps with invented details.
