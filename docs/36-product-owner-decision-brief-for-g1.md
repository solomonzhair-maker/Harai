# DOC-36 — Product Owner Decision Brief for G1

**Status:** Product owner approved all recommended defaults on 2026-10-19; provider, legal/compliance and G1 evidence gates remain open  
**Date:** 2026-10-19  
**Related documents:** DOC-00, DOC-02, DOC-03, DOC-28, DOC-29, DOC-30, DOC-31, DOC-33, DOC-34, DOC-35  
**Purpose:** Record the product owner’s approved default direction for the G1 baseline and distinguish those product decisions from evidence-dependent provider, legal, commercial, and release gates.

> **Decision boundary:** POD-01–POD-12 are approved product-direction defaults. They do not prove provider eligibility, settle legal classification, select the funds-flow/custody model, authorize provider outreach, or authorize payment-engine implementation.

## 1. Recommended owner decisions

| ID | Decision | Owner-approved default / direction | Why this is the safer starting point | What approval does and does not mean |
|---|---|---|---|---|
| POD-01 | Initial market | Uganda only for first launch; regional expansion requires a separate country review | Keeps regulatory, currency, provider and support assumptions bounded | Approves market scope only; not legal permission |
| POD-02 | Merchant eligibility | Start with a defined allowlist of ordinary, verifiable businesses; prohibit or hold categories requiring specialized legal/provider review until approved | “All businesses” creates unknown onboarding, fraud, reputational and provider eligibility risks | Approves a category policy only after the owner reviews the proposed list; does not replace provider KYC/AML rules |
| POD-03 | Launch currencies | UGX as the only planned initial currency unless a confirmed provider contract and settlement path require/permit another | Avoids premature multi-currency pricing, rounding, FX, settlement and reconciliation complexity | Approves product scope only; provider currency capability and legal/accounting treatment still need evidence |
| POD-04 | Payment-method scope and providers | **Owner-confirmed direction:** include both MTN MoMo and Airtel Money (not an either/or choice), card payments (such as Visa/Mastercard through an eligible processor/acquirer), and additional suitable methods. Validate each method/provider independently before activation. | HARAI is intended as a multi-provider gateway; method coverage, contracts, fees, eligibility, settlement and technical access differ by provider and method | Records product intent only; it does not prove access/approval, authorize provider contact, select a processor, or authorize integration/implementation |
| POD-05 | Collections and money-out | Keep both in intended launch scope, but make each method/capability unavailable until provider permission, funding, legal, controls and reconciliation gates pass | Preserves the owner’s comprehensive product goal without representing unsupported operations as available | Approves product intent, not a live flow or provider-specific capability |
| POD-06 | Refunds/reversals | Include where the exact provider product supports the operation; define separate semantics for refunds, reversals and payout returns | Prevents overpromising capabilities that differ by provider/product | Approves intended requirement; actual support remains evidence-gated |
| POD-07 | Funds-flow model | Do not choose yet; compare provider-managed funds and direct integrations using written evidence and qualified Ugandan review | Avoids accidental custody/control assumptions and a potentially unsuitable licensing/contract model | No model is approved until evidence and review support a recommendation |
| POD-08 | Pricing and fees | Do not publish or hard-code prices until provider costs, fee payer, tax treatment, operating cost and commercial model are reviewed | Prevents unsupported prices and margin assumptions | Approves a pricing process, not numeric fees |
| POD-09 | Risk limits and approvals | Require configurable limits, audit trails and risk-based approvals; leave numeric thresholds unset until evidence and risk review | Avoids invented limits and makes high-risk operations controllable | Approves controls as requirements, not specific values or a live payout policy |
| POD-10 | Data and privacy | Collect minimum necessary data, restrict access, document retention/deletion and never place live secrets or identity evidence in repository/tests | Reduces avoidable exposure and supports privacy-by-design | Establishes design direction; detailed obligations require qualified review |
| POD-11 | Reliability and recovery | Require measured objectives and restore/incident procedures, but do not invent availability/RTO/RPO numbers before capacity and business impact analysis | Prevents unrealistic promises and untestable targets | Approves the requirement; numeric targets remain open |
| POD-12 | Governance and implementation | Continue documentation and design proposals; require a separate explicit authorization before payment-engine implementation, provider outreach, merge or deployment | Maintains clear control of consequential actions | Confirms process boundary; does not grant implementation authority |

## 2. Decisions that cannot be settled by product preference alone

Even if the product owner accepts the defaults above, these remain evidence-dependent:
- provider/processor contractual eligibility and permission to support multiple merchants / an aggregator-style gateway for each intended payment method;
- account ownership, who controls or holds funds, settlement route and payout funding source;
- provider-specific collection, payout, refund, reversal, return, callback, idempotency and reconciliation behavior;
- regulatory classification, any required licenses/approvals, merchant due diligence, data obligations and contract responsibilities;
- actual fees, limits, settlement schedules, reserves, tax treatment and provider support commitments.

The product owner may decide the intended product, but cannot make a provider capability or legal permission exist by approving a requirement.

## 3. Proposed merchant eligibility rule for later refinement

Recommended policy shape:
1. Publish a supported-merchant allowlist before onboarding is opened.
2. Require business identity, beneficial-owner/control information and documents only to the extent the final provider and compliance rules require.
3. Explicitly prohibit categories that providers or applicable law prohibit.
4. Put uncertain or higher-risk categories into manual review rather than auto-approving them.
5. Record review decision, evidence reference, reason, reviewer and any re-verification date.
6. Do not copy provider due-diligence rules from another gateway; obtain current written requirements for the actual contracting route.

The actual category list remains a product-owner decision informed by provider and qualified compliance review.

## 4. Owner decision record

On 2026-10-19, the product owner approved all recommended defaults POD-01 through POD-12. This includes Uganda-first launch; a broad but controlled merchant eligibility policy; UGX-first launch currency; both MTN MoMo and Airtel Money plus cards through an eligible processor/acquirer and additional suitable methods; collections and money-out as intended launch capabilities; provider-supported refunds/reversals; no funds-flow model selected until evidence and qualified review; configurable pricing and risk controls with numeric values still open; privacy-by-design; measurable reliability and recovery objectives; and separate authorization gates for consequential actions.

Approval is of product direction and default requirements. It does not approve a specific merchant category list, provider contract, card processor, numeric fee/limit/SLO, funds-flow or custody model, legal classification, production capability, provider contact, payment-engine implementation, merge or deployment. Those remain subject to their stated evidence and authorization gates.

## 5. Status and next action

**POD-01 through POD-12 defaults are approved by the product owner.** DOC-00, DOC-02 and DOC-31 must reflect this approved product direction. G1 remains open until the remaining requirements review, provider/processor evidence, qualified Ugandan legal/compliance review, funds-flow analysis and accountable reviewer gates are satisfied. Product intent does not itself prove provider capability or legal permission.
