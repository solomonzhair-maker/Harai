# DOC-30 — Uganda Compliance and Funds-Flow Review Checklist

**Status:** Draft checklist — not legal advice and not a determination of licensing requirements  
**Market focus:** Uganda first  
**Purpose:** Prepare a structured review for qualified Ugandan legal/compliance advisers and relevant official authorities before HARAI selects its operating model.

## 1. Important boundary

This checklist identifies questions to investigate; it does not conclude that HARAI requires or does not require a particular licence, approval, registration, or regulatory classification. Applicability depends on the actual activities, contracts, flow of funds, customer relationships and current law. Obtain advice from qualified professionals and, where appropriate, written clarification from the Bank of Uganda or another competent authority.

No provider onboarding, collection of live credentials, custody of funds, live transaction, payout, or production launch is authorized by this document.

## 2. Describe the proposed service accurately

The review pack should explain:
- HARAI is intended as a reusable, multi-merchant payment gateway.
- Uganda is the first target market; regional expansion is a future possibility, not assumed launch scope.
- The product ambition includes collections and money-out, with refunds/reversals where provider-supported.
- Provider-managed funds, direct integrations and a possible evidence-supported hybrid are being compared.
- The final contracting parties, account ownership, fund custody/holding, settlement and payout funding route remain unconfirmed.
- HARAI must not claim provider approval, regulatory status, merchant eligibility or production capability until verified.

## 3. Questions for qualified Ugandan counsel/compliance review

### A. Regulatory classification and permissions
- What laws, regulations, licence categories, approvals, notifications or exemptions may apply to each proposed activity?
- Does the answer change if HARAI only supplies software/API orchestration versus contracting with merchants, aggregating payments, initiating transfers, controlling settlement instructions, holding funds, pooling balances or paying third parties?
- Does using a licensed provider or intermediary change HARAI's own obligations? What activities remain the gateway's responsibility?
- What specific approval is needed for sub-merchant onboarding, collections aggregation, disbursements, refunds/reversals, payment initiation or settlement?
- Which regulator or authority is competent for each activity, and what official written confirmation should be obtained before proceeding?
- Are there restrictions based on merchant type, transaction purpose, value, customer location or cross-border flows?

### B. Funds flow and ownership
- At every stage, who is payer, merchant, contracting provider, account holder, beneficial owner of funds, settlement recipient and party with control over transfers?
- Are any funds received, held, pooled, safeguarded, prefunded, netted, transmitted or paid out by HARAI or on its instructions?
- What contractual and legal separation exists between HARAI operating funds, provider-held funds and merchant entitlements?
- What happens on provider insolvency, merchant suspension, dispute, fraud suspicion, negative balance, failed payout, refund or termination?
- Are settlement accounts, reserves, deductions, fees, set-off and reconciliation arrangements legally and contractually clear?

### C. Merchant due diligence and risk
- What business, beneficial-owner, identity, tax, bank-account and source-of-funds checks are required for each merchant category?
- Which party collects and verifies evidence, decides acceptance, monitors changes and keeps records?
- What prohibited/restricted merchant categories and transaction patterns apply?
- What sanctions, fraud, suspicious activity, escalation, reporting and recordkeeping duties may apply?
- What rules govern payout beneficiary verification, changes to destination accounts, transaction limits and dual approval?

### D. Consumer, privacy and data obligations
- Which party is responsible for payer/merchant disclosures, receipts, complaints, refunds, error correction and dispute handling?
- What data-protection obligations apply to personal data, verification documents, transaction records, callbacks, analytics and support access?
- What lawful basis, notices, retention periods, deletion exceptions, data-subject rights and security controls are needed?
- Are cross-border hosting, subprocessors, provider data transfers or international expansion subject to additional restrictions?
- What incident notification, breach response and regulator/customer communication duties apply?

### E. Contracting, liability and governance
- Which contracts are needed among HARAI, each merchant, each provider/intermediary and any technical subprocessors?
- Who bears responsibility for unauthorized transactions, delayed/incorrect status, fraud, mistaken payout, refunds, reversals, downtime and reconciliation differences?
- What audit rights, complaint routes, termination assistance, data return/deletion, service levels and incident-notification terms are required?
- Which decisions require board/owner approval, qualified sign-off, provider approval or independent review?
- What records should be retained to demonstrate that approvals, permissions and financial actions were authorized?

## 4. Funds-flow diagrams to prepare and review

Create separate diagrams for each candidate model; do not combine distinct models into one ambiguous drawing.

1. **Collections:** payer instruction → provider authorization/processing → account credited → merchant settlement → HARAI fee flow, if any.
2. **Money-out:** funding source → account/prefund check → authorization/approval → provider submission → recipient destination → final status and reconciliation.
3. **Refund/reversal:** original transaction → eligibility check → authorized request → provider result → ledger/settlement adjustment → merchant/payer communication.
4. **Failure and dispute:** timeout/unknown status, duplicate callback, provider outage, returned payout, disputed transaction, correction and escalation.
5. **Termination/insolvency:** outstanding transactions, merchant balances/entitlements, reserves, unresolved disputes, data export and closure.

Each diagram must label account owner, party controlling each action, legal/contractual relationship, timing, data shared, reconciliation reference and unresolved assumptions. A diagram is not approved merely because it is technically possible.

## 5. Evidence to collect

- Current official laws, regulations, regulator guidance and relevant licence/authorization registers.
- Written advice from qualified Ugandan legal/compliance professionals.
- Written provider confirmation of permitted gateway/sub-merchant activities and product capabilities.
- Draft/approved provider and intermediary agreements, including settlement and payout terms.
- Fee schedules, settlement reports, API specifications, callback/status rules and sandbox documentation.
- Merchant onboarding/KYC requirements, prohibited-business rules and risk controls.
- Privacy/data-processing terms, retention schedules, security commitments and incident procedures.
- A complete funds-flow diagram and named owners for every account, decision and operational responsibility.

Record source, version/date, reviewer, exact claim supported, limitations and related decision ID. Keep confidential documents in an access-controlled location, not a public repository.

## 6. Decision gates

| Gate | Evidence required | If unresolved |
|---|---|---|
| Regulatory classification | Written qualified review of the actual proposed activities | Do not select the operating model or begin regulated activity |
| Multi-merchant permission | Provider/contractual permission plus review of applicable requirements | Do not onboard sub-merchants or advertise the model as approved |
| Funds custody and settlement | Reviewed funds-flow diagrams and clear account/contract ownership | Do not design live balance or settlement assumptions as facts |
| Money-out funding | Confirmed funding source, authorization rules, failed/returned payout handling | Keep payouts blocked from live use |
| Merchant due diligence | Defined roles, required checks, review states and escalation | Do not activate live merchants without required checks |
| Refunds/reversals | Provider capability, eligibility, accounting and approval rules | Do not claim or enable unsupported flows |
| Data protection | Responsibilities, notices, access, retention and incident plan reviewed | Do not collect unnecessary sensitive evidence or launch |
| Operational readiness | Reconciliation, support, complaints, incident and recovery owners | Do not authorize production launch |

## 7. Current public-source starting points

Use current official material and re-check its effective date during the professional review:
- Bank of Uganda financial infrastructure and oversight: https://bou.or.ug/financial_infrastructure_innovation
- Bank of Uganda National Payment Systems Oversight Framework (2025 PDF): https://bou.or.ug/uploads/Revised_BOU_National_Payment_Systems_Oversight_Framework_2025_698b3e9745.pdf
- Bank of Uganda FAQ on the National Payment Systems Act and regulations: https://bou.or.ug/

These are starting points, not a substitute for reviewing the actual facts and obtaining qualified advice.

## 8. Review record

- Adviser/organization:
- Qualifications and scope:
- Date and materials reviewed:
- Proposed model reviewed:
- Activities and flows covered:
- Written conclusions and conditions:
- Unresolved questions:
- Required authority/provider confirmations:
- Actions, accountable owners and target dates:
- Re-review trigger (law, product, provider or funds-flow change):

## 9. Exit criteria

This checklist is ready for decision support only when the actual candidate funds flows are documented, relevant legal questions have written review, required provider/authority confirmations are identified or obtained, blockers have accountable owners, and the decision register accurately reflects what remains unknown. This checklist alone does not authorize implementation or launch.
