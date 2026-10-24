# DOC-66 — Specialist Research Programme and Authoritative Source Register

**Status:** Research baseline and learning plan; not a professional qualification, legal opinion, provider confirmation, accounting opinion, or approval to implement.  
**Research started:** 2026-10-24  
**Branch:** docs/julypay-discovery-and-requirements  
**Purpose:** Establish a traceable, source-led knowledge base before finalizing DOC-64 case-management rules for provider disputes, Ugandan legal/compliance duties, deadlines, operational routing, retention/privacy, and finance/accounting.

## 1. Important competence boundary

Reading legislation, standards, books and public webpages can improve technical understanding but does not make the researcher a licensed Ugandan lawyer, practising accountant, regulated compliance officer, PCI Qualified Security Assessor, Bank of Uganda-authorized payment service provider, or MTN/Airtel contract holder. Findings in this document are research notes for design and review. Legal interpretation, formal accounting policy, regulatory classification and contractual entitlements must be confirmed by appropriately qualified and accountable people with access to the actual facts and agreements.

Use the source hierarchy below. Do not convert a generic public webpage into a product-specific contract right, assume that a rule for consumers applies to an aggregator/processor, or invent a deadline because it seems operationally reasonable.

## 2. Research method and evidence hierarchy

For each material proposition, record: claim; source title and URL; issuing authority; jurisdiction/product/scope; publication or effective date; exact section/clause; retrieval date; confidence; conflicts/limitations; required reviewer; and design consequence.

Evidence priority:
1. Current Ugandan legislation and regulations from authoritative repositories, plus applicable Bank of Uganda directives and licence/approval conditions.
2. Current signed provider/processor agreements, approved product-specific manuals, written account-specific provider responses and documented role/entitlement assignments.
3. Official provider system rules, developer documentation, consumer terms and privacy notices, checking which customer/product/channel they govern.
4. Official regulator guidance, standards bodies and accounting standard-setters.
5. Recognized professional textbooks and specialist commentary as interpretive background, not substitutes for primary authority.
6. Secondary articles only to discover questions or locate primary sources; never use them alone to set consequential deadlines or financial treatment.

When sources conflict, preserve the conflict and stop the affected decision. Do not silently select whichever source is easiest to implement.

## 3. Six research tracks

### Track A — Provider-specific dispute and payment-operation obligations
Research:
- MTN Uganda Mobile Money Payment System Rules, including participant roles, access/permissions, irrevocability, settlement, security and dispute escalation.
- MTN Open API/Collections product details and the account-specific contract/manual applicable to HARAI's prospective role.
- Airtel Africa developer portal selected for Uganda; current API product manuals, onboarding/role terms, support/escalation and applicable contract.
- Provider-specific rules for collection, transaction-status inquiry, reversal/refund, payout/disbursement, settlement, chargebacks (where relevant), complaints and evidence.
- Distinguish end-user consumer terms from merchant, processor, aggregator and API contract rules.

Decision outputs:
- A product/channel/role matrix listing each operation, permitted actor, prerequisite entitlement, authoritative status source, finality/unknown-outcome treatment, escalation channel, and written evidence.
- A provider question list for unresolved details. Sending follow-up questions is not authorized by this research document.

Initial authoritative sources:
- MTN Uganda, Mobile Money Payment System Rules: https://www.mtn.co.ug/mtn-mobile-money-payment-system-rules/
- MTN Uganda, Open API overview: https://www.mtn.co.ug/helppersonal/mtn-open-api/
- MTN Uganda, Collections Widget product details (developer portal): https://momodeveloper.mtn.com/Uganda_Widget_productDetails
- Airtel Africa Developer Portal (select Uganda; exact API documentation may require portal access): https://developers.airtel.africa/

Preliminary observation (not a product-specific entitlement): MTN's public payment-system rules describe direct or indirect integration, KYC/contracting, client letters for indirect integration where applicable, role-based permissions, transaction limits, security requirements, settlement to a configured bank account, and an example dispute escalation matrix. The rules also discuss cases where a transfer is irrevocable. This does not establish HARAI's eligibility, account permissions, live access, refund authority, or exact SLA. The public matrix itself says payment processors fill it based on their internal resourcing. Confirm applicable contractual terms and account-specific routes before design approval.

### Track B — Uganda legal, regulatory and consumer-protection review
Research:
- National Payment Systems Act, 2020 (current consolidated version and amendments).
- National Payment Systems Regulations, 2021 and relevant Agents Regulations, Sandbox Regulations, licensing requirements, and current Bank of Uganda oversight framework.
- Bank of Uganda Consumer Protection Guidelines and any applicable complaint-handling/dispute-resolution requirements.
- Anti-Money Laundering Act and applicable regulations, but only to determine obligations that attach to the actual entity/activity and accountable-person status.
- Applicable contract, consumer, electronic-transactions, evidence, limitation and sector rules as identified by qualified Ugandan counsel after the actual funds-flow and operating model are known.

Decision outputs:
- A counsel-reviewed applicability memo describing the actual legal entity, role, funds flow, customers, jurisdictions, licences/approvals, consumer-facing promises and complaint obligations.
- A clause-by-clause obligation register with applicability rationale and effective date.
- A list of questions that cannot be resolved until the provider contract and funds-flow model are selected.

Initial authoritative sources:
- National Payment Systems Act, 2020, ULII consolidated text: https://ulii.org/en/akn/ug/act/2020/15/eng%402023-12-31
- National Payment Systems Regulations, 2021, ULII: https://ulii.org/en/akn/ug/act/si/2021/18/eng%402021-03-05
- National Payment Systems (Agents) Regulations, 2021, ULII: https://ulii.org/en/akn/ug/act/si/2021/19/eng%402021-03-05
- Bank of Uganda payment-system oversight/licensing and regulations: https://bou.or.ug/financial_infrastructure_innovation
- Bank of Uganda consumer-protection information: https://bou.or.ug/financial_inclusion_and_consumer_protection
- Anti-Money Laundering Act, ULII consolidated text: https://ulii.org/en/akn/ug/act/2013/12/eng%402025-02-14
- Anti-Money Laundering Regulations, 2015, ULII: https://ulii.org/sw/akn/ug/act/si/2015/75/eng%402022-02-11

Preliminary observation: the National Payment Systems Act defines payment-service roles, including aggregator, and places payment-service provision within a Bank of Uganda licensing/oversight framework. The regulations include data-management/audit-trail and data-protection duties for licensees. The legal status and obligations of HARAI cannot be inferred from the product concept alone; the actual business model and activities require counsel/regulatory analysis. AML obligations apply according to statutory role and activity, not merely because a software system handles a complaint.

### Track C — Deadline and escalation-rule registry
Research:
- Each applicable statute/regulation, provider contract, product manual, payment-scheme rule, complaint policy, service-level commitment and limitation period.
- Start event; calendar vs business days; timezone; holidays; pause/resume conditions; required acknowledgement; response vs resolution deadlines; appeal/escalation window; evidence of submission; changes/supersession.
- Separate external mandatory deadlines, contractual service levels and internal operational targets.

Decision outputs:
- A versioned deadline registry. Each rule must carry source/section, jurisdiction/product/party, trigger event, calculation semantics, owner, escalation path, reviewer, effective date and supersession history.
- Unknown deadlines remain explicitly UNVERIFIED and trigger safe escalation; they must never be silently treated as zero, unlimited, or guessed.
- Distinguish the deadline to report a transaction error, the deadline for a provider to respond, the deadline to settle funds, an appeal window, and a limitation period. They are not interchangeable.

Initial reference:
- MTN public Mobile Money Payment System Rules (provider processor escalation framework): https://www.mtn.co.ug/mtn-mobile-money-payment-system-rules/
- Uganda legal sources listed in Track B.

Caution: public MTN consumer terms may contain a specific rule (for example, a seven-day reversal-claim condition for certain erroneous transfers) for a particular consumer service. Do not transplant such a rule into HARAI's merchant/API complaint workflow without proving its applicability from the relevant contract/manual and qualified review.

### Track D — Accountable operational routing and case governance
Research:
- Financial-services incident/complaint management, RACI/accountability, queue ownership, maker-checker separation, escalation and business-continuity practices.
- Provider's actual escalation matrix and the proposed operator's named support, finance, reconciliation, compliance and security roles.
- Evidence standards for assignment, acknowledgement, attempted contact, successful delivery, accepted handoff, escalation and closure.

Decision outputs:
- A case-routing RACI with primary owner, backup, authorized decision-maker, actual destination, response target, acknowledgement evidence, fallback route and escalation authority.
- A routing state model distinguishing DRAFT, SENT/ATTEMPTED, DELIVERED (where evidence exists), ACKNOWLEDGED, ACCEPTED/OWNED, FAILED and ESCALATED; exact states are to be finalized in architecture review.
- No case can be marked “handed off” merely because an email was composed or a request logged. No individual, inbox, role or approval authority may be invented.

### Track E — Privacy, retention, evidence and security
Research:
- Uganda Data Protection and Privacy Act, 2019 (current consolidated text), Data Protection and Privacy Regulations, 2021 and PDPO guidance.
- Data-controller/collector/processor roles, purpose limitation, minimization, accuracy, data-subject rights, security, cross-border processing, breach response, registration and retention.
- PCI DSS v4.0.1 scope only if cardholder data or the cardholder-data environment is implicated; do not assume all HARAI complaint records are in PCI scope.
- Evidence provenance, access control, encryption, redaction, legal hold, secure deletion, immutable audit, exports and privileged support access.

Initial authoritative sources:
- Data Protection and Privacy Act, ULII consolidated text: https://ulii.org/en/akn/ug/act/2019/9/eng%402023-12-31
- Data Protection and Privacy Regulations, 2021, PDPO: https://pdpo.go.ug/media/2022/03/Data_Protection_and_Privacy_Regulations-2021.pdf
- PDPO registration classification/guidance notes: https://pdpo.go.ug/media//2022/01/20102021105143-Registration_Classification_and_Guidance_Notes.pdf
- PDPO knowledge centre (check current notices): https://pdpo.go.ug/knowledge-center
- PCI Security Standards Council, PCI DSS: https://www.pcisecuritystandards.org/standards/pci-dss/
- PCI SSC FAQ on retention/disposal: https://www.pcisecuritystandards.org/faqs/1318/

Preliminary observation: Uganda's Act and regulations govern collection and processing of personal data, and PDPO guidance says retention must be tailored to the organization's circumstances and legal/operational requirements. A universal retention duration should not be invented. MTN consumer terms publicly state a seven-year record period after account closure for the specified consumer account; this is not automatically HARAI's retention rule. PCI DSS says retention should be limited to what is needed for legal, regulatory or business purposes, and sensitive authentication data such as card verification codes cannot be stored after authorization. Confirm scope and applicability with the acquirer/qualified assessor where card processing is contemplated.

### Track F — Finance, accounting, settlement and dispute outcomes
Research:
- Applicable Uganda financial-reporting framework as determined by entity type and ICPAU requirements.
- IFRS 15 principal-versus-agent analysis; IAS 37 provisions and contingent liabilities; relevant IFRS 9/IFRS 7 issues where financial assets, receivables, credit risk or financial instruments arise; IAS 1 presentation; any applicable PSP-specific Bank of Uganda reporting/audit guidelines.
- Distinguish customer/merchant money, provider-held e-money, settlement receivables, fees, reserves, refunds, reversals, disputes, losses, and company revenue.
- Reconciliation of provider statements, internal immutable ledger, case records and separate authorized financial operations.

Initial authoritative sources:
- IFRS 15: https://www.ifrs.org/issued-standards/list-of-standards/ifrs-15/
- IFRS 15 principal-versus-agent interpretation: https://www.ifrs.org/news-and-events/updates/ifric/2021/ifric-update-november-2021/
- IAS 37: https://www.ifrs.org/issued-standards/list-of-standards/ias-37-provisions-contingent-liabilities-and-contingent-assets/
- IAS 1: https://www.ifrs.org/issued-standards/list-of-standards/ias-1-presentation-of-financial-statements/
- IFRS Foundation Uganda jurisdiction profile (confirm current status with ICPAU): https://www.ifrs.org/use-around-the-world/use-of-ifrs-standards-by-jurisdiction/view-jurisdiction/uganda/
- Bank of Uganda financial reporting and external auditor guidance: https://bou.or.ug/financial_infrastructure_innovation

Preliminary observation: IFRS 15 principal-versus-agent conclusions depend on the promised service and control, including contractual facts; gross vs net revenue cannot be chosen solely from the fact that funds pass through a technical platform. IAS 37 provides a framework for provisions and contingent liabilities, including refund-related obligations in relevant fact patterns. Neither standard by itself determines HARAI's exact entries; an accountant must assess the actual contracts, legal obligations, entity framework and transaction facts.

## 4. Recommended books and standards to study

These are study references, not substitutes for current primary sources or professional advice. Prefer current editions and verify that the text remains in force.

1. **Ugandan primary law first:** National Payment Systems Act and Regulations; Data Protection and Privacy Act and Regulations; Anti-Money Laundering Act and Regulations; Bank of Uganda Consumer Protection Guidelines and current oversight framework. Read the operative text and definitions, not only summaries.
2. **Accounting:** Current IFRS Standards text and official IFRS implementation materials for IFRS 15, IAS 37, IAS 1, and relevant IFRS 9/IFRS 7 topics. Use the applicable framework confirmed by a Ugandan practising accountant.
3. **Payments and operations:** Current MTN/Airtel product manuals and signed agreements; Bank of Uganda reporting/audit guidance; recognized operational-risk, incident-management and reconciliation texts chosen after the actual role and funds flow are defined.
4. **Privacy/security:** Uganda's Act and regulations; current PDPO guidance; PCI DSS v4.0.1 and official FAQs only for applicable card-data scope; current OWASP Application Security Verification Standard and OWASP API Security Top 10 for security design background.
5. **AML/compliance:** Current Ugandan AML legislation/regulations and official Financial Intelligence Authority guidance applicable to the actual regulated entity/activity. FATF recommendations are international standards, not a substitute for Ugandan law or an entity-specific compliance assessment.
6. **Legal practice:** A current Ugandan legal practitioner's annotated statutes, commentary and relevant decisions should be selected by counsel; I will not invent book titles or rely on an unverified edition.

## 5. Initial source-led findings — not final determinations

1. **Provider rules are role-specific.** MTN's public rules distinguish end users from payment processors, discuss direct/indirect integration, contracting/KYC, permissions, limits, settlement and dispute escalation. Exact entitlement still depends on product, role, account and contract.
2. **No universal complaint deadline has been established.** Each legal, contractual and internal deadline must be separately sourced, scoped and versioned.
3. **Regulatory status is a gate.** The National Payment Systems Act and related instruments govern payment-service providers. The actual HARAI operating model must be assessed before concluding which licence, authorization or regulatory obligations apply.
4. **Retention is not a single guessed number.** It requires a data inventory, purpose, applicable legal/contractual duties, legal-hold needs, data-subject rights, cross-border assessment and secure deletion design.
5. **Routing requires real accountable owners.** Role names in a document are not proof that a person is appointed, qualified, available or authorized.
6. **Financial case resolution is not a ledger event.** A complaint outcome must not silently execute or imply a refund, reversal, payout or ledger adjustment. Those require separate authorized workflows and evidence.
7. **Specialist sign-off remains necessary.** Public research cannot replace the provider's written answer for account-specific rights, a qualified Ugandan legal/compliance review, or a qualified accountant's documented accounting conclusions.

## 6. Learning and review sequence

- **R1 — Source validation:** Confirm current versions, amendments, effective dates and exact provisions for each source above; identify inaccessible or account-gated materials.
- **R2 — Operating-model applicability:** Revisit each source against the still-unselected funds-flow model and legal role. Do not assume the role.
- **R3 — Provider evidence matrix:** Separate publicly documented facts from contract-only facts and account-specific confirmations.
- **R4 — Legal obligations register:** Prepare a question-led draft for qualified Uganda counsel; do not label it a legal opinion.
- **R5 — Deadline registry:** Create only source-backed entries; mark unknowns as unverified and blocked.
- **R6 — Routing and privacy control design:** Draft RACI, evidence states, data inventory and retention-decision matrix for review.
- **R7 — Accounting scenario pack:** Prepare hypothetical flows and questions for a practising accountant; no final journal entries until facts/framework are confirmed.
- **R8 — Cross-review:** Map validated conclusions into DOC-64/DOC-65 and the architecture register; preserve all unresolved contradictions.
- **R9 — Independent sign-off:** Record named accountable reviewers, qualifications/authority where relevant, scope of review, evidence reviewed, date and explicit conclusion.
- **R10 — Gate reassessment:** Only after evidence and approvals exist may the accountable reviewer assess whether design gates are met. This document itself does not authorize implementation or tests.

## 7. Outstanding evidence register

| ID | Evidence needed | Current status | Gate consequence |
|---|---|---|---|
| RSR-001 | Actual provider contract/product terms and HARAI role/entitlements for MTN | Not supplied / not independently verified | Provider-specific workflow and permissions remain blocked |
| RSR-002 | Actual Airtel Uganda product documentation, account-specific terms and role/entitlements | Portal public entry found; detailed entitlement not established | Airtel workflow remains blocked |
| RSR-003 | Qualified Uganda legal/compliance applicability review for chosen operating model | Not obtained | No legal deadline or regulatory conclusion may be marked approved |
| RSR-004 | Provider/legal source-backed deadline and escalation register | Not completed | Deadlines and escalations remain unverified |
| RSR-005 | Named operational owner, backup, qualified approver and real routing destinations | Not appointed/evidenced in repository | No operational handoff/authority may be assumed |
| RSR-006 | Approved privacy/data inventory, purpose, retention/deletion, cross-border and legal-hold policy | Not approved | Retention and data-rights behavior remain unresolved |
| RSR-007 | Practising accountant's funds-flow/accounting memo, applicable framework and outcome treatments | Not obtained; funds-flow model unselected | Journal entries, revenue treatment, reserves and loss treatment remain unresolved |
| RSR-008 | Security/PCI scope determination if card processing is pursued | Not established | Do not assume card-data scope or compliance status |
| RSR-009 | Independent review record and gate evidence | Not completed | G1 NOT PASSED; G2 NOT APPROVED |

## 8. Safety and authorization boundary

This research programme authorizes reading, analysis and documentation only. It does not authorize provider follow-up, applications, contracts, credential use, access to restricted portals beyond existing authorized read-only access, live transactions, financial commitments, tests, implementation, merge or deployment. No funds-flow model is selected by this document. No provider eligibility, licence, production entitlement, legal deadline, retention period, accounting treatment, staffing appointment or approval is inferred.

**Current disposition:** research baseline started; authoritative-source validation and specialist review remain in progress. DOC-64 remains a draft proposal, AR-022 remains open, G1 NOT PASSED, and G2 NOT APPROVED.


## Source-validation update — 2026-10-24

The next source pass identified two instruments that must be treated as explicit inputs rather than left implicit:

- **National Payment Systems (Consumer Protection) Regulations, 2022 (S.I. 103 of 2022):** https://ulii.org/en/akn/ug/act/si/2022/103/eng%402022-09-09. The public text covers licensee consumer-protection policy and governance, information/disclosure, protection of consumer assets and information, complaints, inquiry handling and redress. Regulation 6 states a covered licensee submits categorized complaint information and remedial measures to the Central Bank within 15 days from the last day of each month. This is a regulatory reporting deadline only if the entity is within scope; it is not an individual customer complaint response SLA.
- **National Payment Systems (Amendment) Regulations, 2022 (S.I. 102 of 2022):** https://ulii.org/en/akn/ug/act/si/2022/102/eng%402022-09-09. ULII's 2021 NPS Regulations page notes amendments remain unapplied in the consolidated view. Reconcile the 2021 text with the 2022 amendment and any later instruments before relying on a provision as current.

This materially strengthens the legal research track but does not resolve HARAI's licensing status, product role, the applicability of the monthly reporting duty, or any provider/API dispute SLA. These require a fact-specific review by qualified Ugandan counsel and current official/provider evidence. See DOC-67 for the preliminary obligation matrix and the conditional deadline entry.


## Bank of Uganda framework validation update — 2026-10-24

The official **National Payment Systems Oversight Policy Framework (August 2025)** PDF was retrieved and reviewed: https://bou.or.ug/uploads/Revised_BOU_National_Payment_Systems_Oversight_Framework_2025_698b3e9745.pdf. Relevant provisions state that PSPs/PSOs must be licensed to operate, distinguish consumer/merchant-facing PSP activity from PSO activity that may include transfer processing, aggregation and enabling technology, state that PSPs need explicit approval for additional services such as cross-border transfers and product partnerships with other licensed entities, and retain responsibility for material operations performed by third parties. The framework lists payment gateway services within domestic transfer/remittance services and merchant acquisition as eligible payment services/activities (PDF pp. 5–10, 16). These points make legal classification a substantive early gate for HARAI; they do not determine HARAI's status. A qualified Ugandan payments lawyer must assess the actual legal entity, customer-facing role, service scope, funds flow and contracts.

Historical note from the initial retrieval attempt. The full 2011 Consumer Protection Guidelines PDF and January 2024 financial-reporting PDF were subsequently retrieved/reviewed, and the November 2023 financial-reporting PDF was then compared with January 2024. See §§13–16 and DOC-67 §§15–17. The modern BoU page still does not expose a newer standalone full Consumer Protection Guidelines PDF in the reviewed public page data; current legal status/applicability remains a counsel question.


## 11. Second source-validation pass — 2026-10-24

### Newly retrieved sources
- Full published National Payment Systems Regulations, 2021 text (ULII): https://ulii.org/en/akn/ug/act/si/2021/18/eng%402021-03-05
- National Payment Systems (Amendment) Regulations, 2022 (S.I. 102): https://ulii.org/en/akn/ug/act/si/2022/102/eng%402022-09-09
- National Payment Systems (Consumer Protection) Regulations, 2022 (S.I. 103), official publication PDF: https://media.ulii.org/media/legislation/74619/source_file/3a3d89e921e66f51/ug-act-si-2022-103-publication-document.pdf
- PDPO annual DPP compliance report guidance: https://pdpo.go.ug/media/2024/01/Guidance-Note-on-Completion-of-the-Annual-DPP-Compliance-Report.pdf
- PDPO Knowledge Center (current notices): https://pdpo.go.ug/knowledge-center

### Findings and limitations
1. **NPS Regulations amendments:** S.I. 102 of 2022 substitutes Schedules 3 and 4 of the 2021 Regulations (fees and minimum capital). The ULII consolidated 2021 page still flags outstanding amendments not yet applied. This verifies the scope of that specific amendment, not that every later amendment or circular has been found.
2. **NPS outsourcing / cross-border / audit-trail text:** the published 2021 text includes regs. 25 (data management/audit trail and DPPA compliance for PSPs), 28 (specified outsourcing requires written BoU approval), and 29 (cross-border payment services require written BoU approval). Reg. 11(2)(d) describes an operator class including an aggregator, integrator or gateway under the stated conditions. Applicability remains dependent on HARAI's actual role and current complete legal framework.
3. **BoU monthly complaints report:** NPS (Consumer Protection) Regulations, 2022 reg. 6 states a covered licensee must report categorized consumer complaint information and remedial measures within 15 days after month end. This is a conditional regulatory reporting obligation, not an individual complaint resolution SLA.
4. **PDPO annual report:** the PDPO guidance describes regulation 50's annual report within 90 days after financial-year end, including complaint status and breach/action summaries. The PDPO Knowledge Center lists a notice updated 25 August 2026 with a 23 November 2026 deadline. Verify the notice, reporting period, registration/status and prescribed process before deciding applicability or scheduling a filing.
5. **Still unresolved:** whether the 2011 Consumer Protection Guidelines remain current and how they interact with later NPS instruments; which of the November 2023 or January 2024 financial-reporting guideline wordings controls; and HARAI-specific applicability. See §§14–16 and DOC-67 §17. No numeric obligation should be operationalized before qualified review.

See DOC-67 §§10–14 for the obligation matrix and deadline candidates, and DOC-68 for the qualified-reviewer question pack. These are research aids only, not legal or accounting opinions and not authorization to contact a provider/regulator, submit a report, implement, test, merge or deploy.


## 12. Consumer Protection Guidelines source located — 2011 text retrieved (2026-10-24)

The current BoU financial-inclusion page lists a Consumer Protection Guidelines resource. Its download link could not be extracted from the modern page renderer, but a full 18-page PDF titled *Bank of Uganda Financial Consumer Protection Guidelines, 2011* was retrieved from the legacy BoU website: https://bouganda.com/bouwebsite/bouwebsitecontent/Financial_Literacy/Guidelines/2011/Jun/Consumer_Protection_Guidelines_June_2011.pdf.

Key text for qualified review: para. 2 covers BoU-regulated financial-services providers and their agents; para. 3 defines the provider categories as banks, credit institutions, MDIs, forex bureaus and money-remittance companies. Para. 9(5) calls for prompt written acknowledgement unless resolved within three business days; para. 9(6) calls for final response by the end of two weeks with a defined exclusion for delayed consumer replies; para. 9(8) describes six-monthly reporting to BoU within one month after each reporting half-year; para. 9(7) requires identification/remediation of recurring systemic complaint problems. Source citation is the PDF itself; see DOC-67 §15.

**Do not treat as settled current HARAI obligations.** The definition of covered provider categories and the existence of later NPS Consumer Protection Regulations make counsel's current-scope and instrument-interaction analysis necessary. In particular, reconcile the 2011 six-monthly report with the 2022 NPS monthly reporting rule rather than assuming one replaced the other. The two-week response target is not automatically a HARAI or MTN/Airtel SLA.

Historical note from the first retrieval attempt: machine-readable extraction and local download failed. The official BoU PDFs were subsequently inspected through rendered pages, and both November 2023 and January 2024 copies have now been compared; see §14 and DOC-67 §17. The current-version/legal-supersession question remains unresolved.


## 13. January 2024 financial reporting guidelines — rendered PDF reviewed (2026-10-24)

The full 10-page January 2024 PDF has now been visually reviewed page-by-page using the rendered PDF pages (source: https://bouganda.com/bouwebsite/bouwebsitecontent/PaymentSystems/Legal-Frameworks/Financial-Reporting-and-External-Auditor-Guidelines-for-PSPs-and-PSOs-Issued-January-2024.pdf). It states commencement on 1 December 2023 and applies to all BoU-regulated PSPs, PSOs and issuers of payment instruments in respect of Uganda business (clauses 1–2).

Verified headline requirements in clause 4: unverified quarterly income statement and statement of financial position due by the 15th day after quarter-end; external-auditor-verified quarterly statements due by the 30th day after quarter-end; annual financial statements use IFRS as adopted by ICPAU and January–December financial year; draft audited statements and management letter due to BoU by 31 March after year-end; BoU written approval before finalization/publication; extracts published in a newspaper of wide circulation by 30 April; audited statements displayed on the licensee website. Clauses 5–6 require an auditor from BoU's prequalified list, annual audit/quarterly review, a maximum continuous appointment of three years, and prior BoU approval for appointment/change. Clauses 7–10 address auditor duties, access to records, compliance reporting and BoU review. See DOC-67 §16 for the detailed conditional matrix.

**Status update (2026-10-24):** the November 2023 PDF has now been retrieved and compared with January 2024; see §14 and DOC-67 §17. The version comparison identifies wording differences but does not establish which copy legally controls. Qualified counsel/accountant confirmation remains required before operational reliance. No compliance calendar or filing is authorized on this research alone.


## 14. BoU financial reporting guideline version comparison — 2026-10-24

The current official BoU Financial Infrastructure & Innovation page lists both the November 2023 and January 2024 copies of the *Guidelines on Financial Reporting and External Auditors for Payment Service Providers, Payment System Operators and Issuers of Payment Instruments*:

- November 2023 PDF (11 pages): https://bou.or.ug/uploads/Financial_Reporting_and_External_Auditor_Guidelines_for_PS_Ps_and_PS_Os_Issued_November_2023_cb4924312f.pdf
- January 2024 PDF (10 pages): https://bou.or.ug/uploads/Financial_Reporting_and_External_Auditor_Guidelines_for_PS_Ps_and_PS_Os_Issued_January_2024_b1fe168e0e.pdf
- Official listing: https://bou.or.ug/financial_infrastructure_innovation

Both copies state the same formal title, commencement date of 1 December 2023 and coverage of BoU-regulated PSPs, PSOs and issuers of payment instruments for business transacted in Uganda (clauses 1–2). Their contents are not identical. A rendered-page comparison established the following clear differences:

1. **Annual publication count (clause 4(b)(iv)):** November says extracts of signed audited statements must be published in at least two newspapers of wide circulation by 30 April; January says a newspaper of wide circulation. January therefore reduces the stated newspaper count from at least two to one.
2. **Website publication (clause 4(b)):** January expressly adds a requirement to display audited financial statements on the licensee's website (clause 4(b)(v)); the November version's corresponding annual-statement text does not contain that separate website-display clause.
3. **Auditor approval regime and prequalified auditor process (clauses 5–6):** the January PDF visibly contains BoU approval before appointment, approval/conditional approval/decline, withdrawal for sufficient cause, prior written approval before auditor change, annual bid calls and a 30 September application deadline for firms seeking prequalification. However, the November PDF page corresponding to printed page 5 (the transition between clauses 5 and 7) failed to render in this research pass. Therefore whether these provisions are additions or were already present in November is **UNVERIFIED**; do not report them as confirmed differences until that page is obtained and compared.
4. **Auditor reporting package (clause 9(a)):** November describes the auditor submitting an audit report and management letter with the related financial statements by 31 March; January describes draft audited financial statements and a management letter for BoU review with the related financial statements by 31 March. Treat this as a wording/process difference requiring the accountant/counsel to determine the authoritative submission package, not as permission to omit an audit report or other item required elsewhere.
5. **Auditor duties and cross-references (clause 7(a)):** the successfully rendered November and January pages use different formulations of the legal/accounting frameworks the auditor is to consider/report against. November explicitly references the Companies Act in its list; January explicitly mentions IFRS as adopted by ICPAU and refers to other relevant laws/regulations in a different formulation. A qualified reviewer should reconcile the wording with current applicable law and the engagement scope.
6. **Editorial/layout changes:** the PDFs have different page counts (11 vs 10), and paragraph numbering/line breaks are not identical. Both retain the quarterly reporting architecture, IFRS/ICPAU annual statements, January–December financial year, 31 March draft annual submission timing, 30 April publication deadline, three-year continuous auditor limit, auditor access and BoU supervisory-review powers.

**Version-status limitation:** the previously missing November printed page 5 has now been retrieved and compared with January printed page 5. Both copies contain the annual BoU bid-call/prequalification process, the 30 September application deadline, and clause 6's prior approval/conditional approval/decline/withdrawal/change-of-auditor controls. These are therefore **not confirmed January additions**. The visible comparison remains targeted rather than a full textual redline; the two PDFs have different page counts and some other clauses still need careful line-by-line verification. Neither copy expressly states that January repeals, replaces or amends November. Do not conclude supersession, transitional effect or which wording controls HARAI. Obtain qualified confirmation before operational reliance.

The financial reporting guideline is applicable by its stated scope to regulated entities in the named categories. It does not determine HARAI's legal role or prove that HARAI is licensed/covered. No compliance calendar, appointment, publication, filing or other action is authorized by this research.

## 15. Data Protection and Privacy Regulations, 2021 — targeted clause review

Primary official PDF: https://pdpo.go.ug/media/2022/03/Data_Protection_and_Privacy_Regulations-2021.pdf. ULII instrument record: https://ulii.org/en/akn/ug/act/si/2021/21/eng%402021-03-12. The official PDPO annual-report guidance is https://pdpo.go.ug/media/2024/01/Guidance-Note-on-Completion-of-the-Annual-DPP-Compliance-Report.pdf.

The targeted review identified the following design-relevant provisions; the exact current legal interpretation and application still require qualified Ugandan privacy counsel/DPO review:

- **Registration:** regulations 13–15 address registration and related process/exemption issues. Do not assume registration status, exemption or fee treatment for HARAI; verify the actual legal entity, data roles, activities and current PDPO position. Registration is distinct from a conclusion about NPS licensing.
- **Security practices and controls:** regulations 31–32 concern publication of personal-data security practices and procedures and security measures by a data controller. The Data Protection and Privacy Act (DPPA) also imposes security obligations. Translate these into a data inventory, access control, safeguards, incident handling and evidence only after the role and processing facts are known.
- **Breach notification:** regulation 33(1) says the notification required under DPPA section 23(1) is to be made immediately after occurrence of the breach. Regulation 33 specifies Form 7 and information including breach nature, affected personal data, categories/approximate number of data subjects, likely consequences, remedial measures and DPO/other contact details. Counsel must confirm the Act's associated notice duties and the meaning/application of “immediately” to a real incident. This is not a reason to delay containment while preparing a report.
- **Data-subject complaints:** regulation 40 requires data collectors, controllers and processors to develop and implement a complaints-handling system for data-subject complaints. Regulation 41 provides for complaints to the Office about infringement/non-compliance and related notices. This privacy complaint process is separate from NPS payment-consumer complaints and provider transaction disputes.
- **Cross-border processing:** DPPA section 19 and regulation 30 address personal data processed or stored outside Uganda, including adequate protection or consent conditions and restrictions on onward transfers. HARAI's hosting, logs, support access, backups, subprocessors and disaster recovery locations must be inventoried before selecting a hosting architecture or asserting compliance.
- **Retention and deletion:** DPPA section 18 sets the general purpose/necessity-based retention principle and destruction/de-identification requirements subject to specified exceptions; regulation-level obligations and any sectoral, contractual, dispute, audit or legal-hold requirements must be reconciled. No universal retention duration should be invented. Record purpose, legal basis, trigger, retention rule, hold conditions and secure deletion evidence per data category.
- **Annual PDPO report:** regulation 50 requires registered data collectors/controllers/processors to submit within 90 days after the end of the Government financial year a summary of complaints and their status and data breaches/actions. PDPO's official guidance says the Government financial year runs 1 July–30 June and expands the report template's scope under regulation 4(b). This is conditional on the relevant registration/role and current filing guidance; it is not a substitute for immediate breach notification or payment-sector reports.

This is a targeted, not exhaustive, review of the Regulations. Do not label the whole instrument “fully audited” or derive a final compliance position without reviewing the full instrument against HARAI's real data flows and receiving qualified review.

## 16. Reconciliation of complaint reporting — separate clocks and audiences

Three potentially relevant regimes must remain distinct:

1. **2011 BoU Financial Consumer Protection Guidelines, para. 9(8):** six-monthly complaint reporting to BoU, due within one month after the end of each reporting half-year, subject to the guideline's stated exclusion for complaints resolved by the end of the business day after receipt. The guideline's para. 2–3 scope names BoU-regulated financial-services providers and agents and defines categories that do not automatically include every standalone PSP/PSO or technology gateway. Current status and applicability require counsel confirmation.
2. **NPS Consumer Protection Regulations, 2022, regulation 6:** a licensee must submit to the Central Bank within 15 days after the last day of every month information about consumer complaints, segregated by category and agent location, including remedial measures. This is a monthly regulatory reporting duty for covered licensees; it is not a deadline for resolving each complaint. Primary text: https://ulii.org/en/akn/ug/act/si/2022/103/eng%402022-09-09.
3. **Data Protection and Privacy Regulations, 2021, regulation 50:** covered registered data collectors/controllers/processors submit an annual PDPO report within 90 days after the Government financial year, summarizing privacy complaints/status and breaches/actions. It is a different regulator, reporting period, data scope and legal trigger.

Do not assume these regimes are alternatives, that a later rule silently repeals an earlier instrument, or that the same complaint record automatically satisfies every report. Keep the source, scope, trigger, reporting period, recipient, data fields, exception, evidence and reviewer for each obligation separately. Qualified counsel must determine overlap, amendments and which duties attach to HARAI after the operating model and legal role are established.
