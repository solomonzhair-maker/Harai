# DOC-45 — Evidence Register and Acceptance-Test Planning Standard

**Status:** Draft — planning register, not test execution evidence  
**Date:** 2026-10-19  
**Related documents:** DOC-31, DOC-34, DOC-37, DOC-38, DOC-40, DOC-41, DOC-42, DOC-43, DOC-44  
**Purpose:** Define how HARAI will distinguish a requirement, design proposal, external fact, test plan, test execution, and independent approval so that no status is overstated.

## 1. Evidence classes

| Class | Meaning | Examples | What it does not prove |
|---|---|---|---|
| E0 — assertion | Unverified statement or assumption | A planning note or verbal claim without source | Any fact, permission or control |
| E1 — public source | Public provider documentation or regulation | API reference, public onboarding page | HARAI account eligibility, contract permission, production activation or actual runtime behavior |
| E2 — account observation | Read-only observation in an authenticated account, recorded with date and scope | Product subscription or dashboard state | Contractual multi-merchant permission unless explicitly stated; future access or production readiness |
| E3 — written authority/contract | Written provider confirmation, executed agreement, official approval or qualified professional advice in scope | Multi-merchant authorization, fee/settlement schedule, legal memo | Correct implementation or operational performance |
| E4 — design artifact | Reviewed architecture, data model, threat model, API contract or runbook | DOC-40–44 reviewed and versioned | Working code or passing tests |
| E5 — automated verification | Reproducible test output tied to a commit/build and environment | Unit, contract, integration, concurrency, security regression or recovery tests | Independent audit or live provider performance unless explicitly exercised |
| E6 — operational evidence | Controlled exercise or production evidence with safe data handling | Restore drill, reconciliation sample, alert exercise, incident simulation | General compliance outside the tested scope |
| E7 — independent sign-off | Dated review by a named accountable person, with scope and findings | Security, finance, legal/compliance or architecture review | Certification beyond the reviewer's authority or the evidence reviewed |

A higher class does not automatically replace a lower one: for example, a contract does not prove code behavior, and a passing test does not create legal authority.

## 2. Evidence record requirements

Each evidence item should have:
- Unique evidence ID and linked requirement/decision IDs.
- Claim being supported, including exact scope and limitations.
- Evidence class (E0–E7), source/owner and date captured.
- Stable source location or artifact path; use official links where public.
- Environment and version/commit/build for test or operational evidence.
- Redaction/privacy classification and retention location.
- Reviewer, review date and outcome where independent review is required.
- Expiry/revalidation trigger (contract renewal, API version change, credential rotation, material architecture change, incident or periodic review).
- Status: Missing, Requested (not yet sent), Received, Reviewed, Accepted, Rejected, Expired, or Superseded.

Never put API secrets, passwords, access tokens, OTPs, session cookies, private customer information or unredacted payment data into Git, issues, PR descriptions, evidence screenshots or this register.

## 3. Provider evidence register — current state

| ID | Evidence item | Current state | Required next evidence | Blocks |
|---|---|---|---|---|
| EV-MTN-01 | MTN Collections product subscription | E2 read-only portal observation; Test subscription active | Reconfirm account scope if changed; retain sanitized dated record | Does not block test-environment design; does not authorize live processing |
| EV-MTN-02 | MTN Collection Widget subscription | E2 read-only portal observation; Test subscription active | Confirm intended use and relevant contract | Hosted collection method enablement |
| EV-MTN-03 | MTN Disbursement access | No active subscription visible in inspected portal; no production approval visible | Written eligibility, application/approval and current product status | MTN money-out |
| EV-MTN-04 | MTN multi-merchant/aggregator permission | Not established by inspected evidence | Written provider confirmation/contract for HARAI's proposed operating model | Any multi-merchant live processing |
| EV-MTN-05 | MTN fee, settlement and operating terms | Complete commercial schedule not visible in inspected evidence | Written fees, settlement cadence, deductions/reserves, limits, refund/reversal and reconciliation terms | Funds-flow scoring and live launch |
| EV-MTN-06 | MTN callback/recovery behavior | Public docs indicate callback may be one-shot; status enquiry and reconciliation are documented | Confirm applicable API/version and contractual operating behavior | Final callback/retry design |
| EV-AIR-01 | Airtel Uganda developer account dashboard | E2 read-only portal observation; 0 live and 3 archived applications at inspection | Current account/application status if changed | Does not establish production access |
| EV-AIR-02 | Airtel collection API documentation | E1 public docs include initiation, enquiry, refunds and ambiguous/in-progress states | Confirm applicable product, account access, current version and contractual behavior | Airtel collection enablement |
| EV-AIR-03 | Airtel disbursement API documentation | E1 public docs describe money-out flows and ambiguity/error classes | Written access, funding, limits, fees and operating permission | Airtel money-out |
| EV-AIR-04 | Airtel multi-merchant/aggregator permission | Not established by inspected evidence | Written confirmation and applicable contract | Any multi-merchant live processing |
| EV-AIR-05 | Airtel fees, settlement and reconciliation terms | Complete schedule not visible in inspected evidence | Written fees, settlement cadence, service charges, limits, refund/reversal and reconciliation terms | Funds-flow scoring and live launch |
| EV-CARD-01 | Eligible card processor/acquirer and scope | Not selected or verified | Written onboarding eligibility, pricing, settlement, dispute and applicable security scope | Card payment launch |
| EV-LEGAL-01 | Uganda funds-flow and compliance review | Missing | Qualified written advice tied to the exact proposed operating model | Funds-flow selection and launch |
| EV-FIN-01 | Ledger/accounting model approval | Missing | Qualified reviewer sign-off on accounts, posting patterns, fee/tax semantics and corrections | Ledger approval and live financial operations |
| EV-SEC-01 | Independent threat-model/security review | Missing | Named review with findings, severity, remediation and retest requirements | G2 security sign-off |
| EV-OPS-01 | Operations and reconciliation ownership | Not assigned | Named accountable owners, escalation coverage, runbooks and sample evidence | Operational readiness |

These records summarize the existing project notes and are not proof that a provider has denied or granted access. Portal state may change and must be rechecked before consequential decisions.

## 4. Acceptance-test planning rules

A test ID in DOC-31, DOC-34, DOC-41, DOC-42 or DOC-43 is a **planned scenario reference** until an executable test is present and a result is captured. Never label a planned test “passed.”

Each test case must specify:
1. Requirement IDs and threat/decision IDs covered.
2. Preconditions, synthetic fixtures and test environment.
3. Exact action/input, including concurrency or ordering where relevant.
4. Expected persisted state and externally observable response.
5. Expected ledger/posting effect, including the expectation of no posting where applicable.
6. Security/tenant boundary checks.
7. Idempotency, replay, retry and failure assertions.
8. Evidence artifact path, build/commit, timestamp and result.
9. Cleanup and confirmation that no live funds or real personal data were used.
10. Independent reviewer if the requirement requires one.

## 5. Minimum acceptance-test families before any live financial operation

These are test families to plan and later execute under an approved implementation/test authorization; they are not claims of existing code or test coverage.

### A. Tenant and authorization
- Cross-tenant reads, writes, exports, background jobs, event routing and support actions are denied.
- Revoked credentials and permissions cannot continue unauthorized operations beyond an approved, tested revocation window.
- Privileged actions enforce approval separation and record the actor, approver, reason and evidence.

### B. Idempotency and concurrency
- Same key and same payload return the same logical operation.
- Same key with different payload is rejected.
- Concurrent requests, worker redelivery, process restart and duplicate callbacks cannot create duplicate provider submissions or postings.
- Uniqueness is enforced durably, not only by in-memory locks.

### C. Unknown outcomes and provider lifecycle
- Timeout before dispatch, timeout after possible dispatch, missing callback, duplicate callback, stale callback and out-of-order callback are distinguished.
- Unknown outcomes trigger safe enquiry/reconciliation and cannot be converted to failure or success by guesswork.
- Cancellation/expiry does not erase an already submitted provider attempt.
- Provider-specific state mappings and finality rules are contract-tested against the applicable documented API version.

### D. Ledger and reconciliation
- Every posting satisfies the approved accounting invariant and has unique source identity.
- Fees, currency precision, rounding, refunds and net settlement match approved examples.
- Duplicate or conflicting provider records are detected.
- Missing/mismatched records create visible owned discrepancy cases; no silent balancing edits.
- Compensating entries preserve original history and require the approved authority.

### E. Callback, webhook and credential security
- Invalid signatures, expired timestamps where supported, replay, malformed payload and unknown provider references do not alter financial state.
- Outbound webhook signing, secret rotation, retry limits, deduplication and dead-letter recovery behave as specified.
- SSRF protections resist private/reserved destinations, redirects and DNS rebinding within the defined threat model.
- Logs and errors redact secrets, tokens, signatures and unnecessary personal data.

### F. Resilience and operations
- Queue redelivery, worker crash, provider outage, database failure and restart recover without duplicate financial effects.
- Backup restoration and reconciliation after restore are exercised using synthetic data.
- Alerts fire for unresolved UNKNOWN operations, reconciliation backlog, callback failure, webhook failure, repeated auth failures and service degradation.
- Incident and manual-resolution runbooks identify authorized roles and prohibit unilateral undocumented ledger edits.

## 6. Evidence acceptance criteria

Evidence is accepted only when:
- It directly supports the stated claim and scope.
- Its source, date, version and limitations are known.
- It has not expired or been superseded.
- Any independent review is performed by a named, suitably qualified reviewer.
- Results are reproducible or the reason they are not reproducible is documented.
- Failures and deviations are retained rather than deleted or summarized away.
- The artifact contains no credentials or unnecessary personal/payment data.
- It is linked to the decision or requirement it closes.

A failed test is useful evidence but does not satisfy the acceptance criterion. A missing test result is not a pass. A provider's public API documentation does not satisfy written eligibility or contract-permission requirements.

## 7. Review workflow

1. Assign an accountable owner for each evidence item and each P0 requirement.
2. Gather public/account-specific provider evidence read-only; request written confirmation only after the product owner explicitly authorizes outreach.
3. Obtain qualified legal/compliance and finance review for the chosen funds-flow model.
4. Review DOC-40–44 and record changes and dissent.
5. Once implementation is separately authorized, create executable tests and synthetic fixtures mapped to stable requirement IDs.
6. Execute tests in controlled environments and attach results to commit/build identifiers.
7. Independently review failures, waivers and retests; no self-approval for required independent gates.
8. Update DOC-34 and DOC-00 only when evidence genuinely changes status.
9. Keep G2, implementation, production and release decisions separate.

## 8. Current conclusion

This document establishes an evidence standard and records the currently known provider and professional-review gaps. It does not close them. G2 remains open, the funds-flow model remains unselected, and all proposed test IDs remain plans until execution evidence is attached.


## 9. Provider adapter evidence cross-reference

Use DOC-48 for the proposed provider capability descriptor and adapter conformance-test plan. Every capability assertion must retain its evidence class and source; public documentation and account observations do not prove HARAI's contractual authority. DOC-48 test IDs are planning references until actual run evidence is recorded under this document's acceptance rules.
