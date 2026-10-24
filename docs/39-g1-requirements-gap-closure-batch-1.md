# DOC-39 — G1 Requirements Gap Closure Batch 1

**Status:** Draft working package — ten supplemental requirements added and scope dispositions recorded 2026-10-24; G1 remains open  
**Date:** 2026-10-19  
**Related documents:** DOC-00, DOC-03, DOC-31, DOC-32, DOC-33, DOC-34, DOC-35, DOC-38  
**Purpose:** Convert identified internal requirements gaps into stable, testable requirements so the project can progress without waiting for external provider, commercial or legal answers. This document records work completed and the remaining controlled follow-through; it is not implementation authorization or evidence of tests.

## 1. Work completed in this batch

DOC-31 now includes ten supplemental requirements, SUP-001 through SUP-010, for:
- API abuse controls and rate limiting;
- payment-link/session lifecycle and concurrent usage;
- beneficiary and payout-destination protection;
- canonical transaction state transitions and finality;
- dispute and complaint case handling;
- API versioning, stable errors, pagination and correlation IDs;
- CORS, CSRF, trusted-proxy and IP-allowlist boundaries;
- measurable reliability, backups and recovery;
- accessibility and responsive critical journeys;
- end-to-end event correlation and safe audit logging.

The working requirement catalogue now contains 54 unique IDs: the original 44 (including EXT-004 for card payments) plus these 10 additions. The accountable requirements reviewer recorded acceptance of SUP-001–SUP-010 for retention in the intended launch baseline on 2026-10-24; this is a scope disposition only. The detailed criteria remain draft proposals pending applicable product, specialist and architecture review, and are not implemented or verified. Proposed test identifiers are planning references only; they do not mean test files exist or have been run.

## 2. What this unlocks

The team can now review these gaps as explicit requirements rather than informal checklist items. Architecture work can use the IDs to define state machines, edge-security boundaries, observability, recovery, and protected payout-destination changes. Engineering estimation can be organized by requirement and dependency once architecture and implementation gates are authorized.

## 3. Follow-through required

| Action | Responsible role (not a named appointment) | Evidence of completion | Status |
|---|---|---|---|
| Keep DOC-34 requirement-level matrix aligned to all 54 IDs | Requirements owner / technical lead | Every ID has proposed role owner, test/evidence reference, dependency and gate | Open |
| Cross-map SUP-001–SUP-010 to DOC-03 phases | Technical lead / delivery owner | Roadmap links each requirement to design, authorized implementation, verification and release gate | Open |
| Review and disposition each supplemental requirement | Product owner and accountable reviewers | DOC-34 §8.7 records all ten SUP-001–SUP-010 as accepted for retention in the intended launch baseline; normative criteria unchanged | Disposition recorded; verification and specialist review open |
| Set numeric rate limits, expiry behavior, RTO/RPO, reliability and retention targets | Product owner with technical, operations, risk and privacy reviewers | Recorded decisions with rationale and measurable acceptance criteria | Open |
| Review funds-flow, provider and legal dependencies separately | Product owner and qualified external/accountable reviewers | Written evidence or explicitly blocked disposition | Open |
| Preserve implementation, provider-contact, merge and deployment gates | Product owner / repository governance | No consequential action without separate explicit authorization | Ongoing |

## 4. External dependencies remain isolated

The supplemental requirements do not require HARAI to wait for provider replies before the internal design work can proceed. Provider-specific state semantics, quotas, refund eligibility, money-out funding, written multi-merchant permission, settlement terms, card-acquirer selection and qualified Ugandan legal/compliance conclusions must remain marked as dependencies where applicable. Public documentation must not be treated as contractual permission.

## 5. Gate decision

**G0 remains in progress; G1 has not passed.** This batch improves completeness and records the 54-ID scope disposition (51 Accept / 3 Defer), but does not constitute G1 passage, approve final design or release readiness, select a funds-flow model, or authorize implementation, further provider outreach, live credentials, real transactions, merge or deployment.

Follow-through: keep DOC-34 and DOC-03 aligned as requirements change, conduct a focused consistency check, and prepare the expanded baseline for accountable review.


## Traceability correction

A consistency check found that the existing EXT-004 card-payment requirement in DOC-31 had not been represented in DOC-34. The matrix now includes EXT-004 as blocked pending eligible processor/acquirer selection, written permission, security-scope review and sandbox evidence. The consistent count is therefore 54 unique IDs: 44 existing requirements including EXT-004, plus SUP-001–SUP-010.

## Scope disposition reconciliation — 2026-10-24

The accountable requirements reviewer accepted SUP-001–SUP-010 for retention in the intended launch requirements baseline as part of the 54-ID batch decision recorded in DOC-34 §8.7. This includes all eight P0 supplemental requirements (SUP-001–SUP-004, SUP-006–SUP-008 and SUP-010) and the two P1 supplemental requirements (SUP-005 and SUP-009). This decision does not set the still-open numeric parameters, close external dependencies, prove implementation or tests, or pass G1/G2.
