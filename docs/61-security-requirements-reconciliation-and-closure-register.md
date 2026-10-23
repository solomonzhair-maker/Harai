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

## 4A. Direct threat-ID reconciliation (DOC-43 → DOC-60)

This is a source-text mapping for review, not closure. A threat may relate to multiple rows and a row may address only part of a threat. All DOC-43 threats remain Open; all DOC-60 rows remain Draft — evidence absent.

| DOC-43 threat | Directly related DOC-60 row(s) | Reconciliation note |
|---|---|---|
| TH-01 tenant IDOR/BOLA | SEC-E04, SEC-E11, SEC-E15 | Direct mapping for cross-tenant access; privileged support and sensitive-change coverage is partial and needs its own authorization evidence. |
| TH-02 stolen/over-scoped API key | SEC-E01, SEC-E02, SEC-E15 | Direct key theft/scope mapping; key changes also need attributable privileged-change evidence. |
| TH-03 forged/replayed provider callback | SEC-E08 | Direct mapping. |
| TH-04 timeout followed by blind retry | SEC-E09, SEC-E13 | Direct mapping for duplicate effects and investigation of uncertain outcomes. |
| TH-05 race-created duplicate operations/postings | SEC-E09 | Partial mapping; DOC-60 must explicitly include concurrency and atomicity evidence. |
| TH-06 forwarded-header/IP allowlist abuse | SEC-E05, SEC-E06 | Direct mapping. DOC-60 currently cites TH-06/TH-14 for SEC-E05; TH-14 is a browser CSRF/CORS/session threat, not the forwarded-IP threat. Correct SEC-E05's source citation to TH-06 only, and assess browser-session risk under SEC-E03 or a separate control. |
| TH-07 webhook destination SSRF | No dedicated row | Coverage gap: DOC-60 does not have a dedicated row for merchant webhook URL validation, DNS rebinding, redirects and egress restrictions. Add this to the existing matrix in the next consolidated security update; do not treat SEC-E08 as sufficient. |
| TH-08 merchant webhook signing-secret exposure/replay | SEC-E07, SEC-E08 (partial) | Coverage gap/partial: provider callback authentication is not the same as outbound merchant-webhook signing, replay and rotation. Track outbound webhook controls explicitly. |
| TH-09 beneficiary change/payout diversion | SEC-E11, SEC-E15 (partial) | Partial mapping; beneficiary-change authorization, step-up, notification, cooling-off and pending-payout recheck need explicit evidence and finance review. |
| TH-10 privileged support misuse | SEC-E11, SEC-E15 | Direct/partial mapping; require just-in-time scope, case/reason, separation of duties and audit evidence. |
| TH-11 reconciliation import/manual adjustment replay | SEC-E09, SEC-E12, SEC-E13 (partial) | Partial mapping; unique import identity, source integrity and compensating-correction evidence are not explicit enough in the current security matrix. |
| TH-12 secrets/personal data in logs/errors/offline cache | SEC-E07, SEC-E12, SEC-E16 | Direct cross-row mapping; include screenshots, fixtures, exports and offline caches in the evidence review. |
| TH-13 API abuse/resource exhaustion | SEC-E10 | Direct mapping. |
| TH-14 CSRF/CORS/session weaknesses | SEC-E03, SEC-E16 (partial) | Direct session mapping to SEC-E03; SEC-E16 covers disclosure, not CSRF/CORS. Explicit origin/CSRF policy and evidence must remain visible in the control plan. |
| TH-15 worker/job replay after restore/outage | SEC-E09, SEC-E13, SEC-E14 (partial) | Partial mapping; durable job identity, replay after restore and post-restore reconciliation need explicit evidence. |
| TH-16 backup compromise/incomplete restore | No dedicated row | Coverage gap: DOC-60 has no dedicated backup access, restore-integrity, safe worker pause/replay and recovery evidence row. |
| TH-17 dependency/CI/deploy identity compromise | SEC-E07, SEC-E14 (partial) | Partial mapping; provider secret/environment rows do not fully cover dependency provenance, CI permissions, protected branches and deploy identity. |
| TH-18 misrouted cross-tenant webhook/report | SEC-E04, SEC-E12 (partial) | Partial mapping; tenant isolation should explicitly cover event subscriptions, endpoint ownership and per-tenant delivery records. |
| TH-19 unauthorized resolution of contradictory provider evidence | SEC-E09, SEC-E13 (partial) | Partial mapping; discrepancy case ownership, retained evidence and independent correction approval need explicit closure evidence. |
| TH-20 accidental card-data capture | SEC-E07 (partial) | Coverage gap/partial: secret handling is not a card-data boundary assessment. Confirm processor-hosted/tokenized design, data-flow and security scope before any card capability is considered. |

**Audit result:** the existing 16-row DOC-60 matrix does not yet demonstrate complete coverage of the 20-threat DOC-43 model. The clearest missing dedicated rows are TH-07 (outbound webhook SSRF) and TH-16 (backup/restore). TH-08, TH-09, TH-11, TH-15, TH-17, TH-18, TH-19 and TH-20 also require more explicit controls/evidence than generic related rows currently provide. This is a documentation coverage finding, not evidence of an implemented vulnerability or a passed/failed security test.

## 4B. Consolidated corrective actions

1. Correct the SEC-E05 source citation: remove TH-14 as a forwarded-IP threat reference; retain TH-06. Keep TH-14 mapped to browser CSRF/CORS/session controls.
2. Expand DOC-60 in one consolidated update to address TH-07 and TH-16 explicitly and strengthen coverage for TH-08, TH-09, TH-11, TH-15, TH-17, TH-18, TH-19 and TH-20.
3. Verify every DOC-60 requirement reference against the exact DOC-31 requirement text and DOC-34 row before calling the mapping confirmed. Broad labels such as “security requirements” are not exact traceability.
4. Revisit DOC-47 AR-001–AR-020 and retain only source-supported links. No finding is closed by this audit.
5. Name accountable owners/reviewers and obtain required policy decisions before any acceptance evidence is requested. Tests and implementation remain unauthorized.
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