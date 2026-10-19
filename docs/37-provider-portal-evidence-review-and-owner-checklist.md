# DOC-37 — Provider Portal Evidence Review and Owner-Guided Checklist

**Status:** Research checklist; no provider account has been accessed and no provider contacted  
**Review date:** 2026-10-19  
**Related documents:** DOC-00, DOC-28, DOC-29, DOC-30, DOC-33, DOC-35, DOC-36  
**Purpose:** Convert public provider research into a precise checklist for evidence the product owner may retrieve from authenticated MTN and Airtel portals. The owner performs login and any account-specific actions; ChatGPT interprets redacted evidence and updates the provider comparison.

## 1. Public-source findings

### MTN MoMo

Official public resources:
- MTN Uganda Open API overview: https://www.mtn.co.ug/helppersonal/mtn-open-api/
- MoMo developer portal: https://momodeveloper.mtn.com/
- Uganda developer product catalogue: https://momoapi.mtn.com/products
- Uganda Collections Widget product details: https://momoapi.mtn.com/Uganda_Widget_productDetails
- Uganda Disbursement product details: https://momoapi.mtn.com/Uganda_Disbursement_productDetails

Public evidence indicates MTN offers API products for collections and disbursements, and its Uganda documentation describes collections-account and disbursement-funding arrangements. These pages do **not** establish that HARAI is eligible to operate a multi-merchant gateway, that a single HARAI account can collect on behalf of unrelated merchants, or that the desired payout and settlement arrangement will be approved.

### Airtel Money Uganda

Official public resources:
- Airtel Uganda developer portal: https://developers.airtel.ug/
- Airtel Africa developer portal: https://developers.airtel.africa/homeMy
- Airtel Uganda enterprise portal: https://enterprise.airtel.ug/
- Enterprise account registration flow: https://enterprise.airtel.ug/register
- Airtel Money for Business overview: https://enterprise.airtel.ug/about/at-a-glance

Public pages describe collections and disbursements, and the enterprise registration page lists “Payment Aggregators” as an entity type. That is a reason to investigate, **not proof that HARAI qualifies, has been approved, or can use a particular API/funds-flow arrangement**. The developer portal and enterprise business portal are distinct surfaces; access to one must not be assumed to grant access to the other.

### Uganda regulatory and data-protection sources

- Bank of Uganda financial infrastructure and supervised providers: https://bou.or.ug/financial_infrastructure_innovation
- Revised National Payment Systems Oversight Framework (2025): https://bou.or.ug/uploads/Revised_BOU_National_Payment_Systems_Oversight_Framework_2025_698b3e9745.pdf
- Personal Data Protection Office: https://pdpo.go.ug/
- National Payment Systems Act text (ULII legal database): https://ulii.org/en/akn/ug/act/2020/15/eng%402023-12-31

These sources help frame questions and verify public facts. They do not determine HARAI's own licensing status or replace review of its actual activities and funds flows.

## 2. Evidence to retrieve from MTN, if an account already exists

Start at the official MoMo developer portal. Select Uganda where prompted and inspect the account without creating a production application or submitting an application unless the owner separately decides to do so.

Record or screenshot (with secrets and personal data redacted):
1. Whether the account is registered and verified; account type and country.
2. Which API products are available to this account in sandbox and production.
3. Whether Collection, Disbursement/Transfer, transaction status, account balance, refund, callback/notification or other relevant products appear, and whether each is sandbox-only, production-eligible, pending approval or unavailable.
4. Product subscription prerequisites, application approval steps, terms and any stated account/business requirements.
5. Whether documentation identifies an aggregator/technology-partner/multi-merchant model and what approval is required.
6. Whether each merchant needs its own account, collection wallet, contract, settlement account or credentials.
7. Written details for collection settlement, liquidation, payout prefunding, account ownership, refunds/reversals, transaction limits, fees, reconciliation reports and support/escalation.
8. Any sandbox credentials' existence should be recorded as “available/not available” only. Never copy the credential values into chat, screenshots, GitHub or a public document.

Do not initiate real collections, transfers, disbursements or refunds. Do not switch a production toggle, request live keys, accept binding commercial terms or submit business details externally without an explicit decision.

## 3. Evidence to retrieve from Airtel, if an account already exists

Inspect these separately if the owner already has access:
- Airtel developer portal: https://developers.airtel.ug/
- Airtel Money enterprise portal: https://enterprise.airtel.ug/

Record or screenshot (redact credentials and personal information):
1. Which portal the account belongs to: developer/API, enterprise/business, or both.
2. Country selected (Uganda), entity type and current onboarding/application status.
3. Whether “Payment Aggregators” is only a selectable registration category or whether an actual HARAI application exists and has been approved.
4. Available API products and operations in sandbox and production, including collections, disbursements/payouts, transaction query/status, callbacks, refunds/reversals and reconciliation/reporting where shown.
5. Any published or account-specific aggregator requirements, approval letters, contracts or product schedules.
6. Whether each underlying merchant requires a separate merchant/business account, agreement, settlement destination or API credentials.
7. Collection settlement path, who owns and controls funds, payout funding source, any prefunding, fees, limits, settlement timing, refund/reversal capability and operational support.
8. Any sandbox credentials' existence should be recorded as “available/not available” only. Never copy credential values.

Do not start registration or submit KYC/company documents merely to explore the UI. If the account is not registered, report that fact; we will prepare a separate decision brief before any application.

## 4. Safe evidence sharing

Preferred order:
1. Copy non-sensitive product names, status labels, eligibility requirements and terms text into the conversation.
2. Share screenshots only after cropping or masking names, phone numbers, email addresses, account identifiers, QR codes, transaction details and all credentials/tokens.
3. Never share passwords, OTPs, API keys/secrets, access tokens, private keys, recovery codes, full identity documents, customer records or live transaction evidence.
4. If a document is confidential, provide a short redacted excerpt or summary instead of uploading the complete contract. Keep unredacted contracts in controlled storage.
5. Label each item with provider, portal, country, date viewed and whether it is public, sandbox, account-specific or contractual.

## 5. Evidence register template

| Evidence ID | Provider / portal | Country | Evidence type | Claim supported | Status / limitation | Related decisions |
|---|---|---|---|---|---|---|
| PROV-MTN-001 | MTN MoMo developer portal | Uganda | Account product list | Products visible to the logged-in account | Pending owner evidence | DEC-016, POD-04 |
| PROV-MTN-002 | MTN account-specific terms | Uganda | Terms / approval | Eligibility for intended multi-merchant model | Pending written evidence | DEC-016 |
| PROV-AIR-001 | Airtel developer portal | Uganda | Account product list | API products visible to the logged-in account | Pending owner evidence | DEC-016, POD-04 |
| PROV-AIR-002 | Airtel enterprise portal | Uganda | Entity / application status | Whether aggregator application is available or approved | Pending owner evidence | DEC-016 |
| PROV-FLOW-001 | Either provider | Uganda | Written funds-flow terms | Account ownership, settlement and payout funding | Pending written evidence | DEC-016 |
| PROV-COM-001 | Either provider | Uganda | Commercial schedule | Fees, limits and support commitments | Pending written evidence | DEC-016 |

Do not change a status to “approved” merely because a menu item exists or an application form can be opened. Approval requires explicit account-specific evidence.

## 6. What we can decide from portal evidence

Portal evidence can establish which products and application states are visible to the owner. It may narrow the provider shortlist and help us draft an accurate question to provider support. It cannot, by itself, establish legal permission or the right to handle money for unrelated merchants.

Before recommending a funds-flow model, the evidence must address:
- contracting entity and authorized service model;
- account holder and beneficial owner of collection and payout accounts;
- whether funds ever pass through, are controlled by or are held for HARAI;
- merchant onboarding and due diligence responsibilities;
- settlement destinations and timing;
- payout funding source, prefunding and reconciliation;
- refund, reversal, failed/unknown outcome and dispute handling;
- fees, limits, liability, support and termination;
- required regulatory permissions and qualified legal/compliance assessment.

## 7. Current conclusion and next action

**Provider decision remains open.** Public sources establish that MTN and Airtel have relevant business-payment offerings; they do not establish HARAI eligibility or the desired multi-merchant arrangement.

If accounts already exist, the next useful owner-assisted step is a read-only inspection of the MTN Uganda developer portal and Airtel Uganda developer/enterprise portals. If no account exists, tell ChatGPT which portal is unavailable; do not create an account or submit an application just to satisfy this checklist. ChatGPT will then prepare the smallest, safest next-step plan.

No provider has been contacted by this document. No account has been logged into. No credentials have been requested or recorded. No application, live integration, real transaction, merge or deployment is authorized.
