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

The BoU Consumer Protection landing page was reviewed, but its linked guideline PDF was not successfully retrieved as a standalone source. The page's high-level description is not enough to establish detailed clause-level obligations or complaint SLAs. The BoU page also lists the January 2024 Financial Reporting and External Auditor Guidelines for PSPs and PSOs, but the full guideline PDF was not retrieved and reviewed clause-by-clause in this pass. Do not claim its detailed requirements are verified; obtain the official document and compare it with the November 2023 version before accountant/counsel applicability review. DOC-67 records these source-status distinctions.
