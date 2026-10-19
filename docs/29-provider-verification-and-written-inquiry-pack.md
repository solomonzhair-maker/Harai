# DOC-29 — Provider Verification and Written Inquiry Pack

**Status:** Draft — ready for owner review before external submission  
**Market focus:** Uganda first  
**Purpose:** Obtain authoritative, written evidence needed to compare provider-managed funds, direct integrations and any evidence-supported hybrid model.  
**Decision boundary:** This document does not authorize contacting providers, submitting onboarding applications, signing agreements, receiving credentials or initiating financial activity.

## 1. How to use this pack

1. Start from the provider's verified official website and confirm the correct enterprise/API contact channel.
2. The business owner decides whether to send the inquiry and who is authorized to represent HARAI.
3. Keep the inquiry factual. Describe HARAI as a proposed multi-merchant payment gateway; do not claim approval, licensing, provider partnership or production readiness.
4. Request written responses and current contractual/product documentation. Public marketing pages alone do not prove eligibility.
5. Store correspondence in an access-controlled business record. Never put API secrets, private identity documents, account credentials or confidential transaction data in GitHub issues or public repository files.
6. Record unanswered questions as **Unknown — written confirmation required**. Do not score an unknown as a positive result.

## 2. Suggested inquiry message

**Subject:** Request for technical and commercial clarification — proposed Uganda multi-merchant payment gateway

Hello,

We are evaluating authorized payment-integration options for HARAI, a proposed Uganda-first payment gateway intended to support multiple businesses. We are conducting requirements and feasibility research and have not assumed provider approval or access.

Could your team advise whether you support this business model and provide the appropriate current product, technical, commercial and onboarding documentation? We are particularly seeking written clarification on the questions below.

We will submit any required business or due-diligence information only through your verified official process. Please let us know the correct team and next steps.

Regards,  
HARAI project owner

**Before sending:** add the owner's approved business contact details and only make representations that are accurate. Do not invent a registered company, licence, partner status, customer volume or forecast.

## 3. Questions requiring written answers

### A. Eligibility, authorization and legal roles
1. Do you contract directly with a multi-merchant payment gateway/aggregator in Uganda? If yes, what exact entity type, approval, licence or authorization is required?
2. May HARAI onboard its own business customers/sub-merchants under the proposed arrangement? What approval is required for each merchant?
3. Which party is the regulated/contracting provider for each activity? What roles and responsibilities would belong to HARAI, the provider, each merchant and any intermediary?
4. Are there restrictions on merchant categories, use cases, transaction purposes, geography or onward payouts?
5. If an intermediary is involved, does its agreement explicitly permit this gateway/sub-merchant model? Who is responsible for obtaining that written permission?

### B. Funds flow, accounts and settlement
6. For collections, who receives the payer's funds, which account is credited, who legally owns/controls that account, and how/when is the merchant settled?
7. For money-out, what is the source of funds? Is prefunding required? Who holds the prefunded balance, what controls apply, and how are failed or returned payouts handled?
8. Does the provider or intermediary ever hold, pool, safeguard or transfer funds on behalf of multiple merchants? Describe the contractual model and restrictions.
9. What settlement cycles, cut-off times, holidays, reserves, holds, minimums, maximums, currencies and settlement reports apply?
10. Can every transaction and settlement item be reconciled using stable references and downloadable/API-accessible reports?

### C. Technical capability and finality
11. Which exact APIs/products are available for collections, status queries, disbursements, refunds and reversals in Uganda?
12. Which methods, currencies, transaction types and merchant categories are supported for each API?
13. How are authentication, sandbox access, production access, callback verification, IP restrictions and credential rotation handled?
14. What are the documented transaction states and rules for pending, timeout, duplicate request, late callback, reversal, refund and final status?
15. Are idempotency keys supported? If not, what is the approved method to prevent duplicate financial operations?
16. What are the callback retry policy, rate limits, API availability/SLA, support escalation process and incident notification commitments?
17. What sandbox scenarios and test evidence are available before production approval?

### D. Commercial terms and liability
18. Provide the complete fee schedule: transaction fees, payout fees, refunds/reversals, settlement, minimums, taxes, setup, monthly charges and other pass-through charges.
19. What transaction limits, rolling limits, reserves, chargebacks/claims, fraud liability, indemnities and dispute responsibilities apply?
20. What reporting, data protection, retention, audit, termination, data-export and post-termination obligations apply?
21. Which written agreement or approval explicitly authorizes HARAI's intended business model and sub-merchant activity?

## 4. Provider-specific evidence tracker

Maintain one row per provider/product/contracting route. A provider name alone is not enough; direct integration and an intermediary arrangement must be evaluated separately.

| Field | Required evidence |
|---|---|
| Provider and exact product | Official product/API name, market and direction |
| Contracting route | Direct, intermediary or provider-managed funds |
| Eligibility | Written confirmation of HARAI's proposed gateway/sub-merchant model |
| Authorization | Contractual permissions and any required licence/approval confirmed by qualified reviewers |
| Collection capability | API documentation, currencies, limits, status model, sandbox |
| Money-out capability | API documentation, funding source, beneficiary rules, limits, status model, sandbox |
| Refund/reversal | Supported scope, eligibility, limits and lifecycle |
| Funds flow | Account ownership, custody/holding roles, settlement route and timing |
| Reconciliation | Transaction references, settlement reports, data access and discrepancy handling |
| Commercials | Complete written fee schedule and other material terms |
| Security/operations | Authentication, callbacks, SLA, support, incident and data obligations |
| Evidence date/owner | Date received, reviewer and controlled evidence location |
| Open blockers | Exact unanswered questions and accountable owner |

Use these evidence labels: **Publicly documented**, **Written provider confirmation received**, **Contractually confirmed**, **Qualified review required**, **Unknown — written confirmation required**, and **Not applicable with rationale**. A marketing page must not be labelled contractually confirmed.

## 5. Weighted comparison method

Score only after enough evidence is available. Use **N/S (not scored)** for unknowns; do not assign a neutral numeric score to missing evidence.

| Criterion | Weight |
|---|---:|
| Regulatory fit and clarity of responsibility | 20% |
| Written authorization for multi-merchant/sub-merchant model | 15% |
| Funds flow, account ownership and settlement transparency | 15% |
| Coverage of collections and money-out | 15% |
| Reconciliation quality and data access | 10% |
| Total commercial viability | 10% |
| Reliability and support | 5% |
| Security and data responsibilities | 5% |
| Integration and operating effort | 5% |
| **Total** | **100%** |

Score each criterion from 0 to 5 only when evidence supports it: 0 = unacceptable/not supported; 1 = major gap; 2 = partial/conditional; 3 = minimum requirements met; 4 = strong fit; 5 = verified strong fit. Record evidence and rationale beside every score. A high weighted score never overrides a hard gate.

## 6. Hard gates — no recommendation until resolved

- HARAI's proposed business model and each required activity are legally and contractually permitted.
- Multi-merchant/sub-merchant participation is explicitly authorized.
- The full funds flow and account ownership are understood and reviewed.
- The money-out funding source and failed/returned payout treatment are defined.
- Settlement and reconciliation evidence is available and operationally usable.
- Required provider access, support, security and sandbox-to-production path are documented.
- Qualified Ugandan legal/compliance review has assessed the selected model.
- Commercial terms and risk allocation are understood and accepted by the owner.

## 7. Current known public evidence and limitations

DOC-28 records public evidence concerning MTN MoMo collection/disbursement products, Airtel Uganda enterprise collections/disbursements and its registration flow, and Bank of Uganda oversight material. Those sources help identify candidates for inquiry; they do **not** establish HARAI's eligibility, licence status, contract rights, pricing, production access, settlement/custody arrangement or support for every refund/reversal scenario.

## 8. Evidence register template

For each response or document, record:
- Evidence ID and provider/product.
- Document title and version/date.
- Official source or secure internal record reference.
- Date received and reviewer.
- Exact claim the evidence supports.
- Limits, exclusions, assumptions and expiry/revalidation date.
- Related requirement/decision IDs.
- Whether legal, security, finance or technical review remains outstanding.

Do not place confidential contracts or sensitive onboarding evidence in the public repository. Commit only a redacted summary and controlled evidence references where appropriate.

## 9. Exit criteria

The provider comparison is ready for a recommendation only when each viable route has evidence for the hard gates, unresolved gaps are explicitly owned, weighted scores are defensible, and the product owner plus qualified reviewers can compare the consequences. Until then, DOC-28's decision remains open and no live integration is authorized.
