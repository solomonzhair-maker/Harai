# DOC-36 — Product Owner Decision Brief for G1

**Status:** Proposed decisions for owner response; none are approved by this document  
**Date:** 2026-10-19  
**Related documents:** DOC-00, DOC-02, DOC-03, DOC-28, DOC-29, DOC-30, DOC-31, DOC-33, DOC-34, DOC-35  
**Purpose:** Reduce repeated discussion by presenting a small set of material product choices with recommended conservative defaults. The product owner can approve the recommendations, amend them or reject them.

> **Decision boundary:** These are product-scope proposals only. They do not prove provider eligibility, settle legal classification, select the funds-flow/custody model, authorize provider outreach, or authorize payment-engine implementation.

## 1. Recommended owner decisions

| ID | Decision | Recommended default for owner consideration | Why this is the safer starting point | What approval would and would not mean |
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

## 4. Compact response template

The owner can respond with only the exceptions; unmentioned items remain **proposed, not approved**.

- POD-01 Uganda only: Approve / Amend
- POD-02 Merchant allowlist: Approve direction / Amend / Need proposed category list first
- POD-03 UGX-first: Approve / Amend
- POD-04 Payment-method direction (both MTN MoMo and Airtel Money, cards, additional suitable methods): Confirmed by owner; amend only if the intended scope changes
- POD-05 Collections + money-out intended launch scope with hard gates: Approve / Amend
- POD-06 Refunds/reversals where provider-supported: Approve / Amend
- POD-07 No funds-flow model selected yet: Approve / Amend
- POD-08 No numeric pricing until costs/review: Approve / Amend
- POD-09 Configurable controls; numeric risk limits remain open: Approve / Amend
- POD-10 Data-minimization/privacy-by-design direction: Approve / Amend
- POD-11 Set reliability objectives after analysis: Approve / Amend
- POD-12 Separate implementation/provider-contact authorization: Approve / Amend

## 5. Status and next action

**POD-04 payment-method direction is confirmed by the product owner.** The other POD decisions remain proposed until explicitly answered. DOC-00, DOC-02 and DOC-31 have been aligned to the confirmed method scope. Re-check the G1 gates as remaining decisions are resolved. Product intent does not itself satisfy provider/processor evidence or qualified Ugandan legal/compliance review.
