# DOC-25 — JulyPay Read-Only Discovery Log

**Status:** In progress — authenticated inspection partially completed  
**Inspection date:** 2026-10-17  
**Reference:** https://app.julypay.net  
**Safety boundary:** No credentials were entered by the assistant; no forms submitted; no financial, account-changing, export, or secret-management actions performed.

## 1. Purpose

Maintain an evidence-labelled record of what has actually been inspected in the JulyPay reference interface. This log separates observations from unknowns and is not a specification of JulyPay's internal implementation.

## 2. Session/access result

### Initial attempt
- The first browser session reached https://app.julypay.net/login.
- The sign-in screen displayed Email or Phone, Password, Remember me, Sign In, Forgot password and registration links.
- The session was not authenticated at that point.

### Authenticated attempt
The owner then authenticated interactively in the browser preview. The subsequent read-only inspection confirmed that the authenticated dashboard was visible at https://app.julypay.net/dashboard.

The following areas and UI controls were observed: Dashboard, Transactions, Send Money, Withdraw, Scheduled Withdrawals, Deposit, Payment Links, API Keys, Webhooks, Support and Profile. Visible details included payment-link creation field categories, scheduled-withdrawal controls, API-key controls, webhook status/event labels, deposit method options, and profile/security sections.

A detailed evidence-labelled record is maintained in [DOC-26 — Authenticated JulyPay Read-Only Findings](26-authenticated-julypay-read-only-findings.md).

## 3. Safety and privacy boundaries followed

- No password, OTP, recovery code or credential was requested in chat or recorded.
- No create/update/delete form was submitted.
- No payment, deposit, withdrawal, refund, payout or other financial operation was triggered.
- No API key was opened, generated, copied or deleted.
- No webhook endpoint or secret was recorded.
- No customer personal data, private transaction details, account-specific metrics or sensitive identifiers were copied into the repository.
- No settings were changed and no records were exported.

## 4. Remaining discovery checklist

- [ ] Dashboard metric definitions and balance semantics
- [ ] Transaction details, filters, pagination and status timeline
- [ ] Collection initiation and provider-specific lifecycle
- [ ] Hosted checkout and complete payment-link payer journey
- [ ] Validation and error states without consequential submissions
- [ ] Withdrawal authorization, limits, cancellation and reconciliation
- [ ] Scheduled-withdrawal timezone, retry and execution behavior
- [ ] Refund/reversal capabilities and safeguards, if available
- [ ] Bank/card methods and supported markets, if available
- [ ] API documentation, scopes, key rotation and IP allowlisting
- [ ] Webhook signatures, retries, delivery history and safe replay
- [ ] Reports, settlement and reconciliation
- [ ] Verification requirements and role/permission boundaries
- [ ] Support ticket lifecycle and diagnostic access
- [ ] Official provider documentation and authorized provider capability evidence

Items above remain unconfirmed unless separately supported by evidence. Visible UI options do not prove that a backend capability, provider API, contract, or market permission exists.

## 5. Discovery method

Continue with ordinary read-only pages and non-sensitive UI details. Do not submit consequential forms or inspect private customer records or secret material. Record screen purpose, visible controls, status labels, confidence, HARAI implications and open questions. Treat timeouts and untested error paths as unknown rather than assuming failure behavior.

## 6. Current conclusion

Authenticated UI discovery is now partially completed, not complete. See DOC-26 for the detailed findings. G0 remains in progress while workflow behavior, provider capabilities, permissions, commercial access and operational details are validated. No payment-engine implementation, merge, deployment or live financial action is authorized by these observations.
