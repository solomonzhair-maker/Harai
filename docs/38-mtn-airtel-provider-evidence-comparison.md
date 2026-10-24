# DOC-38 — MTN MoMo and Airtel Uganda Provider Evidence Comparison

**Status:** Draft evidence comparison — not a provider selection, legal conclusion, or implementation authorization  
**Evidence review date:** 2026-10-19  
**Related decisions/documents:** DEC-016, DEC-017, DEC-022, DOC-28, DOC-29, DOC-30, DOC-32, DOC-36, DOC-37  
**Purpose:** Consolidate the owner's read-only portal findings and official API documentation into a comparison that distinguishes visible access, documented technical capability, and unverified commercial/legal permission.

## 1. Executive summary

Both providers publish documentation relevant to collections and money-out. The evidence is sufficient to refine HARAI's requirements and next provider questions, but it is **not sufficient to select a provider, select a funds-flow model, or authorize implementation**.

- **MTN MoMo:** The logged-in developer portal showed active Collections and Collection Widget subscriptions in the Test environment. No Disbursement subscription or visible production approval was found.
- **Airtel Uganda:** The authenticated developer dashboard showed zero live applications and three archived applications. Relevant collection, disbursement and merchant-collection APIs are documented, but no production approval was visible.
- **Multi-merchant:** Both portals publish aggregator-/merchant-related technical material. Neither portal inspection provides written confirmation that HARAI's exact business model is contractually permitted.
- **Funds flow and commercial terms:** The inspected pages did not establish account ownership/control, settlement timetable, prefunding terms, full fees, reserves, or contractual allocation of responsibilities.
- **Next gate:** Continue evidence consolidation and requirements work. Any provider outreach or onboarding requires separate explicit owner authorization. Qualified Ugandan legal/compliance review remains required before selecting a funds-flow model.

## 2. Evidence levels

Use the following distinctions throughout this document:

1. **Account-specific observation:** A status or product shown in the owner's logged-in portal at the review time.
2. **Official technical documentation:** Provider-published API/product guidance; it describes a technical surface, not necessarily the owner's access or contractual rights.
3. **Written provider confirmation:** A provider's attributable written answer to HARAI's exact use case; not yet obtained.
4. **Contractually confirmed:** An applicable signed agreement/product schedule or formal approval reviewed by the appropriate accountable reviewer; not yet obtained.
5. **Qualified review required:** A legal, regulatory, financial or security conclusion that must be made by a qualified reviewer for the actual HARAI model.

Do not upgrade evidence level 1 or 2 into level 3 or 4.

## 3. Provider comparison

| Dimension | MTN MoMo Uganda | Airtel Uganda | Evidence conclusion |
|---|---|---|---|
| Logged-in developer portal | Test environment visible | Authenticated Uganda developer portal visible | Both inspected read-only on 2026-10-19 |
| Visible account products/applications | Collections and Collection Widget subscriptions shown Active; no Disbursement subscription shown | 0 live applications; 3 archived applications | Account states are observations, not production authorization |
| Production access | No live approval/status visible; Go-Live route documented | Production endpoint docs exist; Getting Started says production credentials follow activation/onboarding/approval; no approval visible | Production access remains unconfirmed for both |
| Collection API | Request-to-Pay; asynchronous status query; Collection Widget also subscribed | Collection v2.0 payment initiation, transaction enquiry and full refund documented | Both publish collection capabilities; integration access and terms differ and need validation |
| Money-out API | Disbursement catalogue and API docs include Transfer/Deposit, status/balance and refunds; no account subscription visible | Disbursement v2.0 documents B2B/B2C and direct merchant transfers | Technical documentation exists; live permission and funding terms unconfirmed |
| Multi-merchant evidence | Aggregator API documentation describes `transferType`; production values described as contract/onboarding dependent | Merchant Collection documents micro-merchant registration, relationship IDs, transfers, enquiry and refund | Useful technical evidence; neither establishes HARAI's written eligibility |
| Refunds/reversals | Disbursement Refund V1/V2 and status documented; Collection invoice cancellation visible; generic collection reversal not identified in inspected pages | Collection full refund and Merchant Collection refund documented; separate Reverse Interops docs exist | Exact semantics, eligibility, windows, finality and accounting effects still need product-specific confirmation |
| Callback handling | HTTPS and registered hosts; callback sent once without retry; polling/reconciliation fallback | Collection callbacks configurable; authenticated variant uses HMAC-SHA256 | HARAI must design authenticated callback verification, deduplication, status enquiry and recovery |
| Transaction states/errors | Asynchronous collection; insufficient balance and payer-limit errors documented for money-out | Success, failed, ambiguous, in-progress and expired states; insufficient funds, limits, duplicate IDs and timeouts documented | Canonical state model must preserve pending/ambiguous states and prevent unsafe duplicate operations |
| Reconciliation evidence | Daily reconciliation recommended; detailed commercial settlement report not visible in inspected pages | Transactions Summary includes transaction/service-charge fields; settlement schedule/policy not visible | Need sample reports, stable references, settlement mapping and discrepancy process |
| Fees and settlement | No complete fee schedule, settlement cadence, reserves or payout top-up terms found | No complete fee schedule, settlement cadence, reserves or payout cadence found | Unknown — written confirmation required |
| Legal/contractual eligibility | Not confirmed | Not confirmed | Hard blocker before live multi-merchant service |
| Enterprise portal | Not needed to view developer portal; exact business contracting route unresolved | Airtel Money Enterprise account is not registered; no registration was performed | Do not infer developer access equals enterprise onboarding or approval |

## 4. Official source register

### MTN MoMo

- Profile and subscriptions: https://momodeveloper.mtn.com/profile
- Product descriptions: https://momodeveloper.mtn.com/Product-descriptions
- Collections product: https://momodeveloper.mtn.com/product#product=collections
- Disbursements product: https://momodeveloper.mtn.com/product#product=disbursements
- Collection API documentation: https://momodeveloper.mtn.com/API-collections#api=collection
- Disbursement API documentation: https://momodeveloper.mtn.com/API-collections#api=disbursement
- Aggregator guidance: https://momodeveloper.mtn.com/content/html_widgets/1t1oe.html
- Callback guidance: https://momodeveloper.mtn.com/content/html_widgets/c0m2f.html
- Best practices: https://momodeveloper.mtn.com/content/html_widgets/xomcy.html
- Go-Live guidance: https://momodeveloper.mtn.com/golive

### Airtel Uganda

- Authenticated developer dashboard: https://developers.airtel.ug/developer
- Product overview: https://developers.airtel.ug/home
- Getting Started / access lifecycle: https://developers.airtel.ug/documentation/getting-started/1.0
- Collection APIs v2.0: https://developers.airtel.ug/documentation/collection-apis/2.0
- Disbursement APIs v2.0: https://developers.airtel.ug/documentation/disbursement-apis/2.0
- Merchant Collection: https://developers.airtel.ug/documentation/merchant-collection/1.0
- Transactions Summary: https://developers.airtel.ug/documentation/transactions-summary-apis/1.0
- Account API: https://developers.airtel.ug/documentation/account/2.0
- Reverse Interops: https://developers.airtel.ug/documentation/reverse-interops/1.0

## 5. What this means for the funds-flow comparison

DOC-28's candidate models remain open:

- **Provider-managed funds / authorized intermediary:** May reduce direct provider integration burden only if the intermediary contract explicitly allows HARAI's sub-merchant model, supplies all required rails, and provides auditable transaction/settlement evidence. No such intermediary evidence has been collected.
- **Direct provider integrations:** Both providers publish relevant technical APIs, but account-specific access, production eligibility, multi-merchant permission, contracting route, fees and funds flow are unconfirmed.
- **Conditional hybrid:** Keep as a possible future option only if each route independently passes all hard gates and the additional reconciliation/support complexity is justified.

No numerical provider/model scores should be assigned yet. Unknown evidence is **N/S (not scored)**, not zero and not a positive assumption. Hard gates override any future weighted score.

## 6. Required written evidence before a model recommendation

For each proposed provider/product/contracting route, obtain or formally disposition:

1. Written permission for HARAI's exact multi-merchant/sub-merchant gateway model and the legal entity that may contract.
2. The role of HARAI, provider/intermediary and each merchant, including merchant onboarding and due-diligence responsibilities.
3. A funds-flow diagram identifying payer, collection account, account owner/controller, settlement destination, payout funding source, any prefunding/reserve, and the party able to initiate each movement.
4. Exact product/API scope in Uganda, including production approval criteria, collections, disbursements, status enquiries, callbacks, refunds, reversals/returns and reporting.
5. Complete fees, taxes, transaction limits, daily/rolling limits, reserves, minimums, settlement cut-offs/timing, failure fees and termination terms.
6. Sample or documented transaction and settlement reports, stable references, reconciliation workflow, and discrepancy escalation process.
7. Callback authentication, idempotency/duplicate protection, retry/polling behavior, status finality and support/SLA commitments.
8. Qualified Ugandan legal/compliance assessment of the actual operating and funds-flow model, including required permissions, privacy and contractual responsibilities.

DOC-29 is a reusable inquiry pack, not a record of the exact messages sent. The product owner reported sending initial non-binding informational inquiries on 2026-10-24 (DOC-00 DEC-049 / DOC-62 §8); this does not establish delivery, provider receipt or any permission. This document does not authorize follow-up outreach, applications, onboarding, credentials or financial activity.

## 7. Requirements implications (design guidance only)

These observations should be reflected in requirements and later architecture review without beginning payment-engine implementation:

- Use provider adapters behind a canonical HARAI transaction interface; do not leak provider-specific states into merchant-facing semantics without a documented mapping.
- Preserve explicit pending, ambiguous, expired, failed and succeeded states; never infer failure solely from a timeout.
- Use status enquiry and reconciliation as recovery paths where provider callbacks may be lost or sent only once.
- Require callback authentication where available, strict HTTPS, signature verification, replay/deduplication controls, and asynchronous processing.
- Treat collections, payouts, refunds, reversals and payout returns as distinct operations with provider-specific eligibility and ledger effects.
- Make each rail/capability unavailable for live use until provider, legal, security, funding and reconciliation gates pass.
- Do not assume HARAI holds, pools, safeguards or owns customer funds, or operates a stored-value wallet.

Relevant existing DOC-31 requirement IDs already cover the principal implications; avoid inventing duplicate IDs merely because new evidence arrived:
- **COL-004 / OUT-002:** pending, ambiguous and unknown outcomes, status recovery and no blind retry.
- **COL-005 / DEV-002:** callback authentication, replay/deduplication and recoverable event delivery.
- **OUT-001 / OUT-003 / OUT-004:** provider-authorized payouts, original-transaction-linked refunds, and traceable reversals/returns.
- **FIN-003 / REC-001 / REC-002:** explainable fees/net settlement, transaction-to-provider evidence matching, and visible discrepancy handling.
- **MER-002 / OPS-002:** live activation and release remain gated on provider permission, compliance and operational evidence.

These are planning requirements, not approved architecture or permission to code the payment engine.

## 8. Current gate and next actions

**G0 remains in progress; G1 is not passed.** The portal inspections reduce uncertainty about visible test/developer surfaces but do not satisfy written provider, legal, commercial or production-access gates.

Next documentation batch:
1. Update DOC-28 with the portal evidence distinctions and unresolved hard gates without scoring either model.
2. Add provider-specific acceptance/evidence requirements to DOC-31 where current IDs do not already cover them; avoid duplicate requirement IDs.
3. Cross-check DOC-34 traceability and DOC-32 gap list against those changes.
4. Keep DOC-36 product-owner decisions pending until the owner explicitly responds.
5. Do not contact providers, register Airtel Enterprise, submit applications, generate live credentials, run transactions, merge or deploy without separate explicit authorization.

## 9. Review boundary

The portal inspection was performed read-only on 2026-10-19. No credentials, API keys, tokens or private transaction details were retained. No provider was contacted. No application, subscription, setting or transaction was changed. This comparison is internal research, not independent external certification, legal advice, or a provider commitment.
