# DOC-28 — Funds-Flow Architecture Comparison

**Status:** Draft research baseline — not a final legal, commercial, or architecture decision.  
**Decision link:** DEC-016 in DOC-00.  
**Market focus:** Uganda first.  
**Scope:** Compare provider-managed funds arrangements, direct provider integrations and a conditional hybrid for collections and money-out. Refunds and reversals are included only where contractually and technically supported.

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

## 3A. Account-specific portal evidence (2026-10-19)

This subsection supplements the public-source research above with redacted observations from owner-assisted, read-only inspection of the logged-in developer portals. Full evidence summary: [DOC-37](37-provider-portal-evidence-review-and-owner-checklist.md) and [DOC-38](38-mtn-airtel-provider-evidence-comparison.md).

| Provider | Visible account state | Technical documentation observed | Remaining blockers |
|---|---|---|---|
| MTN MoMo Uganda | Test environment; Collections and Collection Widget subscriptions shown Active; no Disbursement subscription visible; no production approval visible | Collection Request-to-Pay/status flow; Disbursement Transfer/Deposit, balance/status and refund operations; Aggregator API `transferType`; callback and reconciliation guidance | Written HARAI multi-merchant eligibility; live approval; funds/account ownership; settlement and prefunding terms; full fees, limits and support |
| Airtel Uganda | Authenticated Uganda developer portal; 0 live applications and 3 archived applications; no production approval visible | Collection v2.0 with enquiry and refund; Disbursement v2.0 B2B/B2C and direct merchant transfers; Merchant Collection with micro-merchant relationships; callbacks and Transactions Summary | Meaning/status of archived applications not investigated; production activation; written HARAI aggregator eligibility; funds/account ownership; settlement and payout funding; full fees, limits and support |

Interpretation:
- MTN's visible active subscriptions establish account-specific Test access only, not production eligibility or authority to process payments for unrelated merchants.
- Airtel's published Merchant Collection operations establish a documented technical surface, not proof that HARAI is approved to register or service sub-merchants.
- Both providers document asynchronous or ambiguous outcomes and transaction/status enquiry. HARAI requirements must preserve unknown/pending states and avoid blind retries.
- MTN callback guidance says callbacks are sent once without retry; Airtel documentation describes an authenticated callback option using HMAC-SHA256. HARAI should require secure callback verification and independent status/reconciliation recovery, while each adapter follows the final approved provider specification.
- The Airtel developer dashboard showed three archived applications, but no names or causes were retained and no archived app was opened. Do not infer that these applications belong to HARAI or can be reactivated.
- The account pages inspected did not establish fees, settlement cycles, reserves, custody/control, payout prefunding, or written contractual permission.

These observations refine the evidence register but do not change the hard-gate rule. Unknowns remain N/S (not scored); no funds-flow model is selected.

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

| Criterion | Weight | Evidence required | Model A score | Model B score | Model C score |
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
| **Total** | **100%** | Scores only after evidence review | **Not scored** | **Not scored** | **Not scored** |

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

## 6A. Three-model decision snapshot (evidence status, not scoring)

The table below makes the three alternatives comparable without assigning invented numerical scores. "Potential design advantage" is a hypothesis to test, not verified HARAI evidence. Every model remains subject to the same legal, provider, settlement, funding and reconciliation hard gates.

| Decision dimension | Model A — Provider-managed funds | Model B — Direct integrations | Model C — Conditional hybrid | Current evidence / decision |
|---|---|---|---|---|
| Contractual permission for HARAI multi-merchant model | Requires explicit intermediary/provider permission | Requires explicit provider permission for HARAI's precise role | Requires permission for each relevant relationship and clear responsibility split | Unknown for all three; hard gate |
| Collection capability for MTN and Airtel | Depends on partner's permitted product/rail coverage | Each provider relationship and product must be approved | Each rail's contract and operating path must be approved | Technical docs exist; HARAI access/permission unconfirmed |
| Money-out capability and funding source | Partner must document permitted payout route and source of funds | Each provider must approve payout product, funding arrangement and HARAI role | Funding, approvals and liabilities must be mapped per rail and model | Unknown; hard gate |
| Settlement destination, control and timing | Defined by partner contract and reports | Defined separately by provider contracts | Multiple flows may create more than one settlement pattern | Unknown; no model may be selected |
| Refund/reversal/return semantics | Depends on partner's exposed and authorized operations | Per-provider method-specific lifecycle | Requires consistent user experience over non-identical rail capabilities | Partially documented technically; contractual/product details unknown |
| Reconciliation evidence | Depends on partner transaction and settlement reports | Requires normalized evidence from each provider | Requires matching across each provider and any intermediary | Incomplete; sample reports and discrepancy process required |
| Fees and commercial viability | Potential partner fees plus underlying costs | Direct negotiated terms and operational costs | May combine both and add reconciliation overhead | Unknown; no rate assumptions |
| Regulatory responsibility | Must define HARAI/partner responsibilities; partner does not automatically remove HARAI obligations | Must be assessed against HARAI's actual activity and contracts | Must assess each role and the combined flow | Qualified Ugandan review required |
| Integration effort and operating burden | Could reduce provider-specific work if the partner truly covers required scope | More direct adapter and support work may be required | Could increase integration and operational complexity | Qualitative hypotheses only; not scored |
| Portability and dependency | Potential intermediary dependency and contract exit risk | Direct provider dependencies | Mixed dependencies and cross-rail complexity | Contractual evidence not available |
| Current disposition | **N/S — not scored** | **N/S — not scored** | **N/S — not scored** | No model selected; evidence and hard gates control |

**No preliminary winner is recommended.** Model C is not automatically more resilient merely because it combines approaches; it should be retained only if evidence shows a concrete benefit that outweighs additional contracts, failure modes and reconciliation burden.


## 6B. Decision boundary: model-independent controls vs model-specific obligations

This section separates controls that can be designed consistently across all candidate models from obligations that must not be decided until the actual funds flow is evidenced and reviewed. It does **not** select Model A, B or C, and it is not a legal or accounting conclusion.

| Topic | Model-independent direction suitable for continued design | Must remain open until the model and evidence are reviewed |
|---|---|---|
| Financial truth and audit | Preserve source evidence, immutable event history, linked corrections and traceable operation/provider references; never turn an unknown outcome into success or failure by assumption | Which party's records are authoritative for each event, settlement, return or balance; evidentiary precedence under each contract |
| Funds ownership and control | Never infer legal ownership, custody or control from the existence of a HARAI ledger, API, provider balance response or dashboard | Who receives, holds, controls, pools, directs or settles funds at every stage; whether HARAI has any regulated role or custody obligation |
| Merchant settlement | Represent settlement as a distinct evidenced process, not as a synonym for provider API success | Settlement counterparty, account owner, destination, timing, reserves, holds, deductions, failed settlement handling and liability |
| Money-out | Keep money-out disabled until the actual capability, authorized funding source, beneficiary controls, approval authority and recovery rules are evidenced | Prefunding/reserve/available-funds model, who may instruct the provider, balance authority, limits, fees, returned-payment treatment and who bears loss |
| Refunds, reversals and returns | Model each as a separate linked operation; preserve the original transaction and the reason/evidence for corrections | Which party can initiate each operation, eligibility/window, funding/accounting effect, finality, fees and dispute responsibility |
| Ledger and reconciliation | Keep technical records, provider events, settlement evidence and accounting decisions distinct; route missing/conflicting evidence to an owned exception | Account chart/posting schedule, fee/tax/rounding treatment, merchant payable/receivable semantics, reserve/clearing accounts, report sources and close rules |
| Merchant and provider authority | Apply capability-specific default-deny and verify actual scope before activation | Exact merchant/provider contracting relationship, sub-merchant permissions, production access and legal/regulatory prerequisites |
| Hybrid route | Require the same per-flow evidence and control gates as a single-model route | Whether a hybrid creates a concrete benefit that outweighs multiple contracts, inconsistent rail behavior and additional reconciliation/operational burden |

### Current decision disposition

**What can proceed now:** retain the cross-model safety requirements above as design constraints; keep provider adapters and HARAI's internal transaction records conceptually separate; document payer, merchant, provider/partner, funding, settlement and beneficiary roles explicitly in future flow diagrams; and keep every unverified capability blocked.

**What cannot be selected from current evidence:** the final operating/funds-flow model; account and settlement ownership; custody or safeguarding responsibilities; direct versus intermediary contracting; provider/merchant-of-record roles; money-out funding and available-balance authority; accounting/posting examples; legal classification; pricing, reserves, liability and settlement SLAs.

### Evidence and decision ownership

Before a recommendation is submitted, the evidence pack in §7 must be reviewed together by:
- the product owner, to confirm the intended commercial/product role;
- a qualified Uganda legal/compliance adviser, to assess the actual activity and contractual structure;
- a qualified finance/accounting reviewer, to approve model-specific account semantics and postings;
- the provider-integration owner, to establish actual provider/partner capability and contractual scope;
- an operations/reconciliation owner, to confirm evidence availability, exception handling and supportability.

These are role requirements, not appointments of named people. Provider outreach remains subject to separate explicit authorization. Where an evidence source or role owner is missing, record the gap; do not score the model favorably or substitute a generic architecture pattern for the missing decision.

**Gate effect:** AR-002 remains OPEN. G2 and affected money-moving capabilities remain blocked until the model-specific flow and posting schedule are reviewed with the required evidence. G1 remains NOT PASSED; G2 remains NOT APPROVED.

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

This document combines public-source research and redacted, account-specific read-only portal observations available on 2026-10-19. Public pages can be incomplete or change; product access beyond the visible Test/dashboard state, pricing, contractual rights, legal interpretation and production eligibility have not been independently confirmed. Contact providers through their official business channels to obtain current, written evidence.
