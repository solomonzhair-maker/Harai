# DOC-61 — Security Requirements Reconciliation and Closure Register

**Status:** Draft reconciliation tracker; no security control is approved or verified by this document.
**Gate status:** G1 NOT PASSED. G2 NOT APPROVED.

## 1. Purpose and source authority

Reconcile DOC-60 security assurance rows against the normative requirements in DOC-31, threat model DOC-43, requirement tracker DOC-34, architecture findings DOC-47, policy proposal DOC-59, and evidence rules DOC-45.

- DOC-31 owns requirement IDs, wording, priority and acceptance criteria.
- DOC-43 owns threat identifiers TH-01–TH-20 and threat rationale.
- DOC-47 owns architecture findings AR-001–AR-020 and their closure status.
- DOC-34 tracks all 54 requirement IDs; it is not an approval or verification record.
- DOC-59 is a policy proposal. DOC-60 is an evidence-planning matrix. Neither proves a control exists or works.
- Where mappings differ or remain uncertain, record the discrepancy; do not silently resolve it or infer closure.

> Documentation only. This register authorizes no implementation, tests, provider contact, production credentials, real financial activity, merge or deployment.

## 2. Reconciliation overview

| Source | Current known state | Required action | Status |
|---|---|---|---|
| DOC-31 | 54 unique requirement IDs, including SUP-001–SUP-010 and EXT-004 | Confirm security-relevant wording and acceptance criteria with accountable reviewers; preserve DOC-31 as source of truth | Open |
| DOC-43 | 20 threat IDs, TH-01–TH-20 | Check every DOC-60 threat reference against exact source descriptions, trust boundaries and assumptions | Open |
| DOC-34 | Requirement-level matrix covers all 54 IDs; role assignments are proposals | Confirm every DOC-60 requirement link and record any missing or conflicting mapping | Open |
| DOC-47 | 20 architecture findings, AR-001–AR-020 | Review each finding and link only those whose source text supports a security/financial-safety relationship | Open |
| DOC-60 | 16 rows SEC-E01–SEC-E16; evidence absent by default | Reconcile IDs, assign named owners/reviewers, approve policy decisions, then gather authorized evidence | Draft — evidence absent |
| Accountability | Roles proposed; named individual owners/reviewers not established | Product owner assigns named control owners and independent security reviewer, plus relevant finance/privacy/operations reviewers | Blocked |
| Security policy | Key lifecycle, identity/session, proxy trust, allowlist defaults, rate limits, secret management and callback verification remain proposals/open choices | Record each accountable decision, rationale and affected IDs in DOC-00 | Open |

## 3. Control-family register

| Control family | DOC-31 requirements to review | DOC-60 rows | Threat mapping status | Architecture linkage | Minimum closure evidence |
|---|---|---|---|---|---|
| Tenant isolation and object authorization | TEN-001, IAM-001, MER-002, SEC-002 | SEC-E04 | Exact TH IDs must be reconciled against DOC-43 | Review AR-001–AR-020; link only supported findings | Approved tenant boundary and negative evidence for API, dashboard, jobs, exports and support paths |
| API keys, scopes and revocation | DEV-001, IAM-001, IAM-002, SUP-003 | SEC-E01, SEC-E02, SEC-E15 | Exact TH IDs must be reconciled | Review source findings on authorization/credential boundaries | Approved lifecycle/scope policy, denied-operation evidence, revocation results and audit trail |
| Dashboard identity, sessions and recovery | SEC-001, IAM-001, IAM-002 | SEC-E03 | Exact TH IDs must be reconciled | Link applicable source findings only | Approved identity/session/recovery policy and normal, privileged and recovery-path evidence |
| Proxy trust and IP allowlisting | SEC-003, SUP-007 | SEC-E05, SEC-E06 | DOC-60 cites TH-06 and TH-14; confirm against DOC-43 | Link applicable source findings only | Approved trusted-proxy design, spoofed-header evidence, allowlist policy and lockout-safe recovery; authentication remains required |
| Provider secrets and environment separation | DEV-001, SEC-002, OPS-002, EXT-004 where applicable | SEC-E07, SEC-E14 | Exact TH IDs must be reconciled | Link applicable source findings only | Secret-flow/access review, environment separation and redaction/rotation evidence; provider authority is separately verified |
| Callback/webhook authenticity and replay | COL-005, DEV-002, COL-004, FIN-005 | SEC-E08 | Exact TH IDs must be reconciled | Link applicable source findings only | Provider protocol evidence; signature/replay/duplicate and invalid-transition evidence |
| Idempotency, unknown outcomes and abuse limits | COL-003, COL-004, COL-008, OUT-002, SUP-001 | SEC-E09, SEC-E10 | DOC-60 cites TH-13; confirm against DOC-43 | Link applicable source findings only | Approved idempotency/retry/limit policy and authorized duplicate, concurrency, timeout and recovery evidence |
| Privileged access and sensitive changes | IAM-002, OUT-005, OUT-006, SUP-003, SUP-010 | SEC-E11, SEC-E15 | Exact TH IDs must be reconciled | Link applicable source findings only | Approved role/approval matrix, grant/revoke evidence and attributable audit trail |
| Audit, privacy and error disclosure | SEC-002, DEV-003, SUP-010 | SEC-E12, SEC-E16 | Exact TH IDs must be reconciled | Link applicable source findings only | Redacted audit samples, access/retention policy, sanitized error examples and privacy review |
| Incident response and financial investigation | OPS-001, OPS-002, REC-001, REC-002, OUT-005 | SEC-E13 | Exact TH IDs must be reconciled | Link applicable source findings only | Approved runbooks, named incident owners, evidence preservation and unknown-payment investigation walkthrough |
| Card-data boundary/provider exposure | EXT-004, DEV-001, SEC-002, OPS-002 | SEC-E07, SEC-E14 | Exact TH IDs must be reconciled | Link applicable source findings only | Eligible processor/authority evidence, reviewed data boundary and PCI/security scope; no unsupported card-handling claims |

**Mapping caution:** The family-to-finding links above are review pointers, not assertions that every finding applies. Inspect the source text before retaining an exact relationship. Never close an AR finding from this register; DOC-47 remains authoritative for its status.

## 4. DOC-60 row-by-row checklist

| DOC-60 row | Concern | Requirement references to verify | Closure condition |
|---|---|---|---|
| SEC-E01 | Stolen/leaked merchant key | DEV-001, SEC-001, SUP-003 | Approved key lifecycle, misuse/revocation evidence and independent review |
| SEC-E02 | Excessive permissions | IAM-001, IAM-002, MER-002 | Every protected operation has explicit authorization; denied cases evidenced |
| SEC-E03 | Session theft/account takeover | SEC-001, IAM-001 | Approved identity/recovery policy and session invalidation evidence |
| SEC-E04 | Cross-tenant access | TEN-001, IAM-001, SEC-002 | Negative evidence across applicable access paths; any cross-tenant access blocks closure |
| SEC-E05 | Proxy/IP spoofing | SEC-003, SUP-007 | Confirm DOC-60's TH-06/TH-14 references; prove untrusted headers cannot alter effective source address |
| SEC-E06 | Allowlist lockout/false confidence | SEC-003, SUP-007 | Approved policy and recovery; authentication, scope and tenant checks remain enforced |
| SEC-E07 | Provider-secret exposure | DEV-001, SEC-002, EXT-004 if applicable | Secret access, environment separation and redaction evidenced |
| SEC-E08 | Forged/replayed callback | COL-005, DEV-002, COL-004, FIN-005 | Authentication, deduplication and state-transition rules evidenced |
| SEC-E09 | Duplicate effects/ambiguous timeout | COL-003, COL-004, OUT-002, FIN-005 | Authorized duplicate/concurrency/unknown-outcome evidence and finance review |
| SEC-E10 | Brute force/API abuse | COL-008, SUP-001 | Confirm DOC-60's TH-13 reference; approve thresholds and evidence alert/recovery behavior |
| SEC-E11 | Insider/workload misuse | IAM-001, IAM-002, SEC-002, SUP-010 | Named roles, least privilege, revocation and privileged audit evidence |
| SEC-E12 | Audit gaps/sensitive log content | SEC-002, SUP-010, FIN-002 | Investigation-quality logs without exposing secrets or unnecessary personal data |
| SEC-E13 | Delayed compromise response/uncertain payment | OPS-001, OUT-002, REC-001, REC-002 | Approved incident/financial-investigation runbooks and named owners |
| SEC-E14 | Cross-environment credentials/data | DEV-001, SEC-002, OPS-002 | Wrong-environment use is rejected and evidenced |
| SEC-E15 | Unauthorized sensitive configuration change | IAM-002, OUT-005, OUT-006, SUP-003, SUP-010 | Approved action-to-role matrix and attributable audit trail |
| SEC-E16 | Error-response information leakage | DEV-003, SEC-002 | Sanitized external errors and redacted, access-controlled diagnostic logs |

## 5. Architecture findings and discrepancy handling

DOC-47 contains AR-001–AR-020. Review every finding and classify its relationship to this register as **applicable**, **partially applicable**, **not applicable** or **unclear**. Record the source section and rationale. AR-001 explicitly warns that traceability coverage is not requirement acceptance or verification. Keep each finding's source status unchanged unless DOC-47 is updated through its own closure process.

For each mismatch among DOC-31, DOC-34, DOC-43, DOC-47, DOC-59 and DOC-60, record: source IDs and sections; what differs or is missing; consequence; proposed owner role; decision required; evidence needed; and status. Keep unknown mappings open rather than inventing threat IDs.

## 6. Closure workflow

1. Confirm current versions of all source documents and inventory exact requirement, threat and finding IDs.
2. Reconcile every SEC-E row to DOC-43 and DOC-31; confirm requirement links against DOC-34. Mark each mapping confirmed, partial, missing, conflicting or not applicable, with rationale.
3. Inspect AR-001–AR-020; keep only source-supported links and preserve the source finding's status.
4. Assign named control owners and an independent reviewer. Role labels alone are not sufficient for closure.
5. Record approved control decisions in DOC-00; leave external authority and evidence blockers explicit.
6. Define evidence under DOC-45: version, environment, method, expected/actual result, negative/failure paths, redaction review and residual risk.
7. Obtain separate explicit authorization before any implementation or test activity. This register grants none.
8. Close only after required evidence exists and accountable reviewers accept it; otherwise keep the item open or blocked.

Closure fields: register row; source IDs; mapping status; decision ID; named owner; independent reviewer; design version; evidence reference; environment/date; expected/actual outcome; failure-path coverage; privacy/redaction review; residual risk/expiry; reviewer decision; next action.

## 7. Status vocabulary

- **Open — source reconciliation required:** source links or consistency checks remain incomplete.
- **Draft — evidence absent:** only the control/evidence plan exists.
- **Blocked:** owner, authority, decision, provider/legal evidence or authorized environment is missing.
- **Implemented — not verified:** use only when a traceable implementation exists but acceptance evidence is incomplete.
- **Verified — reviewer accepted:** complete evidence packet and named independent review are recorded.
- **Exception accepted — time-bounded:** named risk owner, approval authority, compensating controls and expiry/review date are documented.

Document existence, planned test IDs, code review or an unexecuted test does not justify verification.

## 8. Related documents and gate

DOC-00 — Master index/decision register; DOC-31 — launch requirements; DOC-34 — requirement traceability; DOC-43 — threat model; DOC-45 — evidence rules; DOC-47 — architecture findings; DOC-59 — security-policy proposal; DOC-60 — threat-to-control matrix.

**G1 remains NOT PASSED. G2 remains NOT APPROVED.** This register is a planning/reconciliation artifact, not security certification, completed testing, or permission to implement, contact providers, generate production credentials, transact, merge or deploy.