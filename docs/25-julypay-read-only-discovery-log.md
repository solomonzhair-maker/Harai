# DOC-25 — JulyPay Read-Only Discovery Log

**Status:** In progress — authenticated inspection blocked in this session  
**Inspection date:** 2026-10-17  
**Reference:** https://app.julypay.net  
**Safety boundary:** No credentials entered; no forms submitted; no financial, account-changing, export, or secret-management actions performed.

## 1. Purpose

Maintain an evidence-labelled record of what has actually been inspected in the JulyPay reference interface. This log separates observations from unknowns and is not a specification of JulyPay's internal implementation.

## 2. Session/access result

**Observed**
- Opening the supplied entry URL redirected to https://app.julypay.net/login.
- The page title was “Sign In - JulyPay”.
- Visible fields: Email or Phone, Password, and Remember me.
- Visible actions/links: Sign In, Forgot password, and Create one now.
- The visible links pointed to https://app.julypay.net/forgot-password and https://app.julypay.net/register.
- No sandbox or test-mode indicator was visible on the sign-in screen.
- A second attempt to reuse the browser session also reached the sign-in screen; the session was not authenticated.

**Not inspected / unconfirmed**
- Authenticated dashboard and navigation.
- Collection/deposit and withdrawal flows.
- Payment links and STK Push.
- Bank/card workflows.
- Scheduled withdrawals, refunds, reversals and payouts.
- API-key creation, scopes, IP allowlisting, webhooks and delivery history.
- Transaction states, validation errors, retry behavior and status-resolution flows.
- Role/permission hints, reporting/reconciliation, verification and support screens.
- Whether any listed feature exists in the current product version or is available to this account.

The earlier inventory in DOC-01 remains an initial inventory, not evidence that these authenticated screens were inspected in this session. Until access is available, keep these items labelled as previously reported/partially observed or unconfirmed according to their actual evidence, and do not upgrade their status based on this log.

## 3. Discovery method for the next authenticated session

Once the owner has authenticated in the browser preview, continue read-only inspection. Do not ask for or copy passwords, OTPs, recovery codes, API secrets or private customer/transaction data. Do not create or modify settings, generate credentials, trigger payments, initiate payouts/refunds, or export records.

For each screen/workflow, record:
1. Screen name and navigation path.
2. Visible fields, controls, labels and help text.
3. Normal sequence and required versus optional inputs, only where safely visible.
4. Role/permission indicators and disabled/unavailable actions.
5. Visible statuses and their explanatory text.
6. Validation, error, timeout, retry, cancellation and recovery behavior when observable without submitting consequential actions.
7. Environment/test-mode indicators.
8. Evidence classification: Observed, Partially observed, Unconfirmed or Proposed.
9. HARAI requirement IDs and open questions; do not infer hidden implementation.

## 4. Discovery coverage checklist

- [ ] Dashboard and navigation
- [ ] Transactions list, details, filters and status timeline
- [ ] Collections/deposits and provider selection
- [ ] Hosted checkout/payment links
- [ ] STK Push or equivalent mobile-money initiation
- [ ] Withdrawals/payouts and scheduled operations
- [ ] Refunds/reversals and exception handling
- [ ] Bank/card methods, if present
- [ ] API keys, scopes, IP allowlist and credential lifecycle
- [ ] API documentation and error conventions
- [ ] Webhook configuration, signing, attempts, retries and replay
- [ ] Reports, settlement and reconciliation
- [ ] Merchant profile, verification and security
- [ ] Team/role permissions, if present
- [ ] Support tickets and diagnostic information
- [ ] Empty, loading, pending, unavailable and validation-error states

## 5. Current conclusion

Only the unauthenticated entry screen is confirmed by this inspection. This is insufficient to declare JulyPay discovery complete. The next action is for the owner to authenticate through the browser preview (never share credentials in chat), then resume non-sensitive read-only inspection. If authenticated access cannot be made available, continue HARAI's independent requirements design while explicitly keeping reference-specific behavior unconfirmed.

## 6. Source links

- Entry URL: https://app.julypay.net
- Sign-in page: https://app.julypay.net/login
- Password recovery link: https://app.julypay.net/forgot-password
- Registration link: https://app.julypay.net/register
