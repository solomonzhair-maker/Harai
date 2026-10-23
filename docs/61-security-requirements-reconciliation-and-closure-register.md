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
| DOC-31 | 54 unique requirement IDs, including SUP-001–SUP-010 and EXT-004 | Confirm exact wording and acceptance criteria; keep proposed new criteria separate until an accountable owner approves a DOC-31 change | Open |
| DOC-43 | 20 threat IDs, TH-01–TH-20 | Confirm every threat ID and the actual coverage/limitations of linked control rows | Open |
| DOC-34 | Requirement-level matrix covers all 54 IDs; role assignments are proposals | Record explicit, partial and missing acceptance-criteria coverage for each DOC-60 row | Open |
| DOC-47 | 20 architecture findings, AR-001–AR-020 | Review source findings; link only source-supported relationships and do not close findings here | Open |
| DOC-60 | 21 rows SEC-E01–SEC-E21; evidence absent by default | Confirm exact requirement mappings, retain gaps, assign named owners/reviewers and collect evidence only after authorization | Draft — evidence absent |
| Accountability | Roles proposed; named individual owners/reviewers not established | Product owner assigns named control owners and independent security reviewer, plus relevant finance/privacy/operations reviewers | Blocked |
| Security policy | Key lifecycle, identity/session, proxy trust, allowlist defaults, rate limits, secret management and callback verification remain proposals/open choices | Record each accountable decision, rationale and affected IDs in DOC-00 | Open |

## 3. Control-family register

| Control family | DOC-31 requirements to review | DOC-60 rows | Threat mapping status | Architecture linkage | Minimum closure evidence |
|---|---|---|---|---|---|
| Tenant isolation and object authorization | TEN-001, IAM-001, MER-002, SEC-002 | SEC-E04 | Exact TH IDs must be reconciled against DOC-43 | Review AR-001–AR-020; link only supported findings | Approved tenant boundary and negative evidence for API, dashboard, jobs, exports and support paths |
| API keys, scopes and revocation | DEV-001, IAM-001, IAM-002, SUP-003 | SEC-E01, SEC-E02, SEC-E15 | Exact TH IDs must be reconciled | Review source findings on authorization/credential boundaries | Approved lifecycle/scope policy, denied-operation evidence, revocation results and audit trail |
| Dashboard identity, sessions and recovery | SEC-001, IAM-001, IAM-002 | SEC-E03 | Exact TH IDs must be reconciled | Link applicable source findings only | Approved identity/session/recovery policy and normal, privileged and recovery-path evidence |
| Proxy trust and IP allowlisting | SEC-003, SUP-007 | SEC-E05, SEC-E06 | SEC-E05 maps to TH-06 only; TH-14 maps to browser CSRF/CORS/session controls | Link applicable source findings only | Approved trusted-proxy design, spoofed-header evidence, allowlist policy and lockout-safe recovery; authentication remains required |
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
| SEC-E05 | Proxy/IP spoofing | SEC-003, SUP-007 | TH-06 only; verify untrusted headers cannot alter effective source address |
| SEC-E06 | Allowlist lockout/false confidence | SEC-003, SUP-007 | Approved policy and recovery; authentication, scope and tenant checks remain enforced |
| SEC-E07 | Provider-secret exposure | DEV-001, SEC-002, EXT-004 if applicable | Secret access, environment separation and redaction evidenced |
| SEC-E08 | Forged/replayed callback | COL-005, DEV-002, COL-004, FIN-005 | Authentication, deduplication and state-transition rules evidenced |
| SEC-E09 | Duplicate effects/ambiguous timeout | COL-003, COL-004, OUT-002, FIN-005 | Authorized duplicate/concurrency/unknown-outcome evidence and finance review |
| SEC-E10 | Brute force/API abuse | SEC-001, COL-008, SUP-001 | TH-13; approve thresholds and evidence alert/recovery behavior |
| SEC-E11 | Insider/workload misuse | IAM-001, IAM-002, SEC-002, SUP-010 | Named roles, least privilege, revocation and privileged audit evidence |
| SEC-E12 | Audit gaps/sensitive log content | SEC-002, SUP-010, FIN-002 | Investigation-quality logs without exposing secrets or unnecessary personal data |
| SEC-E13 | Delayed compromise response/uncertain payment | OPS-001, OUT-002, REC-001, REC-002 | Approved incident/financial-investigation runbooks and named owners |
| SEC-E14 | Cross-environment credentials/data | DEV-001, SEC-002, OPS-002 | Wrong-environment use is rejected and evidenced |
| SEC-E15 | Unauthorized sensitive configuration change | IAM-002, OUT-005, OUT-006, SUP-003, SUP-010 | Approved action-to-role matrix and attributable audit trail |
| SEC-E16 | Error-response information leakage | DEV-003, SEC-002, TEN-001 | Sanitized external errors and redacted, access-controlled diagnostic logs |
| SEC-E17 | Outbound webhook SSRF | DEV-002 and SEC-002 are candidate links only; neither explicitly requires SSRF-safe DNS/redirect/egress handling | DOC-31 acceptance-criteria gap; owner decision needed before adding explicit criterion |
| SEC-E18 | Backup/restore compromise, integrity and replay | OPS-001, OPS-003, SEC-002, FIN-002 partly support restore, protection and ledger integrity; safe worker pause/replay is not explicit | Partial coverage; add explicit post-restore integrity and consequential-job replay criterion only after owner approval |
| SEC-E19 | Cross-tenant webhook/report delivery | TEN-001, DEV-002, REP-001 directly cover tenant isolation, webhooks and reports; endpoint ownership/dispatch-time revalidation detail is not explicit | Core coverage exists; decide whether endpoint ownership and dispatch-time tenant recheck need explicit wording |
| SEC-E20 | Unauthorized resolution of contradictory provider evidence | REC-001, REC-002, FIN-002, FIN-005, IAM-002 and OUT-005 cover matching, discrepancies, corrections, state rules, approvals and audit | Core coverage exists; confirm independent approval for consequential manual resolution and preservation of source evidence |
| SEC-E21 | Accidental card-data capture | EXT-004 and SEC-002 address card-data boundaries and sensitive-data protection; screenshots, fixtures, analytics and support exports are not all named | Partial coverage; decide whether evidence surfaces should be enumerated in EXT-004/SEC-002 |

## 4A. Direct threat-ID reconciliation (DOC-43 → DOC-60)

This is a source-text mapping for review, not closure. A threat may relate to multiple rows and a row may address only part of a threat. All DOC-43 threats remain Open; all DOC-60 rows remain Draft — evidence absent.

| DOC-43 threat | Directly related DOC-60 row(s) | Reconciliation note |
|---|---|---|
| TH-01 tenant IDOR/BOLA | SEC-E04, SEC-E16, SEC-E19 | Direct support from TEN-001; E19 adds explicit webhook/report delivery detail. Threat remains Open. |
| TH-02 stolen/over-scoped API key | SEC-E01, SEC-E02, SEC-E07, SEC-E15 | DEV-001 and IAM-001/IAM-002 support lifecycle and authorization; deployment/workload credential risks need separate coverage. |
| TH-03 forged/replayed provider callback | SEC-E08, SEC-E13 | COL-005 explicitly covers invalid authentication, duplicate/replay/stale/out-of-order events; provider-specific evidence remains external. |
| TH-04 timeout followed by blind retry | SEC-E09, SEC-E13 | COL-004 and OUT-002 cover unknown outcomes and payout uncertainty; recovery evidence absent. |
| TH-05 race-created duplicate operations/postings | SEC-E09 | COL-003, FIN-002 and OUT-002 support idempotency/financial invariants; concurrency evidence is planned, not run. |
| TH-06 forwarded-header/IP allowlist abuse | SEC-E05, SEC-E06 | SEC-003 and SUP-007 explicitly cover trusted proxies, arbitrary forwarded headers, allowlist changes and recovery. E05 cites TH-06 only. |
| TH-07 webhook destination SSRF | SEC-E17 | DOC-31 DEV-002 now explicitly requires destination policy, IPv4/IPv6 internal-target rejection, DNS/redirect revalidation, DNS-rebinding defenses and egress restrictions. Implementation/evidence remain absent; no security control is verified. |
| TH-08 merchant webhook signing-secret exposure/replay | SEC-E07, SEC-E08, SEC-E15 | DEV-001 covers secret lifecycle; DEV-002 covers signatures/timestamps, retries and dedupe. Rotation/replay evidence and inbound/outbound separation remain explicit evidence needs. |
| TH-09 beneficiary change/payout diversion | SEC-E15 | SUP-003, OUT-006 and IAM-002 cover destination verification/step-up/approval and pending-payout safety. Cooling-off/notification remains a policy decision. |
| TH-10 privileged support misuse | SEC-E02, SEC-E11, SEC-E15 | TEN-001 covers support tools in tenant isolation; IAM-001/IAM-002 and SEC-002 support least privilege and restricted/auditable support access. JIT/case scoping may need design decisions. |
| TH-11 reconciliation import/manual adjustment replay | SEC-E09, SEC-E12, SEC-E20 | REC-001/REC-002/FIN-002 now require stable import identity, content integrity, rejection/quarantine of altered content under a reused ID, provenance and linked correction history. Implementation/evidence remain absent. |
| TH-12 secrets/personal data in logs/errors/offline cache | SEC-E07, SEC-E12, SEC-E16, SEC-E21 | SEC-002 and DEV-003 now enumerate traces, analytics, screenshots, fixtures, offline caches, support exports, diagnostic attachments and build artifacts. Implementation/evidence remain absent. |
| TH-13 API abuse/resource exhaustion | SEC-E10 | SUP-001, COL-008 and SEC-001 cover scoped limits, abuse and brute-force protection. Numeric thresholds require owner/technical decision. |
| TH-14 CSRF/CORS/session weaknesses | SEC-E03, SEC-E05, SEC-E06 | SEC-001, SEC-003 and SUP-007 cover sessions, CORS/CSRF, trusted proxies and recovery. TH-14 is not a forwarded-IP-only threat. |
| TH-15 worker/job replay after restore/outage | SEC-E09, SEC-E13, SEC-E18 | OPS-001, OPS-003 and FIN-002 now explicitly require consequential workers/replay jobs to remain paused until restored data, ledger, queues, transactions and provider references are validated and reconciled, with an authorized resume record. Evidence remains absent. |
| TH-16 backup compromise/incomplete restore | SEC-E18, SEC-E13 | OPS-001/OPS-003/SEC-002 now require documented backup access, protection, encryption/key handling, retention and restore integrity review. Evidence remains absent. |
| TH-17 dependency/CI/deploy identity compromise | SEC-E07, SEC-E11, SEC-E14 | OPS-002 and DEV-001 now require least-privilege attributable CI/deployment identities, scoped credentials, protected release configuration, dependency provenance/security review and auditable artifact/release evidence. Implementation/evidence remain absent. |
| TH-18 misrouted cross-tenant webhook/report | SEC-E04, SEC-E08, SEC-E19 | TEN-001, DEV-002 and REP-001 now require server-derived ownership and dispatch/execution-time revalidation for webhook attempts/retries/replays and report/export paths. Endpoint-change policy and implementation evidence still require review. |
| TH-19 unauthorized resolution of contradictory provider evidence | SEC-E09, SEC-E13, SEC-E20 | REC-001/REC-002/FIN-002/FIN-005/IAM-002 now require preserved source evidence, approved authority, audit history and linked corrections; independent approval applies where the approved risk policy requires it. Policy/evidence remain open. |
| TH-20 accidental card-data capture | SEC-E07, SEC-E12, SEC-E16, SEC-E21 | EXT-004 and SEC-002 now enumerate logs, traces, analytics, fixtures, screenshots, support tools/exports and backups, and retain the card capability gate pending qualified scope review and permission. Implementation/evidence remain absent. |

**Audit result (updated 2026-10-23):** DOC-60 contains 21 proposed control/evidence rows and references all 20 DOC-43 threats. The eight previously identified requirement gaps/partial gaps have been addressed in DOC-31 under existing requirement IDs following product-owner approval of RCP-01–RCP-08. The matrix still represents proposed controls and planned evidence only: no control is verified, no threat is closed, and no security test has been run by this documentation change.

## 4B. Consolidated corrective actions

1. SEC-E05's citation is corrected to TH-06 only; TH-14 maps to browser/session controls.
2. DOC-60 has 21 proposed rows and references all 20 threats, but row existence does not prove complete requirement coverage.
3. Keep exact existing DOC-31 links where criteria substantively support the control; mark indirect/incomplete mappings as partial.
4. RCP-01–RCP-08 are approved and incorporated into DOC-31; reconcile evidence rows and acceptance plans to the final wording, while keeping all evidence and control statuses open.
5. Endpoint ownership/dispatch-time tenant binding and manual discrepancy-resolution authority are now explicit in DOC-31; implementation design, policy details and evidence remain to be reviewed.
6. Review DOC-47 AR-001–AR-020 using source-supported links only. No finding is closed by this audit.
7. Name accountable owners/reviewers before requesting acceptance evidence. Tests and implementation remain unauthorized.
## 4C. Requirement-coverage disposition after approved changes

The product owner approved RCP-01–RCP-08 on 2026-10-23, and the associated criteria are now incorporated into DOC-31. The table below records normative coverage only. Every associated implementation/control and evidence item remains unverified.

| Threat / row | DOC-31 coverage after approved update | Current disposition |
|---|---|---|
| TH-01 / SEC-E04 | TEN-001; IAM-001; SEC-002; REP-001 where applicable | Tenant isolation criteria exist; cross-tenant evidence absent. |
| TH-02 / SEC-E01–E02 | DEV-001; IAM-001; IAM-002; SEC-001 | Credential lifecycle, least privilege and approval criteria exist; evidence absent. |
| TH-03 / SEC-E08 | COL-005; FIN-005; DEV-002 | Callback authenticity, state validation, signatures and deduplication criteria exist; provider-specific evidence absent. |
| TH-04–TH-05 / SEC-E09 | COL-003; COL-004; OUT-002; FIN-002; FIN-005 | Idempotency, unknown outcomes and financial history criteria exist; execution evidence absent. |
| TH-06 / SEC-E05–E06 | SEC-003; SUP-007 | Trusted-proxy and allowlist trust-boundary criteria exist; deployment configuration/evidence absent. |
| TH-07 / SEC-E17 | DEV-002; SEC-002 | Approved criteria now cover SSRF-safe destination policy, DNS/redirect validation and egress restrictions; evidence absent. |
| TH-08 / SEC-E08 | DEV-001; DEV-002; COL-005 | Secret lifecycle, signatures/timestamps, retries and dedupe are covered; provider-specific key lifecycle evidence remains open. |
| TH-09 / SEC-E15 | SUP-003; OUT-006; IAM-002; OUT-005 | Beneficiary verification, approval and audit criteria exist; detailed risk-policy choices remain open. |
| TH-10 / SEC-E11 | TEN-001; IAM-001; IAM-002; SEC-002; GOV-001; SUP-010 | Support isolation, least privilege and restricted/auditable access are covered; named owners and evidence remain open. |
| TH-11 / SEC-E09, E12, E20 | FIN-002; REC-001; REC-002; OUT-005 | Approved criteria now cover import identity/integrity, altered-content replay handling, provenance and correction history; evidence absent. |
| TH-12 / SEC-E07, E12, E16, E21 | SEC-002; DEV-001; DEV-003; SUP-010; EXT-004 | Approved criteria now enumerate sensitive diagnostic, offline and support artefact surfaces; evidence absent. |
| TH-13 / SEC-E10 | SUP-001; COL-008; SEC-001 | Abuse and brute-force criteria exist; numeric limits require separate owner/technical decision. |
| TH-14 / SEC-E03, E05–E06 | SEC-001; SEC-003; SUP-007 | Browser/session/CORS/CSRF controls remain distinct from forwarded-IP/proxy threats; evidence absent. |
| TH-15–TH-16 / SEC-E09, E13, E18 | OPS-001; OPS-003; SEC-002; FIN-002 | Approved criteria now cover post-restore worker pause, integrity/reconciliation gates, authorized resume and backup protection; restore evidence absent. |
| TH-17 / SEC-E07, E11, E14 | OPS-002; DEV-001; SEC-002 | Approved criteria now cover CI/deployment identity, dependency provenance, protected release configuration and audit trail; evidence absent. |
| TH-18 / SEC-E19 | TEN-001; DEV-002; REP-001 | Approved criteria now cover server-derived tenant ownership and dispatch-time revalidation; endpoint policy implementation and evidence remain open. |
| TH-19 / SEC-E20 | REC-001; REC-002; FIN-002; FIN-005; IAM-002; OUT-005 | Approved criteria now cover preservation of conflicting evidence, authorized resolution and linked corrections; policy and evidence remain open. |
| TH-20 / SEC-E21 | EXT-004; SEC-002; DEV-001 | Approved criteria now enumerate prohibited card-data capture surfaces and retain the processor/scope enablement gate; qualified review and evidence remain open. |

**Coverage conclusion:** The previously identified normative wording gaps have been addressed by the approved RCP-01–RCP-08 changes. This does not establish complete design coverage, implementation or control effectiveness. All 20 threats remain Open, all 21 DOC-60 rows remain Draft — evidence absent, and G1/G2 remain open.

## 4D. Product-owner-approved requirements change set (approved 2026-10-23)

The product owner explicitly approved RCP-01 through RCP-08 on 2026-10-23. The corresponding acceptance criteria have now been incorporated into DOC-31 under existing stable requirement IDs; no new IDs were needed. This approval changes normative requirements wording only. It does not verify implementation, close threats, authorize tests, or pass G1/G2.

| Proposal | Suggested destination | Proposed acceptance-criteria wording for review | Why this is proposed | Decision state |
|---|---|---|---|---|
| RCP-01 — Outbound webhook SSRF resistance | DEV-002, or a new stable requirement if the owner determines DEV-002 is too broad | “Before delivering a webhook, the system must validate the destination scheme and resolved network addresses; reject loopback, private, link-local, reserved, metadata-service and other prohibited internal destinations for both IPv4 and IPv6; revalidate every redirect target and resolved address; defend against DNS rebinding and mixed DNS answers; and constrain outbound network access so URL validation cannot be bypassed. Redirect behavior and allowed destination policy must be explicit and auditable.” | TH-07; current webhook-authentication wording does not explicitly cover SSRF-safe DNS, redirects and egress | Approved by product owner 2026-10-23; incorporated into DOC-31; evidence absent |
| RCP-02 — Reconciliation import identity and integrity | REC-001 and/or REC-002, or a new stable requirement if needed | “Every reconciliation import must have a stable, unique source/import identity and an integrity fingerprint tied to its source and content. Reprocessing the same unchanged import must be safely idempotent; reusing an import identity with altered content must be rejected or quarantined for review. Imported records, matching decisions, discrepancies and any correction must retain source provenance and an auditable history; an import must not silently overwrite prior evidence or financial history.” | TH-11; current discrepancy/correction requirements do not explicitly define same-ID altered-content rejection | Approved by product owner 2026-10-23; incorporated into DOC-31; evidence absent |
| RCP-03 — Safe recovery after restore | OPS-001 and OPS-003, with FIN-002 where ledger integrity is relevant | “After restoring a database, backup or queued work state, consequential workers and replay-capable jobs must remain paused until the restored state is validated and required ledger, queue, transaction and provider-reference reconciliation is complete. The recovery procedure must prevent duplicate external side effects, record the reconciliation result and identify the authorized role that may resume processing. Backup access, protection, retention and restore integrity must be documented and reviewed.” | TH-15/TH-16; runbooks and recovery objectives do not by themselves require a post-restore pause and reconciliation gate | Approved by product owner 2026-10-23; incorporated into DOC-31; evidence absent |
| RCP-04 — Dependency, CI and deployment identity security | OPS-002, DEV-001 and SEC-002, or a new stable requirement if current criteria are insufficient | “Build and deployment systems must use least-privilege, attributable identities and scoped credentials; restrict who can change protected release configuration and production deployment permissions; review and record dependency provenance and security findings under an approved policy; protect secrets from logs and build artifacts; and preserve release/change evidence sufficient to identify the source revision, approvals and deployed artifact. Exceptions must have a named approver, rationale and expiry/review date.” | TH-17; broad release gates do not explicitly state dependency provenance, CI permissions, protected changes and deploy identity expectations | Approved by product owner 2026-10-23; incorporated into DOC-31; evidence absent |
| RCP-05 — Sensitive diagnostic and support artefacts | SEC-002 and DEV-003; consider EXT-004 for card-data examples | “Sensitive information controls must apply not only to application logs and API responses but also to traces, analytics, screenshots, test fixtures, offline caches, support exports and diagnostic attachments. These surfaces must minimize collection, redact or exclude secrets, personal data and prohibited cardholder/sensitive-authentication data, restrict access and retention, and be checked before evidence is shared or retained. Private verification evidence must not be placed in offline caches.” | TH-12/TH-20; existing general data-protection criteria do not enumerate all identified artefact surfaces | Approved by product owner 2026-10-23; incorporated into DOC-31; evidence absent |
| RCP-06 — Tenant-bound webhook and report dispatch | TEN-001, DEV-002 and REP-001 | “For every webhook attempt, retry, replay, report and export, the system must derive tenant ownership from trusted server-side records and revalidate that the endpoint, event, job and requested data belong to the same tenant at dispatch/execution time. Client-supplied identifiers must not override ownership. Cross-tenant delivery or disclosure must be denied and auditable; endpoint ownership/change verification must be explicit.” | TH-18; core tenant/webhook/report coverage exists but endpoint ownership and dispatch-time revalidation need an explicit decision | Approved by product owner 2026-10-23; incorporated into DOC-31; evidence absent |
| RCP-07 — Authority for contradictory provider-evidence resolution | REC-001, REC-002, FIN-002, FIN-005, IAM-002 and OUT-005 | “When provider evidence conflicts, arrives late or contradicts an existing state, the discrepancy must remain visible and unresolved until the approved resolution rule and authoritative evidence support a decision. Any consequential manual resolution must record the actor, authority, rationale, source evidence and time; require independent approval where the approved risk policy requires it; preserve the original evidence and prior state; and use a linked correction rather than overwrite financial history.” | TH-19; requirements support discrepancy handling, approvals and audit, but the owner should explicitly confirm manual-resolution authority and preservation rules | Approved by product owner 2026-10-23; incorporated into DOC-31; evidence absent |
| RCP-08 — Card-data boundary and prohibited capture | EXT-004 and SEC-002 | “Before enabling a card-payment path, HARAI must document and obtain qualified review of the processor/acquirer data flow, responsibility boundary and applicable security/compliance scope. The integration must use the approved hosted/tokenized approach where required and must not store or emit raw card data or sensitive authentication data in HARAI-controlled logs, traces, analytics, fixtures, screenshots, support tools, exports or backups unless explicitly permitted by a reviewed and authorized design. The path remains disabled until the required processor permission and scope decisions are approved.” | TH-20; current card boundary exists, but exact prohibited surfaces and enablement gate need confirmation | Approved by product owner 2026-10-23; incorporated into DOC-31; evidence absent |

### Proposed disposition and control rules

1. RCP-01 through RCP-08 are approved requirements changes and are recorded in DOC-31 under DEV-002, REC-001, REC-002, FIN-002, OPS-001, OPS-002, OPS-003, TEN-001, REP-001, IAM-002, FIN-005, DEV-001, SEC-002, DEV-003 and EXT-004. No new stable IDs were needed.
2. Existing requirement IDs were extended after checking the current catalogue; do not create duplicate IDs for the same approved control intent.
3. DOC-00 records the product-owner decision. DOC-34 and DOC-60 must be reconciled to the approved wording; DOC-43 threat status and all evidence statuses remain open until separately reviewed.
4. Any later amendment, deferral or exception requires a recorded decision, rationale, risk owner and compensating control; do not imply the corresponding threat is closed.
5. The approved criteria still require technical/security and relevant finance, operations or privacy review. No evidence collection, tests or implementation is authorized by this requirements approval.


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