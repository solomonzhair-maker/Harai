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


## 11. Source-validation addendum — Bank of Uganda 2025 framework and reporting materials

### 11.1 Revised National Payment Systems Oversight Policy Framework (August 2025)

Official PDF: https://bou.or.ug/uploads/Revised_BOU_National_Payment_Systems_Oversight_Framework_2025_698b3e9745.pdf

The current official PDF was retrieved and reviewed. Key findings relevant to classification and design:

- The framework states that the National Payment Systems Act and implementing regulations form the legal basis for oversight and identifies related laws including the Electronic Transactions Act, Computer Misuse Act, Contracts Act, Electronic Signatures Act, Data Protection and Privacy Act and Anti-Money Laundering Act (PDF pp. 4–5).
- It states that PSPs and PSOs must be licensed by BoU to operate in Uganda, subject to the Act and applicable exceptions (pp. 5–6).
- The framework distinguishes PSPs that deal directly with merchants/consumers and handle funds/acquire transactions from PSOs that process transfers, aggregation or enabling technology without consumer-facing services (pp. 8–10). This is a regulator's framework description, not a determination of HARAI's classification.
- It says a PSP needs explicit approval for additional services such as cross-border transfers and product partnerships with other licensed entities (p. 9).
- It says PSPs/PSOs retain full responsibility for material activities even where third-party providers are contracted, including robust third-party risk management and governance (pp. 9–10).
- The framework lists domestic money transfer/remittance services, including payment gateway services, and merchant acquisition among eligible payment services/activities; it states cross-border transfer/remittance requires prior BoU approval/licence (p. 16).

**Design consequence:** HARAI must not assume that being a software platform or relying on MTN/Airtel removes licensing or oversight questions. A qualified Ugandan payments lawyer must map the intended legal entity, customer-facing role, aggregation, transaction processing, funds handling, merchant acquisition, settlement and cross-border plans against the Act, regulations and framework. No legal classification is concluded here.

### 11.2 Consumer-protection guidelines — source status

Official BoU page: https://bou.or.ug/financial_inclusion_and_consumer_protection

The current page describes the Consumer Protection Guidelines as addressing fair and transparent treatment, product terms/cost disclosure, fair marketing, customer-data protection and effective complaint/dispute handling. The linked guideline document itself was not retrievable as a standalone authoritative PDF in this pass. Therefore the exact operative provisions, covered institution types, complaint timelines, and relationship to the 2022 NPS Consumer Protection Regulations remain **not verified clause-by-clause**. Do not derive a numeric complaint SLA from this landing page. The 2022 NPS Consumer Protection Regulations remain the more directly identified primary source for covered licensees, subject to counsel's applicability review.

### 11.3 Financial Reporting and External Auditor Guidelines for PSPs and PSOs — source status

Official BoU infrastructure page: https://bou.or.ug/financial_infrastructure_innovation

Historical note from the initial retrieval pass. Both official November 2023 and January 2024 PDFs were subsequently inspected via rendered pages; see §17 for the comparison and the remaining legal-version/applicability limitations. The 2025 framework does not substitute for the financial-reporting guideline.

**Required next step:** obtain qualified confirmation of which guideline version controls, whether any transition/circular changes the wording, and whether HARAI's actual legal role falls within the covered categories. See §17. Do not use either PDF to generate live reporting returns before that review.

### 11.4 Revised legal applicability and routing controls

| Finding | Consequence | Required reviewer/evidence |
|---|---|---|
| A payments platform/aggregator or enabling technology may fall within the framework's PSO/PSP descriptions depending on its actual activities and role. | Licensing classification remains a pre-design gate, not a late launch checklist. | Written counsel memo using actual funds-flow diagram, customer contracts, provider contracts and service scope. |
| Outsourcing critical technical or aggregation work does not by itself transfer away a covered PSP/PSO's material operational responsibility. | If HARAI is a covered regulated entity, provider/processor due diligence, contractual allocation, monitoring, incident escalation and continuity need review. | Counsel/compliance review of applicable outsourcing rules and all relevant contracts. |
| BoU's guideline page provides broad principles but the detailed guideline PDF was not verified. | Do not hardcode unverified complaint SLAs or claim clause-level compliance. | Obtain and archive the actual guideline with version/effective date and extract relevant clauses. |
| The January 2024 PSP/PSO financial reporting guideline is listed but its full text was not reviewed. | No claim of compliance and no final accounting/reporting design based on the listing alone. | Official PDF plus practising accountant's documented applicability and accounting-framework review. |

This addendum supersedes any implication elsewhere in DOC-67 that the detailed 2025 oversight framework or January 2024 reporting guideline had already been reviewed clause-by-clause. All conclusions remain preliminary and role-dependent.


## 12. Additional source-validation addendum — PDPO annual compliance report

**Official sources reviewed on 2026-10-24:**
- PDPO, Guidance Note on Completion of the Annual Data Protection and Privacy Compliance Report: https://pdpo.go.ug/media/2024/01/Guidance-Note-on-Completion-of-the-Annual-DPP-Compliance-Report.pdf
- PDPO Knowledge Center, current notices: https://pdpo.go.ug/knowledge-center

The PDPO guidance note states that regulation 50 of the Data Protection and Privacy Regulations, 2021 requires each registered data collector, controller and processor to submit a summary within 90 days after the end of each financial year. The report covers complaints and their status, plus data breaches and actions taken. The same guidance says the PDPO expanded the report's scope using regulation 4(b). The PDPO Knowledge Center currently lists a notice updated 25 August 2026 requiring registered data controllers, collectors and processors to submit annual compliance reports by **23 November 2026**.

**Applicability warning:** this is a current public notice, not evidence that HARAI is registered, that its reporting status is known, or that a particular report is due from it. The applicability of regulation 50, the current notice, reporting period, template, filing channel, and the effect of the stated scope expansion must be confirmed against the notice itself and the organization's registration/status by the DPO/privacy counsel. Do not submit anything or contact PDPO based on this research note alone.

| Candidate obligation | Timing / content | Applicability / status | Safe design consequence |
|---|---|---|---|
| PDPO Annual Data Protection and Privacy Compliance Report | Regulation 50 guidance describes submission within 90 days after financial year end; report includes complaints/status and breaches/actions. Current PDPO Knowledge Center notice lists 23 November 2026. | **CONDITIONAL — NOT YET DETERMINED**. Must verify whether HARAI is a registered data collector/controller/processor and whether the current notice applies to its reporting cycle. | Add to compliance calendar only after status, reporting period, current notice and required format are confirmed. Keep complaint and breach records exportable with data minimization and access control. Preserve submission receipt if later applicable. |

This item is separate from the NPS (Consumer Protection) Regulations, 2022 monthly complaint report. The PDPO annual report is a data-protection compliance report for covered registered organisations; the BoU monthly complaint report is conditional on being a licensee covered by the NPS consumer-protection instrument. They must not be conflated.

## 13. National Payment Systems amendment reconciliation — scope verified

The official ULII page identifies the 2022 National Payment Systems (Amendment) Regulations (S.I. 102 of 2022) as amending the 2021 Regulations. The published amendment text states that it substitutes Schedules 3 and 4 (fees and minimum capital requirements). It does not, in that instrument, replace the main body of the 2021 Regulations wholesale. The ULII consolidated 2021 Regulations page continues to warn that outstanding amendments have not been applied in the consolidated view.

Sources:
- 2021 Regulations: https://ulii.org/en/akn/ug/act/si/2021/18/eng%402021-03-05
- 2022 Amendment Regulations: https://ulii.org/en/akn/ug/act/si/2022/102/eng%402022-09-09
- Official published amendment PDF: https://media.ulii.org/media/legislation/74618/source_file/611d937d3b6fefb1/ug-act-si-2022-102-publication-document.pdf

The 2021 Regulations text includes regulation 11(2)(d) describing a third-party systems class including an aggregator, integrator or gateway facilitating electronic payments without first setting up a merchant account; regulation 25 requires a PSP's management information system to support audit trails and personal-data handling in accordance with the DPPA; regulation 28 addresses outsourcing of licensed services, core payment-system operations or technical personnel and written BoU approval; regulation 29 addresses prior written BoU approval for cross-border payment services. These are **source-text findings, not a conclusion that any specific provision applies to HARAI**. Counsel must check all later instruments, circulars, licence conditions, amendments and current official directions before final legal mapping.

## 14. Reviewer-pack research status and remaining retrieval gaps

- Retrieved: official BoU 2025 NPS Oversight Policy Framework PDF and reviewed relevant sections; official ULII 2021 NPS Regulations text; official 2022 NPS Amendment instrument; official 2022 NPS Consumer Protection Regulations text; PDPO annual compliance report guidance and current Knowledge Center notice.
- Retrieved and reviewed: the full 2011 BoU Financial Consumer Protection Guidelines PDF (legacy BoU-hosted copy; current status/applicability still requires counsel confirmation) and both November 2023 and January 2024 PSP/PSO financial reporting PDFs via rendered pages. The previously missing November printed page 5 was retrieved and compared; see §17.1. This remains a targeted comparison, not a complete textual redline.
- Still not completed: formal counsel-confirmed current NPS instrument/version analysis; exhaustive specialist review of all DPPA Regulations, 2021 provisions against HARAI's actual processing. The November/January financial-reporting comparison and targeted DPPA review were added later in §17; see the explicit scope limitation there.
- Consequently, do not claim those missing documents have been fully reviewed. Public page listings prove publication only, not detailed obligations.


## 15. BoU Financial Consumer Protection Guidelines, 2011 — full text retrieved; current applicability unresolved

**Source reviewed:** Bank of Uganda, *Financial Consumer Protection Guidelines, 2011*, PDF hosted on the legacy BoU website: https://bouganda.com/bouwebsite/bouwebsitecontent/Financial_Literacy/Guidelines/2011/Jun/Consumer_Protection_Guidelines_June_2011.pdf. The current BoU financial-inclusion page continues to list a Consumer Protection Guidelines resource and describes disclosure, fair marketing, customer-data protection and complaint/dispute handling, but its modern page renderer did not expose the download URL. The 2011 PDF is therefore a retrieved published version, not proof that no later instrument or direction modifies it.

Relevant text in the retrieved 2011 PDF:
- Paragraph 1: effective 1 June 2011.
- Paragraph 2: application to BoU-regulated financial-services providers and their agents for business in Uganda.
- Paragraph 3 defines “financial services provider” as a bank, credit institution, microfinance deposit-taking institution, forex bureau or money-remittance company regulated by BoU. This definition matters: do not automatically extend the guideline's provisions to a standalone PSP/PSO or technology gateway without counsel mapping the relevant definitions and later NPS instruments.
- Paragraph 9(2)–(4): covered providers should have documented effective complaint procedures, make them available through communication channels, and investigate/determine complaints competently, promptly and impartially, provide appropriate redress and explain decisions clearly.
- Paragraph 9(5): prompt written acknowledgement on receipt, except where the complaint is resolved within three business days; “resolved” depends on the complainant accepting the response.
- Paragraph 9(6): final response by the end of two weeks after receipt, with additional time beyond a week excluded where the complainant takes more than a week to answer a written request for information.
- Paragraph 9(8): six-monthly complaint-handling reports to BoU, covering each half-year and due within one month after the relevant period, subject to the paragraph's stated exclusion for complaints resolved by the end of the business day after receipt.
- Paragraph 9(7): identify and remedy recurring/systemic complaint causes.

**Critical reconciliation:** the 2022 NPS (Consumer Protection) Regulations have a separate monthly complaint reporting rule for licensees within their scope. Do not silently assume the 2011 six-monthly report applies to HARAI, that the 2022 rule supersedes it for every institution, or that one reporting requirement replaces the other. Qualified counsel must identify the covered entity class, later amendments/directions, interaction between instruments and exact current deadlines. The two-week final-response rule is a candidate from the 2011 guideline for the entities/relationships to which it applies; it is not a universal HARAI or provider SLA and must not be hard-coded as such before legal validation.

| Candidate requirement | Source text | Status for HARAI | Design implication |
|---|---|---|---|
| Complaint acknowledgement | 2011 Guidelines para. 9(5): prompt written acknowledgement, exception if accepted resolution within 3 business days | Applicability unresolved; definition of provider and subsequent instruments require counsel review | Support configurable acknowledgement targets and record acknowledgement evidence; do not label the target as universally statutory |
| Final response | 2011 Guidelines para. 9(6): end of two weeks, with stated pause/exclusion for delayed consumer response | Applicability unresolved; interpret calendar/business-day calculation and relationship with NPS Regulations with counsel | Model clock start, pause evidence and deadline provenance; keep legal deadline configurable until approved |
| Complaint reporting | 2011 Guidelines para. 9(8): six-monthly report due within one month after each half-year, subject to stated exclusion | Applicability and interaction with NPS 2022 monthly reporting unresolved | Keep reporting rules separate by authority, licence/category and reporting period; never merge into one generic “regulatory report” |
| Systemic complaint analysis | 2011 Guidelines para. 9(7) | Conditional on applicability | Preserve complaint categorization/root-cause trends and evidence of corrective actions |

Source evidence is reproduced only as summarized findings, not as a legal opinion. The source PDF is an archived/legacy BoU-hosted URL; counsel should confirm current status, applicability and any superseding circulars/guidelines before relying on it.


## 16. January 2024 BoU Financial Reporting and External Auditor Guidelines — text visually reviewed

**Source:** Bank of Uganda Guidelines on Financial Reporting and External Auditors for Payment Service Providers, Payment System Operators and Issuers of Payment Instruments, 10-page PDF hosted on the legacy BoU website: https://bouganda.com/bouwebsite/bouwebsitecontent/PaymentSystems/Legal-Frameworks/Financial-Reporting-and-External-Auditor-Guidelines-for-PSPs-and-PSOs-Issued-January-2024.pdf. The PDF text was reviewed via its rendered pages. The guidelines state that they commence on 1 December 2023 (clause 1(b)) and apply to all PSPs, PSOs and issuers of payment instruments regulated by BoU in respect of business transacted in Uganda (clause 2). This is a conditional regime: HARAI's category/licence status has not been established.

### Requirements stated in the January 2024 PDF
- **Quarterly financial statements (clause 4(a)):** income statement and statement of financial position; unverified statements due to BoU by the 15th day of the month following quarter-end; statements verified by the external auditor and submitted under cover letter by the 30th day of the month following quarter-end; standard Appendix 1 template.
- **Annual financial statements (clause 4(b)):** use IFRS as adopted by the Institute of Certified Public Accountants of Uganda; align financial year to January–December; draft audited financial statements and management letter to BoU for review by 31 March after year-end, after Board consideration; BoU reviews and gives written approval before finalization/publication; extracts from signed audited statements published in a newspaper of wide circulation by 30 April; audited statements displayed on the licensee's website.
- **External auditor (clauses 5–6):** appoint an auditor from BoU's prequalified list; annual audit plus quarterly review; continuous appointment not exceeding three years; seek BoU approval before appointment and do not change auditors without prior written BoU approval.
- **Audit/reporting responsibilities (clauses 7–10):** auditor reports on compliance with NPS law/regulations and other listed laws/standards; assesses internal controls, risk management, information systems and report reliability; verifies periodic returns; has access to books, management information systems and records; draft audited statements and management letter must be submitted to BoU for review by 31 March each year. The Central Bank may reject work and call for a fresh audit at the licensee's expense.
- **Appendix 1:** quarterly template includes income, direct operating costs, operating expenses, trust-account management costs, assets, trust account balances, liabilities and electronic-money deposits among other categories. These are source template labels, not a determination that each category is applicable to HARAI.

### Applicability and version caution
Historical note from the initial review pass. Both PDFs have since been located and partially compared; see §17.1. The November PDF's printed page 5 did not render, so the comparison is not a complete redline and the relationship/current controlling version remains unresolved. The guidelines apply to regulated PSPs, PSOs and issuers; they do not by themselves classify HARAI or prove that HARAI is licensed/covered. A qualified accountant and payments counsel must confirm current version, applicability, reporting calendar, audit requirements and any later directions before creating a live compliance calendar or reporting policy.

| Candidate obligation | Source | Status for HARAI | Required next evidence |
|---|---|---|---|
| Quarterly unverified statements by day 15 after quarter end | Jan 2024 Guidelines cl. 4(a)(ii) | Conditional on regulated PSP/PSO/issuer status; not assumed | Counsel confirms category/current version; accountant confirms calendar and template |
| Auditor-verified quarterly statements by day 30 after quarter end | cl. 4(a)(ii) | Conditional | Confirm applicable audit/review scope, submission method and due-date calculation |
| Annual statements and management letter by 31 March | cl. 4(b)(iii) and cl. 9(a) | Conditional | Confirm reporting period, Board approval sequence and exact filing package |
| BoU written approval before finalization/publication; newspaper extract by 30 April; website publication | cl. 4(b)(iv)–(v) | Conditional | Confirm current wording, approval procedure and publication requirements with accountant/counsel |
| Auditor prequalification, prior approval, quarterly review and three-year limit | cls. 5–6 | Conditional | Verify current BoU approved list/process and any subsequent circulars |
| January 2024 vs November 2023 version relationship | Official BoU page lists both; rendered-page comparison recorded in §17.1 | UNRESOLVED — legal effect/current wording not established | Qualified counsel/accountant to confirm controlling version, later directions and applicability |


## 17. Updated source comparison and targeted DPPA review — 2026-10-24

### 17.1 BoU financial reporting versions

The official BoU page lists both documents:
- [November 2023 PDF](https://bou.or.ug/uploads/Financial_Reporting_and_External_Auditor_Guidelines_for_PS_Ps_and_PS_Os_Issued_November_2023_cb4924312f.pdf) (11 pages).
- [January 2024 PDF](https://bou.or.ug/uploads/Financial_Reporting_and_External_Auditor_Guidelines_for_PS_Ps_and_PS_Os_Issued_January_2024_b1fe168e0e.pdf) (10 pages).
- [Official BoU listing](https://bou.or.ug/financial_infrastructure_innovation).

Both copies state commencement on 1 December 2023 and cover BoU-regulated PSPs, PSOs and issuers of payment instruments for Uganda business. The visible rendered-page comparison established these differences:

| Topic | November 2023 copy | January 2024 copy | Status / consequence |
|---|---|---|---|
| Annual newspaper publication, cl. 4(b)(iv) | Extracts of signed audited statements in **at least two** newspapers of wide circulation by 30 April | Extracts in **a newspaper** of wide circulation by 30 April | Wording changes the stated count from at least two to one; obtain confirmation of controlling version before relying on either. |
| Website publication, cl. 4(b) | No separate website-display clause in the corresponding annual-statement text inspected | Expressly requires audited statements to be displayed on the licensee's website, cl. 4(b)(v) | Additional express publication requirement in January text. |
| Auditor appointment/approval | Printed page 5 includes annual BoU bid call, written application deadline of 30 September, and clause 6 approval/conditional approval/decline, withdrawal for sufficient cause, and prior written approval before changing auditor | Same core provisions are present on January printed page 5 | **No substantive difference identified on this page**. These controls must not be described as January additions. Remaining differences elsewhere require full redline and specialist confirmation. |
| Auditor annual reporting package, cl. 9(a) | Refers to audit report and management letter with the related financial statements by 31 March | Refers to draft audited financial statements and management letter for BoU review with related statements by 31 March | Wording/process difference; accountant/counsel must confirm exact package and whether other provisions still require the audit report. |
| Auditor duties, cl. 7(a) | Lists frameworks with an explicit Companies Act reference and a different formulation of standards | Explicitly mentions IFRS as adopted by ICPAU and refers to other relevant laws/regulations in a different formulation | Scope wording differs; reconcile against current applicable law and auditor engagement. |
| Shared core | Quarterly statements (unverified by day 15; verified by day 30), annual IFRS/ICPAU framework, January–December year, draft annual statements/management letter by 31 March, publication by 30 April, three-year continuous auditor limit and audit/access/supervisory duties | Same overall framework, with differences above | Core reporting architecture appears shared; not a legal conclusion about supersession. |

**Version conclusion:** neither PDF states in the reviewed text that the January copy expressly repeals or supersedes the November copy. Both state the same commencement date. The official page lists both, so mark “controlling/current wording” **UNVERIFIED — SPECIALIST CONFIRMATION REQUIRED**. Do not turn differences into an implemented policy or calendar. Applicability is conditional on HARAI's actual regulated role, which is not established.

### 17.2 Privacy obligations and evidence map

Primary sources: [PDPO official Regulations PDF](https://pdpo.go.ug/media/2022/03/Data_Protection_and_Privacy_Regulations-2021.pdf), [ULII Regulations record](https://ulii.org/en/akn/ug/act/si/2021/21/eng%402021-03-12), [DPPA consolidated Act](https://ulii.org/en/akn/ug/act/2019/9/eng%402023-12-31), [PDPO annual report guidance](https://pdpo.go.ug/media/2024/01/Guidance-Note-on-Completion-of-the-Annual-DPP-Compliance-Report.pdf).

| Candidate obligation | Source | Status for HARAI | Evidence/action needed before closure |
|---|---|---|---|
| Registration and any exemption | DPPA Regulations 2021 regs. 13–15 | Applicability/status unknown | Confirm entity, data collector/controller/processor roles, registration record, renewal status and any valid exemption with privacy counsel/DPO/PDPO evidence. |
| Security practices and security measures | Regulations regs. 31–32; DPPA security duties | Conditional on actual processing role; design not verified | Data inventory, risk assessment, technical/organisational control evidence, accountable owner and independent review. |
| Breach notification | DPPA s.23; Regulations reg. 33(1)–(3) | Conditional; incident workflow not legally validated | Confirm “immediately” trigger and all recipients/notices with counsel; maintain breach type/data/categories and approximate data-subject counts, consequences, remedies and DPO/contact details; Form 7 source. |
| Data-subject complaint handling | Regulations reg. 40; complaints to Office under reg. 41 | Conditional; distinct from payment complaint workflow | Document accessible privacy complaint intake, case ownership, response/evidence and escalation, with counsel-approved clocks. |
| Cross-border processing/transfer | DPPA s.19; Regulations reg. 30 | Actual locations/subprocessors unknown | Inventory primary hosting, backups, logs, support access and subprocessors; document legal basis/adequacy/consent and onward-transfer analysis. |
| Retention/deletion | DPPA s.18 and relevant sector/contract rules | No universal duration established | Per-data-category purpose, trigger, source of legal/contractual retention, legal hold, deletion/de-identification and verification evidence. |
| Annual PDPO compliance report | Regulations reg. 50; PDPO guidance | Conditional on registered role; no filing status assumed | Confirm registration, reporting period/template/channel and due date with current PDPO guidance. Guidance states 90 days after Government FY (1 July–30 June); report includes complaint status and breaches/actions. |

This is a targeted source review, not a claim that every provision of the Regulations has been exhaustively audited or that HARAI is registered/covered.

### 17.3 Complaint reporting: do not merge obligations

| Regime | Reporting clock / recipient | What it covers | Important distinction |
|---|---|---|---|
| BoU Financial Consumer Protection Guidelines 2011, para. 9(8) | Six-monthly; due within one month after each half-year to BoU, with stated next-business-day resolution exclusion | Complaints within the guideline's defined provider/agent scope | Scope is not automatically every PSP/PSO/gateway; current status requires counsel confirmation. |
| NPS Consumer Protection Regulations 2022, reg. 6 | Monthly; within 15 days after month-end to Central Bank | Complaints by consumers, categorized by category and agent location, including remedial measures | Regulatory reporting deadline, **not** an individual resolution SLA. |
| DPPA Regulations 2021, reg. 50 | Annual; within 90 days after Government FY to PDPO | Privacy complaints/status and personal-data breaches/actions | Different regulator, period, data fields and legal trigger; separate from NPS reporting. |

**No replacement or equivalence assumed.** Counsel must determine whether multiple duties apply concurrently, whether any later instrument modifies the 2011 guideline, and what is applicable to the actual HARAI entity/activity.

### 17.4 Remaining open evidence and gates

- No formal regulator confirmation of which BoU guideline version controls has been obtained.
- No qualified Ugandan legal opinion or accountant review has been obtained.
- HARAI's licence/legal category, data registration status, funds flow, provider contract terms and cross-border data locations remain unverified.
- No operational compliance calendar or filing package is approved.
- DOC-64 remains a draft proposal; DOC-65 remains preliminary; AR-022 remains open; FIN-004/SUP-005 remain unverified; G1 NOT PASSED; G2 NOT APPROVED.
