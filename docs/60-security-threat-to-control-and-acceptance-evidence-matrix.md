# DOC-60 — Security Threat-to-Control and Acceptance Evidence Matrix

**Status:** Draft security assurance planning artifact — not a security approval or verification record
**Purpose:** Connect documented threats and security policies to control objectives, accountable review, requirement traceability, required evidence and explicit closure criteria.
**Gate status:** G1 NOT PASSED. G2 NOT APPROVED.

> This matrix is a planning and review aid. Control presence, evidence readiness, and actual effectiveness are separate questions. No row is marked implemented or verified without concrete evidence and independent accountable review. No tests are authorized or claimed as run. This document authorizes no implementation, provider contact, production credentials, real transactions, merge or deployment.

## 1. How to use this matrix

- **Threat references:** DOC-43 remains the threat-model source; use its exact threat IDs and update this matrix when that model changes.
- **Requirement source:** DOC-31 owns requirement wording and acceptance criteria; DOC-34 tracks requirement-level evidence. The references below identify relevant existing requirements where known; they do not create new requirement IDs.
- **Control proposal:** a desired safeguard, not proof it exists.
- **Evidence required:** the concrete artifact needed to support a review, with secrets and personal data redacted.
- **Closure rule:** the stated control and its negative/failure behavior are demonstrated in an authorized environment, evidence is recorded under DOC-45, and the assigned accountable reviewers accept it.
- **Statuses:** all entries begin `Draft — evidence absent`. Update to `Blocked`, `Implemented — not verified`, or `Verified` only when the defined evidence exists. Never infer verification from a planned test ID, code review alone, or an unexecuted test.
- **Owners:** role names are proposals, not assigned people. The product owner must name accountable individuals and an independent security reviewer before any formal closure.

## 2. Threat-to-control matrix

| ID | Threat / failure scenario | Source and requirement linkage | Proposed preventive/detective control | Required acceptance evidence | Proposed accountable reviewer(s) | Status / closure condition |
|---|---|---|---|---|---|---|
| SEC-E01 | Stolen or leaked merchant API key is used by an unauthorized party | DOC-43 threat model; DOC-31 security/API requirements; DOC-59 §§3, 8, 11 | High-entropy server-issued key design; narrow scopes; secure verifier storage; rate limits; immediate revocation; anomaly audit | Approved key design; redacted storage/configuration review; negative authorization evidence; revocation propagation result; incident exercise record | Security lead + API technical lead + merchant product owner | Draft — evidence absent; close only after key misuse and revocation behavior are demonstrated and independently reviewed |
| SEC-E02 | A key with excessive permissions performs an operation beyond the merchant's entitlement | DOC-31 role/access and financial-control requirements; DOC-34; DOC-59 §§3.2, 4 | Deny by default; per-operation scopes; server-side entitlement and environment checks; additional approvals for high-impact actions | Approved scope-to-operation matrix; denied-operation evidence; privileged-operation approval trail; requirement mapping | Security reviewer + product owner + finance/control reviewer for financial operations | Draft — evidence absent; every protected operation must have an explicit authorization decision |
| SEC-E03 | Dashboard session theft, account takeover or unsafe recovery grants privileged access | DOC-31 access/security requirements; DOC-51; DOC-59 §4 | Individual identities; approved MFA/step-up policy; session controls and revocation; secure recovery; audited privileged actions | Approved identity/session policy; session invalidation and recovery evidence; role/action audit samples; independent review | Identity/security lead + dashboard technical lead + operations owner | Draft — evidence absent; close after normal, privileged and recovery paths meet approved policy |
| SEC-E04 | One merchant reads or modifies another merchant's data through an ID, export, cache, job or support tool | DOC-43 tenant/access threats; DOC-31 tenancy/security requirements; DOC-34; DOC-56; DOC-57; DOC-59 §5 | Server-derived tenant context; ownership checks at every boundary; scoped caches/jobs/storage/exports; controlled support access | Data-flow and access-boundary review; cross-tenant negative evidence for API, dashboard, workers, exports and support paths; reviewer sign-off | Security reviewer + backend/data lead + privacy reviewer | Draft — evidence absent; any cross-tenant access is a blocker, not a tolerated exception |
| SEC-E05 | Forged forwarded-IP headers or untrusted proxy configuration bypasses an IP allowlist | DOC-31 SUP-007; DOC-43 TH-06/TH-14 as referenced by DOC-34; DOC-59 §6 | Explicit trusted-proxy list; derive client address only from trusted hops; reject malformed policy input; apply allowlist server-side | Approved network/proxy diagram; source-IP derivation specification; spoofing and malformed-input evidence; reviewed deployment configuration | Security reviewer + platform/network owner | Draft — evidence absent; close only when untrusted headers cannot alter the effective source address |
| SEC-E06 | An allowlist blocks a legitimate merchant or is treated as sufficient authentication | DOC-31 SUP-007; DOC-59 §6 | Configurable allowlist policy; clear source-IP guidance; auditable edits and recovery; always require authentication, scope and tenant checks | Approved default/risk-tier decision; UX and operations guidance; positive/negative allowlist evidence; proof other auth controls remain enforced | Product owner + security reviewer + merchant support/operations owner | Draft — evidence absent; default and any mandatory risk-tier rules require explicit decision |
| SEC-E07 | Provider credentials are exposed to merchants, browsers, logs, source control or unrelated workloads | DOC-43 threat model; DOC-31 provider/security requirements; DOC-48; DOC-59 §7 | Server-side adapter boundary; approved secret manager/KMS; least-privilege runtime access; environment separation; redaction; controlled rotation | Secret-flow diagram; secret-store access review; repository/log/bundle scans with results; rotation/revocation evidence; provider permission evidence where relevant | Security reviewer + platform owner + provider integration owner | Draft — evidence absent; no provider secret may be exposed to a merchant or public/client-side surface |
| SEC-E08 | Replayed or forged webhook/provider callback triggers an invalid state change | DOC-14; DOC-31 webhook/transaction requirements; DOC-41; DOC-43; DOC-59 §9 | Protocol-appropriate signature/verification; replay and deduplication handling; expected account/environment/transaction correlation; valid state-transition guards | Approved provider protocol evidence; signature/replay negative cases; duplicate-delivery evidence; transaction-state transition evidence | Integration lead + security reviewer + payments-domain reviewer | Draft — evidence absent; message authentication must not bypass authoritative state validation |
| SEC-E09 | Duplicate requests, retry storms or ambiguous timeouts cause duplicate financial effects | DOC-31 idempotency/rate-limit requirements; DOC-41; DOC-42; DOC-58; DOC-59 §8 | Scoped idempotency; request fingerprint/conflict rules; bounded retries; unknown-outcome handling; reconciliation before unsafe repeat actions | Approved idempotency contract; duplicate/concurrency and timeout scenario evidence; reconciliation evidence; finance review | Payments technical lead + finance/reconciliation owner + security reviewer | Draft — evidence absent; no unresolved ambiguous outcome may be silently treated as success or failure |
| SEC-E10 | Brute force, scraping or API abuse exhausts resources or hides malicious activity in log flooding | DOC-31 SUP-001; DOC-43 TH-13; DOC-59 §8 and §10 | Per-credential/merchant/operation controls; authentication throttling; abuse alerts; safe lockout/recovery; bounded audit ingestion | Approved thresholds and rationale; load/abuse scenario evidence when authorized; alert/response exercise; evidence that controls do not bypass authorization | Security reviewer + platform/operations lead + product owner | Draft — evidence absent; numeric limits and emergency response must be approved before acceptance |
| SEC-E11 | Privileged employee or compromised internal workload misuses access | DOC-43; DOC-31 governance/audit requirements; DOC-59 §§4, 7, 10 | Separate internal roles; least privilege; time-bounded access where feasible; audited secret/data access; independent approval for sensitive changes | Role matrix; access grant/revocation records; sample privileged-action logs; break-glass procedure and review | Security lead + operations owner + independent governance reviewer | Draft — evidence absent; role owners and escalation authority must be named |
| SEC-E12 | Audit logs omit context, can be altered without detection, or contain secrets/personal data | DOC-31 SUP-010; DOC-42; DOC-43; DOC-45; DOC-59 §10 | Correlation IDs; policy/action/outcome metadata; restricted log access; integrity and retention controls; redaction/data minimization | Event schema; redacted end-to-end correlation sample; access/retention policy; integrity controls review; sensitive-data exposure checks | Security reviewer + finance/reconciliation owner + privacy reviewer | Draft — evidence absent; evidence must support investigation without exposing credentials or unnecessary data |
| SEC-E13 | Compromise response is delayed, unauthorized or loses evidence of uncertain payment outcomes | DOC-31 incident/recovery requirements; DOC-41; DOC-42; DOC-45; DOC-59 §11 | Named incident owner; revocation and containment runbook; evidence preservation; provider enquiry/reconciliation; approved notification process | Approved runbook; named contacts and severity matrix; tabletop record; revocation exercise; ambiguous-payment investigation walkthrough | Operations/incident owner + security reviewer + finance/reconciliation owner | Draft — evidence absent; responsibilities and notification rules must be approved and operationally actionable |
| SEC-E14 | Test/sandbox credentials or data are accepted in production, or production credentials enter test systems | DOC-31 environment/provider requirements; DOC-40; DOC-48; DOC-59 §§3, 5, 7 | Environment-bound credentials and configuration; explicit provider account/environment binding; separate access policies and data stores as approved | Environment boundary diagram; wrong-environment rejection evidence; configuration and secret-access review | Platform owner + integration lead + security reviewer | Draft — evidence absent; cross-environment credential acceptance is a release blocker |
| SEC-E15 | Sensitive configuration changes (keys, roles, payout destination, webhook target, allowlist) occur without appropriate authority or traceability | DOC-31 SUP-003/SUP-010 and relevant access requirements; DOC-51; DOC-59 §§3–4, 6, 10 | Server-side authorization; step-up/dual control where approved; change confirmation; audit record and alerts | Approved action-to-role/approval matrix; denied/approved change evidence; audit trail; recovery/notification review | Product owner + security reviewer + finance/control owner for money-out settings | Draft — evidence absent; high-impact change policy and named approvers required |
| SEC-E16 | Security error responses disclose secrets, stack traces, tenant existence or sensitive internal details | DOC-31 API/security requirements; DOC-58; DOC-59 §§8, 10 | Stable sanitized error contract; server-side detailed logs with redaction; consistent authorization behavior where appropriate | Approved error taxonomy; negative-case response samples; logs reviewed for secret/PII leakage | API technical lead + security reviewer + privacy reviewer | Draft — evidence absent; external responses must be useful without disclosing protected internals |

## 3. Requirement traceability rules

1. DOC-31 is authoritative for requirement IDs, wording, priorities and acceptance criteria. This matrix does not replace or silently rewrite it.
2. DOC-34 must contain the current control/evidence relationship for every requirement claimed as satisfied. Where an exact requirement ID is not yet mapped, record the mapping as an open task; do not invent a new ID.
3. DOC-43 is authoritative for threat identifiers and threat-model rationale. Confirm each exact ID against the current source before using it in a formal review; the references above are navigational pointers, not a claim that the source model is closed.
4. DOC-45 defines evidence quality and test-execution rules. Store only redacted evidence references in this repository; secrets, provider contracts and sensitive records belong in appropriately controlled systems.
5. DOC-47 tracks architecture findings AR-001–AR-020. Security findings must be linked to those findings when applicable, but this matrix does not close them automatically.
6. DOC-59 describes proposed security policies. A policy statement does not prove that a control is implemented or effective.
7. Preserve explicit status distinctions: planned control, approved design, implemented control, executed evidence, independently reviewed result and accepted residual risk are different states.

## 4. Minimum evidence packet for each closure

Each closure packet should include:

- Stable DOC-31 requirement ID(s), DOC-43 threat ID(s) and any DOC-47 finding ID(s).
- Control owner and named independent reviewer; role labels alone are not enough for final approval.
- Approved control/design reference and the version reviewed.
- Evidence type and location, environment, date, scope and relevant configuration version.
- Procedure or inspection method, expected result, actual result and pass/fail/blocked disposition.
- Negative/failure-path evidence, not only a successful happy path.
- Redaction/privacy review and confirmation that no credentials or unnecessary personal data are exposed.
- Residual risk, compensating controls, risk owner and expiry/review date for any explicitly accepted exception.
- Reviewer decision, rationale, unresolved conditions and a link to the decision register where the decision changes scope or policy.

Evidence must be authentic, reproducible to the extent appropriate, relevant to the current version and collected in an authorized environment. A screenshot alone is insufficient when configuration, raw results or repeatable evidence are needed to establish a security property.

## 5. Proposed closure workflow

1. **Scope:** confirm threat, requirement, data/process boundary and affected environments.
2. **Assign:** appoint a named control owner, independent security reviewer and any domain reviewer (finance, privacy, operations or provider integration).
3. **Decide:** approve control behavior, thresholds, exceptions and recovery paths; record decisions in DOC-00.
4. **Map:** reconcile control and evidence IDs with DOC-31, DOC-34, DOC-43, DOC-45 and DOC-47.
5. **Prepare:** write an evidence procedure with expected outcomes, including negative and failure cases.
6. **Authorize:** obtain explicit permission for the relevant implementation and test activity after the required gates are met.
7. **Execute:** collect evidence in the approved environment; record exact version and results. This step has not been authorized by DOC-60.
8. **Review:** have the independent reviewer assess evidence, limitations and residual risks.
9. **Close or block:** close only when criteria are met; otherwise retain the blocker, owner and next required evidence.
10. **Maintain:** reopen the assessment when a material change affects code, credentials, proxy topology, providers, permissions, data flows or threat assumptions.

## 6. Status vocabulary

| Status | Meaning | Permitted basis |
|---|---|---|
| Draft — evidence absent | Control and evidence plan are proposals | Default for all rows in this document |
| Blocked | A required decision, authority, owner, environment or evidence is missing | Record the exact missing prerequisite and responsible role/person when assigned |
| Design approved — not implemented | Named accountable reviewers approved the design, but no implementation evidence exists | Recorded approval and versioned design reference |
| Implemented — not verified | Implementation is identified, but required verification evidence is absent or incomplete | Traceable implementation reference; never infer security effectiveness |
| Verified — reviewer accepted | Defined acceptance evidence exists and independent review accepted it | Complete evidence packet and recorded reviewer decision |
| Exception accepted — time-bounded | A named risk owner accepted a specific residual risk under approved authority | Documented rationale, compensating controls, expiry/review date and approving authority |

No status in this table means the entire security program, G1 or G2 is approved.

## 7. Priority blockers before security acceptance

- Name an accountable security lead and independent reviewer; assign owners for identity, platform/secrets, payments/finance, privacy and incident response.
- Reconcile the matrix against the exact current DOC-43 threat IDs and all relevant DOC-31 requirements, especially SUP-001, SUP-003, SUP-007 and SUP-010.
- Resolve the trusted-proxy/IP-allowlist policy, API key design, dashboard identity/session policy, provider-secret architecture, rate-limit strategy and webhook verification design.
- Establish evidence retention/access rules and the controlled location for confidential supporting artifacts.
- Link each security control to approved requirements and architecture decisions; preserve unknowns and conflicting assumptions explicitly.
- Obtain independent architecture/security review and record open findings in DOC-47 rather than claiming closure based on document completeness.

## 8. Related documents

- DOC-00 — Master index and decision register.
- DOC-12–DOC-14 — API contracts, API-key/IP controls and webhooks.
- DOC-31 — Launch requirements and acceptance criteria.
- DOC-34 — Requirement-level traceability matrix.
- DOC-40 — G2 architecture and financial-safety baseline.
- DOC-41 — Transaction state machine and provider mapping.
- DOC-42 — Ledger and reconciliation proposal.
- DOC-43 — Threat model and security controls.
- DOC-45 — Evidence register and acceptance-test planning standard.
- DOC-47 — Architecture review and contradiction register.
- DOC-48–DOC-49 — Provider adapter and merchant API specifications.
- DOC-51 — Dashboard roles and permissions matrix.
- DOC-56–DOC-59 — Backend data ownership, service/data flow, API contract and security-policy proposals.

## 9. Gate and authorization statement

DOC-60 is a draft assurance matrix, not evidence of implementation, a completed penetration test, a completed independent review or an approved security posture.

**G1 remains NOT PASSED. G2 remains NOT APPROVED.** No implementation, test execution, provider contact, production credential generation, real financial activity, merge or deployment is authorized by this document. These actions require the relevant gates and separate explicit authorization.