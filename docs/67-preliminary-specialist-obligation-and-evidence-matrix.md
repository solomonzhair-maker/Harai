# DOC-67 — Preliminary Specialist Obligation and Evidence Matrix

**Status:** Preliminary source-led research; not legal advice, an accounting opinion, provider confirmation, compliance certification, or implementation approval.  
**Research date:** 2026-10-24  
**Branch:** `docs/julypay-discovery-and-requirements`  
**Purpose:** Translate the first research pass into a traceable matrix of known rules, scope limitations, unresolved evidence and required reviewers before DOC-64 is considered for approval.

## 1. Reading guide and confidence labels

- **Verified public-source text:** the cited public primary/official source states the described point. This does not itself prove the rule applies to HARAI's eventual legal role or product.
- **Preliminary interpretation:** a design consequence inferred from the source, requiring review against actual facts.
- **Not verified:** current terms, exact clause, provider account entitlement, or applicability could not be established from accessible public evidence.
- **Blocked:** do not turn this item into a production rule until the named evidence/review exists.

The matrix is deliberately conservative. No provider contact, application, contract negotiation, credentials, transaction, test, implementation, merge or deployment is authorized here.

## 2. Primary-source obligation matrix

| ID | Topic / proposition | Public authority and source | Scope and confidence | Design consequence | Required evidence / reviewer |
|---|---|---|---|---|---|
| LEG-01 | Offering a payment service, operating a payment system or issuing a payment instrument generally requires a Bank of Uganda licence, subject to the Act's stated exceptions. | National Payment Systems Act, 2020, §6–7, ULII consolidated text: https://ulii.org/en/akn/ug/act/2020/15/eng%402023-12-31 | Verified public-source text from search result. Whether HARAI's eventual activities meet a statutory definition is **not decided**. | Treat the selected business model, contracts, custody/control of funds, settlement role, customer promises and provider relationships as regulatory gates; do not infer that software-only branding or using another provider settles the question. | Qualified Ugandan payments counsel; regulatory classification memo based on actual funds flow and legal entity; written regulator/provider evidence where required. |
| LEG-02 | The Act's consumer-protection framework for PSPs includes transparency, accountability, data protection, protection against unfair trade practices, disclosure, confidentiality and dispute-resolution mechanisms. | National Payment Systems Act, §65, same ULII source above. | Verified public-source text. Application depends on the relevant entity/activity and applicable regulations. | DOC-64's complaint intake, communications, evidence and escalation controls are directionally relevant, but do not establish compliance on their own. | Counsel maps each principle to the actual role, applicable BoU instruments and customer-facing disclosures. |
| LEG-03 | BoU states it licenses/authorises payment-system providers and operators and publishes a 2025 revised oversight framework plus 2024 financial-reporting/external-auditor guidance for PSPs/PSOs. | BoU, “Strengthening Uganda's Financial Infrastructure”: https://bou.or.ug/financial_infrastructure_innovation | Official regulator landing page verified. Exact PDF text/clauses are **not yet reviewed clause-by-clause** in this pass. | Do not state the 2024 reporting guide applies to HARAI unless HARAI's classification makes it applicable. Add current framework and guideline PDFs to counsel/accountant review pack. | Retrieve and archive current official PDFs; record version/date; qualified counsel and accountant to assess applicability. |
| LEG-04 | BoU's consumer-protection page describes guidelines promoting fair and transparent treatment, disclosure, customer-data protection and effective complaint/dispute handling. | BoU, “Financial Inclusion and Consumer Protection”: https://bou.or.ug/financial_inclusion_and_consumer_protection | Official regulator page verified; the page references Consumer Protection Guidelines, but the exact guideline PDF and operative clause text were not validated in this pass. | Use the actual guidelines as a required source before approving complaint SLAs, acknowledgement scripts, escalation and customer communications. Do not derive a numeric SLA from the landing page. | Retrieve current guideline PDF from the official page; counsel records exact clause, covered institution and effective date. |
| LEG-05 | National Payment Systems Regulations, 2021 include obligations relevant to licensed PSPs such as audit-trail/MIS and data protection; outsourcing rules may require prior written BoU approval for specified licensed-service/core-operation arrangements. | National Payment Systems Regulations, 2021: https://ulii.org/en/akn/ug/act/si/2021/18/eng%402021-03-05 | Preliminary search-result evidence only; full text could not be opened in this pass. Exact regulation numbers and applicability are **not confirmed here**. | Keep audit-trail and outsourcing controls as design questions, not a final legal conclusion. No vendor/outsourcing arrangement should be declared compliant based on this summary. | Retrieve the full current instrument and amendments; counsel identifies exact regulations and role-specific application. |
| PRIV-01 | Personal data must be processed fairly/lawfully, kept adequate and not excessive, retained only for a legally or operationally justified period, and protected by security safeguards. | Data Protection and Privacy Act, 2019, §§1–3: https://ulii.org/en/akn/ug/act/2019/9/eng%402023-12-31 | Verified consolidated public text. Application and any sector-specific duties need review. | Create data inventory, purposes, minimum fields, access classifications, retention classes and owner; do not set one global retention duration. | Privacy counsel/DPO documents controller/processor roles and lawful bases for each data category. |
| PRIV-02 | Before collection, the data subject is to be told specified information including purpose, responsible party, recipients, rights and the intended retention period; data should be collected for a specific lawful purpose and be limited to necessary/relevant data. | DPPA §§12–14, same ULII source. | Verified public-source text. Notice requirements and exceptions need fact-specific review. | Intake forms and notices need privacy review; collect only evidence necessary to investigate the complaint and protect against unnecessary cross-tenant exposure. | Approved privacy notice, data inventory and field-level minimization review. |
| PRIV-03 | DPPA §18 restricts retention beyond what is necessary, subject to specified lawful, contractual and other exceptions, and provides for deletion/de-identification at expiry; §19 addresses processing/storage outside Uganda. | DPPA §§18–19, same ULII source. | Verified public-source text. Other applicable statutes/contracts and litigation holds may alter particular periods. | Retention matrix must be per record class and purpose; document legal basis, trigger, expiry, hold/suspension, deletion method and evidence. Cloud-region/cross-border storage must be assessed, not assumed compliant. | Qualified Ugandan privacy counsel/DPO; processor contracts and cloud-region/data-flow inventory; approved legal-hold procedure. |
| PRIV-04 | The Act sets security duties, processor-contract security requirements and breach-notification provisions, including immediate notification to the Authority where a responsible party believes personal data was accessed/acquired without authorization; authority determines whether data subjects should be notified. | DPPA §§20–23, same ULII source. | Verified public-source text. Exact operational reporting process and current prescribed manner require review of regulations/guidance. | Maintain incident evidence and an escalation path capable of prompt privacy/legal triage; do not hardcode an unreviewed notification timer or promise automatic user notification in every event. | DPO/security lead and counsel review current regulations, PDPO process, breach-response contacts and escalation clock. |
| PRIV-05 | The Act provides for a Data Protection Register and registration of persons/institutions processing personal data. | DPPA §§29–30, same ULII source. | Verified public-source text; current registration procedures and applicability should be checked. | Confirm registration status/obligation before launch; do not treat registration as completed without documentary evidence. | DPO/counsel checks PDPO current registration rules and records proof or reasoned applicability decision. |
| PROV-01 | MTN Uganda public payment-system rules distinguish end users from payment processors; PSPs/integrators/aggregators submit KYC for contracting; direct or indirect integration is contemplated, with permissions and security conditions. | MTN Uganda, Mobile Money Payment System Rules: https://www.mtn.co.ug/mtn-mobile-money-payment-system-rules/ | Verified official public source. Does not establish HARAI's eligibility or approved role. | Build a product/role/entitlement matrix. No API availability or right to serve multiple merchants may be inferred from the public page. | Current signed agreement, product-specific manual, approved role/permissions, onboarding evidence and account-specific written confirmation. |
| PROV-02 | MTN's public rules describe settlement only from available e-money, authorized personnel/credentials and a configured bank account; a public dispute escalation matrix is presented as a sample that processors populate according to their own resourcing. | MTN Uganda, same rules page. | Verified public source; exact contractual duties, routes and service levels for a particular account are not established. | Do not hardcode the sample contact matrix as HARAI's approved escalation route. Distinguish settlement capability from case status and authorization. | Provider-approved escalation matrix, actual named account contacts, contractual deadlines and settlement/escrow model. |
| PROV-03 | MTN's Open API page lists product capabilities such as RequestToPay, Transfer, Balance, transaction-status inquiry and account-holder validation; access is subject to onboarding/regulatory policies. | MTN Uganda Open API overview: https://www.mtn.co.ug/helppersonal/mtn-open-api/ | Public product overview only; exact product, account, environment, country and production entitlements not verified. | Treat each endpoint/action as unavailable until its current product manual and entitlement are evidenced. A technical endpoint is not a legal authorization. | Current product manual and account-specific entitlements, sandbox/production distinction, permissions, limits, error/finality rules and provider support path. |
| PROV-04 | Airtel's developer portal allows product/application workflows and OpCo selection, including Uganda, but the accessible public material reviewed here did not establish HARAI's Uganda product rights, dispute SLAs or reversal terms. | Airtel Africa Developer Portal: https://developers.airtel.africa/ | Portal existence verified; account-gated and contract-specific terms **not verified**. | Airtel provider obligations remain an open evidence item; do not import MTN rules or other-country Airtel terms. | Current Uganda-specific product manual and signed terms; account-specific written entitlement and escalation/deadline evidence. |
| FIN-01 | IFRS 15 principal-versus-agent assessment depends on the specified promise and control before transfer; indicators include responsibility for fulfilment, inventory risk and pricing discretion. | IFRS Foundation, IFRIC Update Nov. 2021: https://www.ifrs.org/news-and-events/updates/ifric/2021/ifric-update-november-2021/ and IFRS 15: https://www.ifrs.org/issued-standards/list-of-standards/ifrs-15/ | Official standard-setter materials verified as the right starting point; no conclusion on HARAI's accounting is made. | Do not choose gross vs net revenue merely because the platform routes funds. Separate customer/merchant funds, fees, settlement receivables and company revenue in the scenario analysis. | Practising accountant assesses contracts, promises, control, funds flow and applicable Uganda reporting framework; documented conclusion required. |
| FIN-02 | IAS 37 provides a framework for provisions and contingent liabilities; a refund, claim, dispute or litigation scenario may need assessment depending on whether a present obligation, probable outflow and reliable estimate exist under the standard's criteria. | IFRS Foundation, IAS 37: https://www.ifrs.org/issued-standards/list-of-standards/ias-37-provisions-contingent-liabilities-and-contingent-assets/ | Official overview verified; no accounting recognition conclusion for HARAI. | Complaint state alone must not automatically create or release a provision. Case outcomes must be supplied to finance for assessment under approved policy. | Accountant reviews representative scenarios, probability/evidence, estimation, disclosure and review cadence. |
| FIN-03 | BoU publishes financial-reporting and external-auditor guidance for PSPs/PSOs; applicability depends on whether the entity falls within the covered regulated category. | BoU infrastructure/oversight page: https://bou.or.ug/financial_infrastructure_innovation | Official publication listing verified; exact guide not yet reviewed in detail. | Keep PSP reporting requirements as a gated question; do not claim compliance or apply reporting templates until entity classification is confirmed. | Current guide and qualified accountant/counsel applicability memo. |
| SEC-01 | PCI DSS retention FAQ states that payment data should be retained only as needed for legal, regulatory or business purposes and prohibits retaining sensitive authentication data after authorization, even encrypted. | PCI SSC FAQ 1318: https://www.pcisecuritystandards.org/faqs/1318/ and PCI DSS: https://www.pcisecuritystandards.org/standards/pci-dss/ | Official PCI SSC material; applicability depends on the actual card-data and service-provider/merchant scope. | Do not store CVV or other sensitive authentication data after authorization; avoid placing card data/secrets in complaint notes/logs. PCI scope and responsibility must be confirmed with acquiring/provider parties. | Card-flow/data-flow diagram and written acquirer guidance; qualified PCI assessor where required. |

## 3. Deadline registry — current findings

| Candidate rule | What is currently supported | What is not established | Safe disposition |
|---|---|---|---|
| Provider error-report / reversal deadline | MTN consumer-facing terms have been reported publicly to contain a seven-day condition for a narrow category of erroneous consumer transfers. | Applicability to HARAI, merchants, aggregators, collections, disbursements, API transactions or contractual disputes. It must not be assumed to apply to those workflows. | Do **not** create a HARAI deadline from this consumer rule. Mark provider/API rule UNVERIFIED until the applicable contract/manual confirms it. |
| Provider response or resolution SLA | MTN's public processor matrix shows escalation categories and roles as an example. | Contract-specific acknowledgement, response, resolution, appeal or escalation timing. Airtel Uganda details also remain unverified. | No numeric external SLA; obtain the current contract/manual and account-specific escalation matrix. |
| Privacy breach response | DPPA §23 uses “immediately” for notification to the Authority where the responsible party believes personal data was accessed/acquired without authorization, and the Authority determines whether data subjects should be notified. | The operational procedure, prescribed form/channel and any details in current regulations/guidance have not been fully reviewed here. | Flag for immediate human privacy/legal escalation; do not translate “immediately” into an invented number of hours. |
| Privacy notice / retention | DPPA §13 requires notice of the intended retention period; §18 supplies the general necessity/exception/deletion framework. | Exact retention period for each HARAI record type. | Create a per-record retention decision; no universal duration. |
| Payment-service transfer timing / settlement | The NPS Act authorizes regulations addressing specific time for effecting transfers; MTN public rules describe settlement mechanics. | HARAI-specific service, product, settlement cycle, exceptions and contractual cut-offs. | No generic deadline; source each rule to applicable regulation and provider contract. |
| Legal limitation period / litigation | Not reviewed comprehensively for the specific claims, parties and contract. | Applicable claim type, accrual event, forum, contract terms, tolling and any special statutory regime. | Qualified Ugandan counsel must determine; do not use complaint SLA as a limitation period. |

All deadlines need a registry entry with source/version, jurisdiction, product, party, triggering event, time zone, calendar/business-day semantics, holiday calendar if relevant, pause/resume conditions, accountable owner, escalation route, evidence of submission, effective date and supersession history. Until approved, the UI should state **deadline rule unverified** and route for human triage rather than fabricate a due date.

## 4. Operational routing evidence matrix

| Control | Minimum evidence before it can be called operational | Current status |
|---|---|---|
| Internal case owner | Named person/team, accountable manager, working hours/coverage, backup, scope and delegation/approval authority | Not supplied/verified |
| Provider escalation | Current provider-approved channel, category-to-destination mapping, authorized sender, escalation levels, applicable SLA and evidence of delivery/acknowledgement | MTN public sample exists; not HARAI-approved. Airtel not verified. |
| Financial review | Named finance/reconciliation reviewer, separation of duties, approved correction workflow and escalation for material discrepancies | Not supplied/verified |
| Legal/compliance/privacy escalation | Named qualified reviewer or retained firm, scope of engagement, emergency route and approved authority | Not supplied/verified |
| Communication delivery | Persisted event proving the actual attempt and its result; distinguish draft, submitted, sent, delivered, acknowledged and accepted/owned | Proposed in DOC-64; no operational route verified |
| Unavailable owner / failed route | Tested fallback destination, accountable fallback owner, retry/escalation rule and audit trail | Not supplied/verified |

No case should be described as “handed off” because an email was drafted, a button clicked, or an internal ticket created. Keep routing in an unconfirmed/failed state until the relevant delivery or acceptance evidence exists.

## 5. Privacy and retention decision template

For each data class (complaint narrative, identity/KYC evidence, transaction identifiers, provider payloads, customer communications, audit events, attachments, fraud indicators, exports and backups), record:

1. Purpose and necessity; lawful basis and controller/processor role.
2. Minimum fields and sensitivity classification; permitted readers and export rules.
3. Notice provided and recipient categories.
4. Legal, regulatory, contractual and operational retention basis, each with exact source and clause.
5. Trigger date and approved duration; hold/suspension conditions and owner.
6. Storage region, subprocessors, cross-border route and security controls.
7. Data-subject request process and conflicts with legal/financial recordkeeping duties.
8. Deletion/de-identification method, backup expiry, evidence of completion and exceptions.
9. Qualified reviewer, approval date, policy version and review date.

Do not fill the duration field with an arbitrary number. Record “UNRESOLVED — counsel/DPO decision required” until supported.

## 6. Finance/accounting scenario pack required

A practising accountant should assess, at minimum, the following hypothetical flows once the real model and contracts are known:

- **Provider-managed funds:** provider receives/holds funds and settles the merchant; determine which party owes the merchant, how HARAI's fee is earned, whether any receivable exists, and how failed/late settlement is represented.
- **Direct integration / HARAI-controlled flows:** establish legal and contractual custody/control, safeguarded/escrow/trust account requirements if any, settlement liabilities, reconciliation responsibilities and whether licensing/authorization is required before activity.
- **Conditional hybrid:** specify who holds funds and is responsible at every stage; do not describe “hybrid” as a safe default without a complete funds-flow map and approvals.
- **Refund/reversal/dispute:** distinguish a customer claim, an approved proposed outcome, an authorized provider operation, a pending/unknown result, a completed refund, a receivable/recovery and any loss/provision. These are not interchangeable.
- **Provider fees, reserves and settlement breaks:** reconcile provider statements, internal ledger and bank statements; determine gross/net revenue, fee expense, receivables/payables, timing differences and write-off authority.
- **Fraud/unauthorized transaction/litigation:** assess the applicable contractual obligation, probability/measurement under IAS 37 or other applicable requirements, and disclosure/recognition only after accountant review.

Deliverables: annotated funds-flow diagram; accounting policy/framework determination; chart-of-accounts mapping; example journal entries with assumptions; reconciliation and exception ownership; provisions/contingencies decision memo; financial-statement presentation/disclosure assessment; and sign-off by a practising accountant. These deliverables are not produced or approved by this research note.

## 7. Provider-specific evidence still missing

### MTN Uganda
- Executed agreement and correct legal contracting parties.
- Product-specific current manuals and approved API list for the intended role.
- Multi-merchant/aggregator permissions and restrictions.
- Production vs sandbox access, transaction types, limits, reversals/refunds, idempotency and finality/unknown-outcome semantics.
- Contractual dispute categories, required evidence, submission channel, response/resolution SLA, escalation contacts and appeal rights.
- Fees, settlement cycle, prefunding/float, settlement accounts, reconciliation data and service credits/termination.
- Written confirmation that any public rule or API product applies to the intended account and role.

### Airtel Uganda
- Uganda-specific product/manual and executed agreement.
- Approved merchant/aggregator role and permitted collection/disbursement operations.
- API/product entitlements, environment separation, limits, transaction finality, retries/idempotency and status inquiry.
- Provider-specific dispute/reversal rules, evidence requirements, deadline calculations, support channels and escalation route.
- Fees, settlement, prefunding/float, reconciliation, privacy/security obligations and termination.
- Written account-specific confirmation.

The user's earlier informational inquiries (recorded in DOC-62/DEC-049) are not evidence of receipt, provider approval, eligibility or entitlement. This document authorizes no follow-up message.

## 8. Required next review sequence

1. Retrieve and archive the full current BoU Consumer Protection Guidelines, revised 2025 NPS oversight framework and January 2024 PSP/PSO financial-reporting/external-auditor guidelines; extract exact clauses and effective dates.
2. Retrieve the full current National Payment Systems Regulations and DPPA Regulations; verify exact regulation numbers, amendments and prescribed procedures.
3. Map the intended legal entity and each proposed product/funds-flow against statutory definitions with qualified Ugandan counsel; leave the funds-flow model unselected until the evidence is sufficient.
4. Populate the deadline registry only with applicable, sourced rules; preserve unresolved conflicts and consumer/API scope distinctions.
5. Obtain provider-specific evidence through an authorized process when separately approved; do not treat public marketing pages as contract terms.
6. Complete the data inventory and retention decision table with a DPO/privacy counsel.
7. Prepare hypothetical funds-flow/accounting scenarios and obtain practising-accountant review.
8. Update DOC-64, DOC-65, DOC-63 and relevant architecture/decision records only with traceable findings and explicit reviewer conclusions.

## 9. Current gate status

- DOC-64 remains a **draft design proposal**, not approved or implemented.
- DOC-65 remains an internal proposal review matrix.
- DOC-66 is the research programme; this DOC-67 is the preliminary obligation/evidence matrix.
- FIN-004 and SUP-005 remain accepted in scope, design not approved, not implemented and not verified.
- AR-022 remains **OPEN** pending evidence and required review.
- Funds-flow models remain unselected; G1 **NOT PASSED**; G2 **NOT APPROVED**.
- No legal qualification, provider entitlement, production access, accounting policy, named operational route or deadline has been established by this research.
- No tests, implementation, provider outreach, credentials, financial activity, merge or deployment are authorized.


## 10. Source-validation addendum — NPS consumer-protection instruments

The first pass identified an important additional primary source: the **National Payment Systems (Consumer Protection) Regulations, 2022 (S.I. 103 of 2022)**. The ULII source lists provisions on a licensee's consumer-protection policy, board and management responsibilities, consumer information, protection of consumer information/assets, complaints, inquiry handling and redress. Regulation 6 states that a licensee must, within 15 days from the last day of each month, submit to the Central Bank information about consumer complaints received, categorized and including remedial measures. This is a **monthly regulatory-reporting deadline for a covered licensee**, not a generic deadline for answering or resolving an individual complaint and not an automatic obligation for an entity whose licensed status is not established.

Primary source: https://ulii.org/en/akn/ug/act/si/2022/103/eng%402022-09-09

The 2021 National Payment Systems Regulations page also explicitly notes that amendments remain unapplied in the consolidated view. The 2022 National Payment Systems (Amendment) Regulations should therefore be reconciled against the 2021 text before describing the operative instrument as fully consolidated: https://ulii.org/en/akn/ug/act/si/2022/102/eng%402022-09-09. The 2021 Regulations' public text includes, among other things, a PSP management-information-system audit-trail requirement, personal-data duties tied to the DPPA, outsourcing controls and restrictions on cross-border payment services. Exact applicability must be assessed against the current amended text and HARAI's actual legal role.

### New deadline-registry candidate

| Rule candidate | Trigger and timing | Applicability | Required evidence / safe disposition |
|---|---|---|---|
| Monthly complaint report to BoU | Within 15 days from the last day of each month, under NPS (Consumer Protection) Regulations, 2022, reg. 6 | Only if the entity is a licensee covered by the Regulations; definitions, current directions and reporting format/channel must be confirmed | Keep BLOCKED pending qualified Ugandan counsel's applicability decision and current reporting instructions. If applicable, retain categorized complaint data, remedial measures, reviewer approval and submission receipt. Do not treat it as an individual case-resolution SLA. |

This finding should be carried into the legal obligation register and deadline registry only after the entity's licensing/applicability analysis. No regulatory report has been prepared or submitted, and this note does not authorize provider or regulator contact.
