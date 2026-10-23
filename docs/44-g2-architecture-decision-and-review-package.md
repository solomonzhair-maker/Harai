# DOC-44 — G2 Architecture Decision and Review Package

**Status:** Draft for structured review — G2 not approved  
**Date:** 2026-10-19  
**Related documents:** DOC-00, DOC-28, DOC-30, DOC-31, DOC-33, DOC-34, DOC-37, DOC-38, DOC-40, DOC-41, DOC-42, DOC-43, DOC-45, DOC-46, DOC-47  
**Purpose:** Provide one controlled package for reviewing the proposed architecture, funds-flow options, financial safety, external dependencies, and the evidence required before G2 can pass.

## 1. Decision requested

Review whether the proposed logical architecture is sufficiently coherent to proceed to detailed design planning. This review does **not** authorize payment-engine implementation, provider contact or applications, production credentials, live money movement, merge, or deployment.

G2 must not be marked passed merely because documents exist or requirements are mapped. It requires recorded decisions, named accountable reviewers, resolved critical contradictions, and evidence appropriate to the decision.

## 2. Current status snapshot

| Area | Current position | G2 treatment |
|---|---|---|
| Product direction | POD-01–POD-12 defaults approved by product owner | Preserve as product direction; does not establish provider or legal permission |
| Requirements baseline | DOC-31 catalogue and DOC-34 matrix contain 54 unique requirement IDs | Traceability exists; requirement approval and verification remain open |
| Logical architecture | DOC-40 proposal | Reviewable proposal, not implementation authorization |
| Transaction lifecycle | DOC-41 proposal | Provider finality, retry and callback contracts still require evidence |
| Ledger/reconciliation | DOC-42 proposal | Independent finance/accounting review required |
| Threat model | DOC-43 proposal | Independent security review and later technical validation required |
| Funds flow | DOC-28 compares provider-managed, direct, and conditional hybrid models | All remain unscored; no model selected |
| Provider readiness | DOC-37/DOC-38 account observations and public documentation | Written multi-merchant eligibility, production approval, commercial terms and operating rules remain unconfirmed |
| Uganda legal/compliance | DOC-30 checklist | Qualified, appropriately scoped professional review required |
| Independent review | Named reviewers not yet assigned | Blocking for the relevant approval gates |
| Implementation and test evidence | Proposed test IDs only; no payment-engine tests claimed | Must be produced after authorized implementation |

## 3. Architecture decisions to record

Use one record per decision. A decision is not closed until the accountable owner, rationale, supporting evidence, dissent/risks, and effective document references are recorded.

| ID | Decision | Proposed position for review | Evidence needed to close |
|---|---|---|---|
| G2-DEC-01 | Deployment topology | Prefer a modular monolith with isolated modules and separate worker processes as a candidate, not a mandate for microservices | Requirements, expected workload, failure isolation, team and operations assessment |
| G2-DEC-02 | Provider adapter boundary | Provider-specific adapters isolate status, errors, authentication, retries, callbacks and money-out differences | Provider contract matrix and contract-test plan |
| G2-DEC-03 | Transaction state | Retain explicit UNKNOWN state and separate collection, refund/reversal and money-out operations | DOC-41 review; provider finality and retry evidence |
| G2-DEC-04 | Idempotency | Enforce at durable storage boundary with request fingerprint and conflict behavior | Database constraints, API semantics and concurrency test plan |
| G2-DEC-05 | Financial record model | Append-only balanced postings or an explicitly approved equivalent | Finance/accounting review of DOC-42 and chosen funds flow |
| G2-DEC-06 | Reconciliation | Design operation-to-provider and provider-to-settlement matching with owned exceptions | Actual available provider reports/APIs, cadence, settlement semantics and operating owner |
| G2-DEC-07 | Funds-flow model | Do not select until Models A/B/C are evaluated against the same evidence-backed criteria | DOC-28 scorecard, provider permission, legal/compliance advice and commercial terms |
| G2-DEC-08 | Tenant isolation | Enforce server-side tenant context across API, database access, workers, reports, support and callbacks | Threat-model review and negative-test plan |
| G2-DEC-09 | Callback and webhook controls | Authenticated inbound callbacks, durable deduplication, signed outbound events and safe retries | Provider-specific signatures, replay protections, outbound delivery contract and threat review |
| G2-DEC-10 | Money-out availability | Keep disabled unless all relevant provider, funding, approval, legal, reconciliation and operational gates pass | Written authorization and full evidence package |
| G2-DEC-11 | Card payments | Treat as conditional on eligible processor/acquirer, contract, data-flow and compliance review | Processor evidence and applicable security/compliance scope |
| G2-DEC-12 | Production release gate | Require explicit release decision after independent review and verified evidence | DOC-23 release package, test results, incident/restore readiness and named sign-offs |

These positions are recommendations for review, not decisions already approved.

## 4. Funds-flow decision protocol

Compare all three models in DOC-28 using the same criteria and evidence labels:

1. Who legally receives, holds, controls and pays out funds at each stage.
2. Whether the provider contract explicitly permits HARAI's proposed multi-merchant/aggregator use.
3. Merchant onboarding, KYC/KYB, prohibited categories, geographic and currency scope.
4. Collection and money-out availability, funding source, limits, refunds, reversals and dispute responsibility.
5. Settlement recipient, cadence, timing, deductions, reserves, prefunding and failed-settlement handling.
6. Fee schedule, tax treatment, fee payer, refunds and net-settlement calculations.
7. Reconciliation data, report access, reference quality, discrepancy process and support escalation.
8. Data protection, security, audit, complaint handling and applicable regulatory obligations.
9. Commercial viability, provider dependency, operational workload and portability.
10. Implementation complexity, failure modes, recovery behavior and evidence gaps.

Scoring rules:
- Mark each criterion **Supported**, **Partially supported**, **Unknown**, **Contradicted**, or **Not applicable**, with a citation/evidence reference.
- Do not turn unknowns into zero-cost assumptions or score unsupported claims as favorable.
- If a critical legal authority or contractual permission is absent, that model remains blocked regardless of its numerical score.
- Keep the decision open when models cannot be compared on equivalent evidence.
- Record rejected alternatives and the reason; do not retroactively rewrite the evidence.

## 5. G2 exit criteria

G2 may be considered for approval only when all applicable criteria below are satisfied or an explicitly authorized exception is recorded without weakening a hard safety/legal gate.

- [ ] Architecture boundaries and deployment approach reviewed by a named technical owner and independent reviewer.
- [ ] State-machine transitions, ambiguous outcomes, retry rules and finality requirements reviewed against provider evidence.
- [ ] Funds-flow model selected only after the evidence-backed comparison and qualified legal/compliance review.
- [ ] Ledger, fees, refunds, settlement and reconciliation semantics reviewed by an appropriately qualified finance/accounting reviewer.
- [ ] Tenant isolation, identity, credentials, callbacks, outbound webhooks, SSRF, privileged access and incident risks reviewed by an independent security reviewer.
- [ ] Every P0 requirement has a proposed design/control, owner, dependency, and verification approach; no unacknowledged critical gap.
- [ ] Provider-specific assumptions are labelled and linked to authoritative evidence; unresolved assumptions have an owner and block affected capabilities.
- [ ] Test IDs are clearly distinguished from executed tests; required evidence and pass/fail rules are defined.
- [ ] Operational ownership, discrepancy escalation, incident handling, backup/restore, and manual-resolution authority are defined at a design level.
- [ ] DOC-44 decision records and DOC-45 evidence register are updated with dates, accountable people, and review outcomes.
- [ ] Product owner explicitly records G2 outcome; this does not by itself authorize implementation or production release.

## 6. Blocking conditions that cannot be waived by documentation

- No evidence of contractual/legal authority for the proposed multi-merchant funds flow.
- No safe way to determine or reconcile an ambiguous money-moving outcome.
- No approved ledger/accounting semantics or controlled correction process.
- Unresolved critical tenant-isolation or credential/callback security issue.
- Money-out path without confirmed funding source, limits, authorization and return/reconciliation handling.
- Production method enabled without required provider approval and merchant eligibility.
- No accountable person authorized to resolve operational financial discrepancies.
- A material claim represented as verified when it is only proposed or inferred.

## 7. Review roles and independence

Names are intentionally unassigned until the product owner identifies suitable people. Role labels are not evidence that a reviewer exists.

| Review role | Required contribution | Independence expectation |
|---|---|---|
| Product owner | Confirm scope, trade-offs, priorities and decision outcome | Owns product decisions, not independent technical assurance |
| Technical architecture reviewer | Challenge service boundaries, consistency, failure handling and operability | Should not be the sole author of the proposal |
| Finance/accounting reviewer | Approve ledger semantics, fees, refunds, settlement and reconciliation controls | Appropriately qualified; independent of sole implementation author |
| Security reviewer | Review threat model, controls, trust boundaries and validation plan | Independent review; later testing still required |
| Uganda legal/compliance adviser | Review proposed role in funds flow, provider permission, obligations and launch conditions | Qualified for the relevant jurisdiction and subject |
| Operations owner | Own monitoring, exception queues, incidents, restore and reconciliation procedures | Explicitly accountable for day-to-day operation |

One person may hold more than one role only where independence and conflicts are assessed and documented. ChatGPT's drafting or self-review is not an external sign-off.

## 8. Decision log template

Copy for each decision:

- **Decision ID:**
- **Question and scope:**
- **Options considered:**
- **Selected option / status:**
- **Accountable decision owner:**
- **Required reviewers and their review status:**
- **Evidence references and evidence quality:**
- **Assumptions and unresolved unknowns:**
- **Security/financial/legal impact:**
- **Consequences and migration/reversal plan:**
- **Date, version and next review trigger:**

## 9. Current conclusion

G2 is **not approved**. The material design proposals are now sufficiently organized for structured review, but written provider authorization, the funds-flow decision, qualified finance/legal/security review, named accountable owners, and verification evidence remain outstanding. The next work should close evidence and decision gaps before any implementation gate is requested.


## 10. Detailed architecture review register

DOC-47 records the cross-document review findings AR-001–AR-021, distinguishing confirmed inconsistencies from design gaps, unverified assumptions, external blockers, evidence gaps and decisions needed. AR-019 is closed only for the index/cross-link task; AR-021 remains open for idempotency-scope alignment. Its findings must be dispositioned with evidence; the register does not pass G2 or authorize implementation. Use DOC-46 for P0 closure ownership/evidence tracking and DOC-45 for evidence quality and test-result discipline.
