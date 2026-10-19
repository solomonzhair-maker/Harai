# DOC-28 — Funds-Flow Architecture Comparison

**Status:** Draft research baseline — not a final legal, commercial, or architecture decision.  
**Decision link:** DEC-016 in DOC-00.  
**Market focus:** Uganda first.  
**Scope:** Compare provider-managed funds arrangements with direct provider integrations for collections and money-out. Refunds and reversals are included only where contractually and technically supported.

## 1. Purpose and decision rule

The product owner selected an evidence-based comparison before choosing a funds-flow model. This document records an initial public-source review and the evidence still required.

A public API or sandbox is not proof that HARAI is eligible for production access, may aggregate multiple merchants, may receive or control funds, or may offer a regulated payment service. Do not make a final model choice until provider contracts, settlement arrangements, licensing responsibilities, fees, support, and operational requirements are verified.

## 2. Candidate models

### Model A — Provider-managed funds

An appropriately authorized payment provider or partner processes the relevant payment flows and settles funds to the merchant or approved destination under its own contracts and operating procedures. HARAI supplies gateway technology, merchant tools, transaction state tracking, webhooks, audit records, and reconciliation support within the permitted arrangement.

Questions to verify:
- Does the partner contractually allow a multi-merchant gateway and sub-merchant onboarding?
- Which party is merchant of record, payment service provider, aggregator/integrator, and settlement counterparty for each flow?
- Where do collected funds land, who can direct them, and who is responsible for settlement and safeguarding?
- Does the partner support both collections and payouts, as well as refunds/reversals?
- What are the fees, limits, settlement timing, reserves, dispute process, service levels, termination rights, and data responsibilities?
- Can HARAI obtain transaction-level evidence and reconcile every provider settlement?

### Model B — Direct provider integrations

HARAI contracts directly with each provider for approved products. Provider adapters normalize differences in authentication, requests, callbacks, statuses, transaction queries, disbursement, refunds, and reconciliation.

Questions to verify:
- What exact legal entity and merchant category can apply for each API product?
- Does the provider approve HARAI's intended multi-merchant or aggregator use case?
- Are direct agreements and required regulatory permissions in place for each activity?
- Which party receives and controls funds, and how are merchant settlements executed?
- Are production access, callback security, payout limits, support and escalation arrangements available?
- Can each provider provide the transaction and settlement evidence required for reconciliation?

### Model C — Hybrid (only if evidence supports it)

HARAI may use different models for different rails or use cases if each arrangement is explicitly authorized and the additional reconciliation and operational complexity is manageable. Hybrid is not assumed to be the default; it must pass the same review as either single model.

## 3. Initial public-source evidence

### MTN MoMo

- MTN's public developer materials list Collections and Disbursements APIs, alongside transaction-status and related capabilities. Official references:
  - https://momo.mtn.com/api/
  - https://www.mtn.co.ug/helppersonal/mtn-open-api/
  - https://momoapi.mtn.com/Uganda_Widget_productDetails
  - https://momoapi.mtn.com/Uganda_Disbursement_productDetails
- The Uganda collection widget documentation describes a collections/online-vendor account and subsequent liquidation to the client's bank account. This is a documented example of a provider-controlled collection and settlement path, not proof that HARAI qualifies for that product or can pool funds for merchants.
- MTN's Uganda disbursement product details describe a client account and prefunding through a Mobile Money Trust Account. Exact applicability, terms, limits and onboarding must be confirmed with MTN for HARAI's intended business model.
- Public API availability does not establish multi-merchant aggregation rights, live production approval, fees, or HARAI's legal permissions.

### Airtel Money

- Airtel Uganda's enterprise page publicly describes both collections into business accounts and disbursements from business accounts to recipients:
  - https://enterprise.airtel.ug/about/at-a-glance
- This establishes that business collections and disbursement services are publicly described. It does not establish that HARAI has API access, a sandbox, permission to aggregate merchants, supported refund/reversal APIs, production eligibility, or any particular pricing or settlement model. Obtain official technical and commercial documentation from Airtel.

### Uganda regulatory context

- The National Payment Systems Act, 2020 defines payment services and payment service providers and provides a licensing framework. The Act includes aggregator-related definitions and provisions concerning payment systems and services:
  - https://ulii.org/en/akn/ug/act/2020/15/eng%402023-12-31
- Bank of Uganda states that it licenses and authorizes payment system providers and operators under the Act and related regulations:
  - https://bou.or.ug/financial_infrastructure_innovation
- The precise classification and permissions applicable to HARAI depend on its actual activities, contracts, and funds flow. This document is not legal advice. Obtain qualified Uganda legal/compliance advice and, where appropriate, written clarification from the relevant authority before offering regulated services.

## 4. Provider evidence matrix (public-source review, 2026-10-19)

**Evidence labels**
- **Publicly documented:** visible on an official public page; does not prove HARAI eligibility or a contract.
- **Partially documented:** a related product or onboarding route is visible, but the required detail is missing.
- **Unknown — written confirmation required:** public evidence found does not settle the question.
- **Not assessed:** no reliable evidence collected for the specific item yet.

| Provider / route | Collections | Money-out / disbursement | Merchant aggregator eligibility | Settlement / custody | Refunds / reversals | Fees / limits / service levels | Current conclusion |
|---|---|---|---|---|---|---|---|
| MTN MoMo API — direct | **Publicly documented:** MTN API portal lists Collection and Collection Widget products | **Publicly documented at general API level:** portal lists Disbursement; exact Uganda product conditions need confirmation | **Unknown:** public API pages do not establish HARAI's multi-merchant aggregation rights | **Partially documented:** Uganda product-specific account and liquidation/prefunding details must be checked against current official product documentation and written terms | **Unknown:** confirm API/product-level reversal/refund rules, timing and finality | **Unknown for HARAI's commercial API agreement:** obtain enterprise price sheet, limits, settlement SLA and support terms | Technically plausible candidate; access, contracts, aggregation, funds flow and economics are not verified |
| MTN — via an authorized intermediary | Depends on intermediary's contracted MTN access | Depends on intermediary's contracted products and prefunding/settlement model | **Unknown:** intermediary must explicitly permit HARAI to onboard and service sub-merchants | **Unknown:** must map funds path, account ownership, settlement destination and liability | **Unknown:** must confirm which party can initiate and evidence reversals/refunds | Partner-specific; obtain full schedule including any pass-through fees | Candidate only if partner demonstrates rights and supplies auditable settlement data |
| Airtel Money Enterprise — direct | **Publicly documented:** Airtel Uganda describes collections into business accounts | **Publicly documented:** Airtel Uganda describes disbursements from business accounts | **Partially documented:** Airtel Enterprise registration lists “Payment Aggregators” as an entity type; this is an onboarding category, not approval or proof of HARAI eligibility | **Partially documented:** enterprise onboarding requests settlement bank/mobile details; exact collection account, settlement cycles and funds-control model remain unknown | **Unknown:** obtain official API documentation and written confirmation of refund/reversal capabilities | **Unknown for HARAI:** obtain commercial proposal, limits, minimums, settlement SLA and incident support | Strong candidate for formal inquiry; no assumption of production API or aggregator approval |
| Airtel — via an authorized intermediary | Depends on intermediary's contracted Airtel access | Depends on intermediary's contracted payout products | **Unknown:** intermediary must explicitly allow HARAI's multi-merchant model | **Unknown:** require signed funds-flow diagram and settlement evidence examples | **Unknown** | Partner-specific; obtain full fee and service schedule | Candidate only after legal, technical and commercial due diligence |
| Other licensed/authorized payment partner | Must be evidenced per product | Must be evidenced per product | Must be expressly allowed in the agreement | Must be mapped end to end | Must be verified per provider and flow | Must be provided in writing | Do not shortlist on marketing claims alone; check current Bank of Uganda authorization lists and exact permitted activities |

Official public references reviewed:
- MTN MoMo API catalogue: https://momo.mtn.com/api/
- MTN Uganda collection product details: https://momoapi.mtn.com/Uganda_Widget_productDetails
- MTN Uganda disbursement product details: https://momoapi.mtn.com/Uganda_Disbursement_productDetails
- Airtel Money Uganda enterprise services: https://enterprise.airtel.ug/about/at-a-glance
- Airtel Enterprise registration (entity categories and settlement/KYC steps): https://enterprise.airtel.ug/register
- Bank of Uganda payment-system oversight and licensed-provider directory: https://bou.or.ug/financial_infrastructure_innovation

## 5. Decision evidence checklist and weighted comparison

Use this scoring scale only after evidence is received: **0 = unacceptable/not supported; 1 = major gap; 2 = partial/conditional; 3 = meets minimum; 4 = strong; 5 = verified strong fit.** Every score must link to a source, written provider response, or accountable review. Unknown is **N/S (not scored)**, never zero or a guessed score.

| Criterion | Weight | Evidence required | Model A score | Model B score |
|---|---:|---|---|---|
| Regulatory fit and responsibility clarity | 20% | Qualified Uganda legal/compliance assessment; provider/partner roles and permissions in writing | N/S | N/S |
| Multi-merchant/aggregator authorization | 15% | Written approval for HARAI's exact intended model and sub-merchant lifecycle | N/S | N/S |
| Funds flow and settlement transparency | 15% | Account ownership, destination, timing, reserves, prefunding, control of funds, settlement statement examples | N/S | N/S |
| Collections and money-out coverage | 15% | Capability matrix for each required rail, direction and merchant category | N/S | N/S |
| Reconciliation and financial evidence | 10% | Transaction-level references, settlement reports, fees, dispute process and discrepancy support | N/S | N/S |
| Commercial viability | 10% | Written fees, minimums, volume commitments, taxes, reserves, termination and price-change terms | N/S | N/S |
| Reliability and support | 5% | SLA, support hours, incident escalation, status API, maintenance notices and recovery process | N/S | N/S |
| Security and data responsibilities | 5% | API authentication, callback signing, credential controls, data-processing terms, audit access | N/S | N/S |
| Integration and operating effort | 5% | Documented API quality, sandbox, test support, operational workload and maintenance estimate | N/S | N/S |
| **Total** | **100%** | Scores only after evidence review | **Not scored** | **Not scored** |

**Hard gates override the numerical score:** do not recommend a route if its legal permission/authorization, multi-merchant permission, settlement ownership, money-out funding source, or reconciliation evidence is unresolved. A high total score cannot compensate for a hard-gate failure.

## 6. Preliminary comparison

| Dimension | Provider-managed funds | Direct integrations | Evidence status |
|---|---|---|---|
| Time to first authorized integration | Could be shorter if a suitable partner already has provider access | Could require separate onboarding and contracts | Unconfirmed for HARAI |
| Control over provider-specific integration | Shared with partner | More direct technical control | General design expectation |
| Multi-merchant support | Must be expressly allowed by partner | Must be expressly approved by each provider | Unconfirmed |
| Collections | Depends on partner's supported products and contracts | Public MTN and Airtel materials describe business collection products | Access and terms unconfirmed |
| Money-out | Depends on partner's payout products, permissions and settlement arrangement | Public MTN and Airtel materials describe disbursement products | Access and terms unconfirmed |
| Refunds/reversals | Partner must expose and authorize required workflows | Each provider's exact capabilities must be verified | Unconfirmed |
| Settlement and custody | Defined by partner/provider contracts and flow | Defined by each direct provider arrangement | Unconfirmed |
| Fees and commercial viability | Partner pricing plus provider costs, where applicable | Direct pricing and negotiated terms | Unknown |
| Reconciliation | Depends on available partner reports and settlement evidence | Requires normalization across provider reports | Design requirement |
| Regulatory responsibility | Must be mapped across HARAI and partner; partnership does not automatically remove HARAI obligations | Must be assessed against HARAI's actual activities | Requires qualified review |
| Operational complexity | Potentially lower at first, but partner dependency remains | More provider-specific integration and support work | General expectation |
| Long-term flexibility | Depends on partner contract and portability | Potentially greater provider-level control | Not proven; assess commercially |

## 7. Required evidence pack

Before recommending a model, collect and compare:

1. Written provider/partner confirmation that HARAI's intended multi-merchant gateway use case is permitted.
2. Exact legal entities, roles and permissions for each party in each funds flow.
3. End-to-end funds-flow diagrams for collections, merchant settlement, payouts, refunds and reversals.
4. Production eligibility, onboarding documents, sandbox availability, technical specifications and callback authentication requirements.
5. Fee schedules, transaction limits, minimum volumes, prefunding/reserve requirements, settlement cycles, failed/returned payment fees and termination terms.
6. Settlement reports, transaction-level references and reconciliation capabilities.
7. Support hours, incident escalation, service levels, outage handling and recovery procedures.
8. Data protection, retention, audit access, dispute handling, customer complaints and liability allocation.
9. Qualified Uganda legal/compliance assessment of required licences, approvals, outsourcing/partner structure and consumer-protection obligations.
10. A weighted comparison with explicit unknowns and evidence links; unknowns must not be scored as confirmed capabilities.

## 8. Preliminary recommendation

**Do not select a final funds-flow model yet.** Continue with Model A and Model B evidence gathering in parallel. Keep Model C as a conditional design option only if it reduces meaningful dependency without creating unmanageable regulatory, reconciliation, or operational complexity.

The architecture should isolate provider adapters from the canonical HARAI transaction model, avoid representing pending/unknown provider outcomes as final, preserve immutable audit and financial records, and reconcile provider settlements. It must not assume that HARAI itself holds customer funds or maintains a stored-value wallet.


### Provider inquiry pack (send only through official business channels)

Ask MTN, Airtel, and any prospective intermediary to answer in writing:

1. Can HARAI, as a gateway intending to onboard multiple independent merchants, use the product/API under consideration? If yes, what legal entity, licence, merchant category, and contractual structure are required?
2. Is HARAI treated as a merchant, technology provider, agent, aggregator, or another category? Which party contracts with each sub-merchant and bears customer-facing payment obligations?
3. For each flow, show the payer, provider/partner account, any prefunding or reserve account, settlement account, beneficiary, and party controlling each movement of funds.
4. Can collection proceeds be settled directly to each approved merchant? If not, what alternative is allowed and what permissions are required?
5. Which exact Uganda APIs support collection initiation, callback verification, status lookup, disbursement, refund, reversal, return, and statement/report download?
6. What happens after timeout or conflicting status? Which endpoint or support process establishes finality, and what are the safe retry rules?
7. Provide all fees, taxes, limits, minimum balances/volumes, settlement windows, prefunding, reserve, returned-payment and dispute costs.
8. Provide sandbox access process, production approval criteria, test cases, callback security requirements, SLA, incident escalation and technical support contacts.
9. Provide current contract templates, data-protection terms, liability allocation, termination/portability rules, and permission to use the provider's name/logo in merchant-facing claims.
10. Confirm in writing which refunds/reversals are available, who may initiate them, the permitted time windows, and how they appear in reports.

Do not send identity documents, bank details, API secrets or other sensitive data through this planning document or a public issue. Submit formal onboarding information only through verified official provider channels and after the owner has authorized that step.

## 9. Decision gate

A recommendation can be presented only after:
- official provider/partner evidence is gathered;
- the proposed merchant and settlement model is confirmed in writing;
- fees and operational requirements can be compared;
- qualified regulatory review identifies required permissions and constraints;
- security, ledger, reconciliation and failure-recovery implications are documented.

Any final decision remains subject to explicit product-owner approval and applicable provider, legal, compliance and operational approvals. No provider onboarding, production credentials, live transactions, or payment-engine implementation is authorized by this research document.

## 10. Source limitations

This is a preliminary review of public pages available on 2026-10-19. Public pages can be incomplete or change; product access, pricing, contractual rights, legal interpretation and production eligibility have not been independently confirmed. Contact providers through their official business channels to obtain current, written evidence.
