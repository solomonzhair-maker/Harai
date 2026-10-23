# DOC-27 — Public JulyPay Website and Commercial-Positioning Review

**Status:** Draft — public claims recorded; independent verification required  
**Review date:** 2026-10-17  
**Sources:** Public JulyPay website, pricing page, terms of service, contact page, and public registration page  
**Purpose:** Understand the reference product's stated audience, onboarding model, pricing, public integration requirements and declared processing relationship before expanding HARAI requirements.

## 1. Evidence rules

This document records statements published on public pages. It does not independently validate those statements, legal status, provider licensing, actual technical behavior, or contractual availability. Website content may be incomplete, stale, inconsistent across pages, or subject to change.

- **Publicly stated:** Published on the cited page at the time reviewed.
- **Dashboard-observed:** Visible in the authenticated UI; see DOC-26.
- **Unverified:** Requires direct confirmation from accountable parties, official provider documentation, legal review, or contract evidence.
- No inference about JulyPay's private implementation is made.

## 2. Public product positioning

The homepage positions JulyPay as a mobile-money collection platform **built for small-scale/community ISPs**, particularly for collecting subscription payments through selected integrated ISP billing systems. It says users need an active account on an integrated ISP billing system and completed KYC, and that integration credentials are shared after checks. It also says API documentation is available to verified ISP billing systems following checks.

Source: https://julypay.net/

**Implication for HARAI:** JulyPay's public positioning is narrower than a general-purpose, self-service payment gateway. HARAI's target market must be decided independently; the reference's audience should not automatically constrain HARAI to ISPs or imply that all businesses can integrate with the same provider.

## 3. Public registration and onboarding claims

The public registration page presents Personal and Business paths and describes use cases including individuals, freelancers, hotspot operators, non-profits, companies, collections and API integrations. The visible form asks for identity and organization information, registration documents, an identity document, a selfie holding ID, proof of address, and intended use.

Source: https://app.julypay.net/register

The Terms of Service state that users must be at least 18, provide accurate information, complete KYC, use the service lawfully, and understand that transactions may be delayed, rejected or reversed.

Source: https://julypay.net/terms-of-service/

**Important inconsistency to resolve:** The homepage says JulyPay is only available to Wi-Fi ISPs using selected billing systems, while the registration page advertises broader Personal/Business categories. The public pages do not establish which registration categories are actually eligible for approval or API access. Treat this as a public-site messaging/eligibility ambiguity, not proof of a backend defect.

**HARAI implication:** Define separate account registration, business verification, product eligibility, provider activation and API access states. Do not equate a successfully created account with approval to process live payments.

## 4. Public pricing

The pricing page publicly states:
- Collections fee: 4%.
- Withdrawal fee tiers:
  - UGX 500–45,000: UGX 1,000
  - UGX 45,001–125,000: UGX 1,500
  - UGX 125,001–500,000: UGX 1,800
  - UGX 500,001–5,000,000: UGX 2,000

Source: https://julypay.net/pricing/

These are **JulyPay's advertised prices only**, not proposed HARAI prices or independently confirmed provider costs. The page does not, by itself, clarify every edge case (for example, treatment of amounts outside the listed ranges, taxes, reversals, failed attempts, fee payer options, or effective date).

**HARAI implication:** Pricing must be a versioned, explicit product decision with clear fee payer, rounding, tax, refund/reversal, failed-payment and payout rules. Never copy JulyPay's pricing by default.

## 5. Publicly stated processing relationship and terms

The Terms of Service, updated 21 November 2025, state that JulyPay does not directly hold or process customer funds and that payments are handled by RelWorx, described there as a licensed third-party provider. The terms say JulyPay provides a platform to initiate and track payments and depends on that provider's availability.

Source: https://julypay.net/terms-of-service/

This is a **statement made by JulyPay's published terms**, not independent verification of RelWorx's licensing, service scope, or JulyPay's current agreements. Those details require direct verification before any reliance.

**HARAI implication:** HARAI must decide and document its own funds-flow and responsibility model before choosing architecture or making marketing claims. Identify who contracts with the merchant, who initiates processing, who receives/holds/settles funds, who handles KYC, disputes, refunds, complaints, and regulatory reporting. Provider contracts and qualified legal/compliance review are launch gates.

## 6. Contact and API access

The public contact page provides support contact information and a Kasese Town office address. The homepage says API documentation is restricted to verified ISP billing systems and directs prospective integrators to compliance@julypay.net.

Sources:
- https://julypay.net/contact/
- https://julypay.net/

Contact details are recorded only as public reference points; no outreach was performed and no response or agreement is implied.

## 7. Relevance to HARAI discovery

Public claims suggest these topics should be first-class requirements:
1. Merchant and organization eligibility, KYC/verification states, and activation gates.
2. Separate onboarding from production API credentials and live-processing authorization.
3. Transparent fee calculation and merchant-visible fee disclosures.
4. Provider dependency, degraded service, outage communication and reconciliation.
5. Clear distinction between gateway software and the entity that actually processes or settles funds.
6. API access approval, integration review, sandbox/live separation and credential issuance.
7. Accurate marketing language: do not claim licensing, compliance, integrations, or support without documentary evidence.

These are requirements candidates, not approved V1 scope.

## 8. Open questions for HARAI's decision register

- Is HARAI initially for its own products (such as Shigona and Herupu), a limited partner set, or third-party merchants generally?
- Which country/countries and currencies are in scope for V1?
- Will HARAI act as software/orchestration only, or will any party in the proposed model contract for or control funds?
- Which licensed providers will be contracted, for which methods and transaction directions?
- What are the eligibility, KYC/KYB, underwriting and ongoing-monitoring requirements?
- Which fee model will HARAI use, who pays each fee, and how are taxes, rounding, refunds, reversals and failed attempts handled?
- What evidence is required before enabling production API keys or money-out features?

## 9. Source list

- JulyPay homepage: https://julypay.net/
- JulyPay pricing: https://julypay.net/pricing/
- JulyPay Terms of Service: https://julypay.net/terms-of-service/
- JulyPay contact: https://julypay.net/contact/
- JulyPay registration: https://app.julypay.net/register/
- Authenticated dashboard observations: DOC-26 — `docs/26-authenticated-julypay-read-only-findings.md`

## 10. Conclusion

The public website adds important business-model context that cannot be obtained from dashboard screens alone. It also reveals an eligibility-messaging ambiguity between the homepage and registration page. This should inform discovery, not be silently resolved by assumption. HARAI's market, pricing, provider model, and compliance obligations remain open decisions. No payment-engine implementation, merge, deployment or live financial activity is authorized by this review.
