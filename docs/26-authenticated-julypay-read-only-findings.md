# DOC-26 — Authenticated JulyPay Read-Only Findings

**Status:** Draft — observed interface findings; not a complete product or provider specification  
**Inspection date:** 2026-10-17  
**Reference:** https://app.julypay.net  
**Method:** Authenticated browser preview, read-only inspection  
**Safety:** No forms submitted, settings changed, payments/withdrawals initiated, data exported, or secrets/customer details recorded.

## 1. Evidence rules

- **Observed:** The page, navigation item, control, or label was visible in the authenticated interface.
- **Partially observed:** The screen or workflow was visible, but its full behavior and outcomes were not tested.
- **Unconfirmed:** Not safely verified or not visible during this session.
- These findings describe visible UI only. They do not establish API behavior, provider capability, contractual rights, settlement mechanics, or internal implementation.

## 2. Authenticated access and navigation

The authenticated dashboard was visible at https://app.julypay.net/dashboard.

Visible navigation areas:
- Dashboard
- Transactions
- Send Money
- Withdraw
- Scheduled Withdrawals
- Deposit
- Payment Links
- API Keys
- Webhooks
- Support
- Profile

The inspection intentionally did not record account-specific metrics, transaction records, customer data, endpoint URLs, or credential material.

## 3. Screen-level observations

| Area | Evidence | Observed UI details | Limits / open questions |
|---|---|---|---|
| Dashboard | Observed | Authenticated dashboard and navigation visible | Metric definitions, calculation rules, filters, loading/error states not verified |
| Transactions | Partially observed | Transaction area is available; visible status vocabulary included Completed and Failed | Search/filter behavior, detail fields, status transitions and authoritative source not fully inspected |
| Send Money | Partially observed | Form categories included phone number, amount and description; Send Money action visible | No submission; validation, permissions, limits, confirmation and failure behavior unverified |
| Withdraw | Partially observed | Mobile-money withdrawal area, amount, optional description, bank-details guidance and registered-phone restriction were visible | No submission; provider coverage, approvals, limits, timing, cancellation and reconciliation unverified |
| Deposit | Partially observed | Mobile-money STK Push and bank-transfer options; visible guidance described entering number/amount, initiating STK Push and confirming on the phone | No payment initiated; provider/API details, consent records, callback security and timeout handling unverified |
| Payment Links | Partially observed | Search/status filtering; empty state “No payment links found”; creation form categories included title, description, amount, fee payer, expiry and usage limit | No link created; payer journey, expiry enforcement, fee calculation, revocation and reuse rules unverified |
| Scheduled Withdrawals | Partially observed | A schedule frequency labelled “Every day”; Pause and Delete controls visible | No controls used; execution semantics, timezone, approvals, retry and cancellation effects unverified |
| API Keys | Partially observed | API-key area, active-key indication, Create and Delete controls | No key opened, generated, copied or deleted; scopes, storage, reveal-once behavior, rotation and audit unverified |
| Webhooks | Partially observed | Two entries visible with Active and Triggered labels; event labels covered transaction, wallet, deposit, withdrawal and send-money success/failure outcomes | Endpoint values intentionally not recorded; signing, retry schedule, delivery history, replay, SSRF protection and event guarantees unverified |
| Profile | Partially observed | Profile information, ID verification, bank details, password update, two-factor authentication, email notifications and account deletion areas visible | No changes made; verification requirements, recovery behavior, session controls and permission rules unverified |
| Support | Observed as navigation | Support navigation item visible | Ticket lifecycle and diagnostics not inspected |

Visible status labels included **Active**, **ACTIVE**, **Completed**, and **Failed**. Capitalization differs in the interface; HARAI must define its own canonical states and presentation rules rather than copying inconsistent labels.

No validation or error alerts were visible on the unsubmitted forms. This does **not** mean error handling is absent; error paths were not exercised.

## 4. Reference routes observed

- Dashboard: https://app.julypay.net/dashboard
- Send Money: https://app.julypay.net/send-money
- Withdraw: https://app.julypay.net/withdraw
- Deposit: https://app.julypay.net/deposit
- Payment Links: https://app.julypay.net/payment-links
- Payment-link creation: https://app.julypay.net/payment-links/create
- Scheduled Withdrawals: https://app.julypay.net/scheduled-withdrawals
- API Keys: https://app.julypay.net/api-keys
- Webhooks: https://app.julypay.net/webhooks
- Profile: https://app.julypay.net/profile

Routes are recorded as navigation references only; availability and behavior may vary by account or product version.

## 5. HARAI requirements implications (proposals, not approved scope)

1. Define a canonical transaction state model; distinguish pending/unknown outcomes from final failure.
2. Make transaction history searchable and provide an auditable timeline without treating a dashboard summary as the financial source of truth.
3. Specify payment-link amount, fee, expiry, usage-limit, revocation and duplicate-payment behavior before implementation.
4. Treat withdrawals and scheduled money-out as privileged operations with explicit limits, authorization, audit, duplicate-execution prevention and reconciliation.
5. Define API-key scopes, one-time secret display, revocation, rotation, secure storage, optional IP restrictions and audit evidence.
6. Define webhook event schemas, signature verification, delivery attempts, bounded retries, deduplication, safe replay and endpoint security.
7. Define profile/verification states, protected evidence access, MFA/session recovery and account-deletion implications.
8. Provide consistent empty, loading, pending, success, failure and validation states, including accessible status announcements.
9. Verify every provider-specific claim from official documentation and authorized provider access; visible options do not prove API availability or commercial permission.
10. Keep original HARAI architecture, implementation, API contracts and branding; use JulyPay solely as a workflow reference.

## 6. Remaining discovery gaps

- Transaction detail view, filters, pagination, and status history.
- Dashboard metrics and balance semantics.
- Payment-link payer journey, fees, expiration, revocation and limits.
- Provider names, API contracts, callback authentication and status-query behavior.
- Withdrawal approvals, fees, limits, cancellation, reversals and settlement.
- API-key scope/rotation behavior and IP allowlisting.
- Webhook signing, retries, replay and delivery logs.
- Verification requirements, team roles, permission boundaries and support workflow.
- Empty/loading/error states and safe recovery from timeouts.
- Public API documentation and official provider capability evidence.

Do not attempt to fill these gaps by triggering financial actions, exposing secrets, or inspecting private customer data.

## 7. Source-of-truth relationship

- DOC-01 remains the broader feature inventory and should be updated only where evidence supports a status change.
- DOC-24 contains differentiator proposals; none become V1 requirements automatically.
- DOC-00 owns decisions and blockers.
- DOC-02 defines the requirement process and scope boundaries.
- DOC-03 remains a provisional roadmap with approval gates.

## 8. Current conclusion

Authenticated read-only discovery is now **partially completed**. The listed UI areas and visible controls were observed, but the complete workflows, error behavior, permissions, API semantics, provider support and commercial/legal prerequisites remain unverified. G0 discovery is still in progress. No payment-engine implementation, merge, deployment, or live financial activity is authorized by these observations.
