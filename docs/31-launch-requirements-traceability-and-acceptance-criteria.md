# DOC-31 — Launch Requirements Traceability and Acceptance Criteria

**Status:** Draft working baseline — product-owner and independent review required  
**Parent documents:** DOC-02 (product scope), DOC-03 (roadmap), DOC-28 (funds-flow comparison), DOC-29 (provider inquiry), DOC-30 (Uganda compliance review)  
**Purpose:** Turn the broad launch ambition into traceable requirements that can later be reviewed, estimated, designed and tested without silently assuming missing provider or legal capabilities.

## 1. Interpretation rules

- This is a requirements and acceptance-criteria draft, not an implementation specification or authorization to build.
- Requirement IDs are stable identifiers for discussion. Priorities and criteria may change only through documented review.
- **Blocked** means the behavior depends on external evidence, legal/compliance review, a product-owner decision or an approved architecture.
- A public provider page is not sufficient evidence of HARAI eligibility or a contractual permission.
- “Successful” must mean a verified outcome according to the approved lifecycle rule, not merely that an API request was accepted.
- Any money-moving behavior must be disabled in production until its provider capability, legal/contractual permission, funding/settlement route, limits, reconciliation and operational controls pass the applicable gates.

## 2. Priority definitions

- **P0 — hard safety gate:** must be designed, reviewed and verified before any live financial operation.
- **P1 — intended core launch capability:** part of the comprehensive launch package, subject to external feasibility and release gates.
- **P2 — conditional extension:** only included after separate approval and verified feasibility.
- **Blocked — external authority/evidence required:** cannot be represented as available until the named dependency is resolved.

## 3. Requirements catalogue

### A. Governance, merchant tenancy and onboarding

| ID | Priority | Requirement | Acceptance criteria | Dependencies / evidence |
|---|---|---|---|---|
| GOV-001 | P0 | Decisions and scope are controlled | Each material product, funds-flow, security and release decision has an ID, owner, status, rationale and linked evidence; proposals are not treated as approvals | DOC-00; named owner and independent reviewer |
| TEN-001 | P0 | All merchant data is tenant-isolated | Automated tests demonstrate that a user/API key for one tenant cannot read, alter, export or trigger operations for another tenant through APIs, background jobs, reports, support tools or callbacks | Approved tenant model; security review |
| MER-001 | P1 | Merchant onboarding has explicit lifecycle states | Registration, submitted, needs-information, under-review, approved, rejected, suspended and closed states (or an approved equivalent) have documented transitions, permissions and audit events | Provider KYC rules; qualified compliance review |
| MER-002 | P0 | Live merchant activation is gated | A merchant cannot use live methods until required verification, provider authorization, contract/configuration, limits and risk controls are recorded as satisfied; rejected or suspended merchants cannot initiate new live operations | Provider and compliance evidence; activation policy |
| IAM-001 | P0 | Users receive least-privilege access | Every protected operation requires an explicit permission; role changes and access revocation take effect according to a documented session/token policy and are auditable | Approved roles and permission matrix |
| IAM-002 | P0 | Sensitive actions have appropriate approval controls | Payouts, beneficiary changes, credential changes, refunds and other designated high-risk actions enforce configured limits and independent approval/step-up controls where policy or law requires; the initiator cannot approve their own action where separation is required | Risk policy; legal/provider conditions |

| MER-003 | P0 | Merchant verification evidence is handled safely | Required evidence, reviewer decision, reason, expiry/reverification and access history are recorded; evidence access is least-privilege; test data contains no real identity documents | Provider requirements; privacy review |
| MER-004 | P1 | Merchant suspension and closure are controlled | Suspension blocks new operations according to approved policy, preserves required records, and provides a documented path for pending transactions, disputes, settlements and closure | Compliance, provider and records-retention rules |

### B. Collections and payer experience

| ID | Priority | Requirement | Acceptance criteria | Dependencies / evidence |
|---|---|---|---|---|
| COL-001 | P1 | A merchant can create a collection request through an authenticated API | The API validates tenant, currency, amount precision, method availability, merchant status, reference and idempotency; invalid requests return documented errors and create no payable transaction | Approved API contract; provider capability matrix |
| COL-002 | P1 | Hosted checkout clearly identifies the payment | Before payer authorization, checkout displays the correct merchant identity, amount, currency, purpose/reference and available provider-supported method; expired, cancelled or unavailable sessions cannot be paid | Approved UX, disclosures and provider flow |
| COL-003 | P0 | Duplicate collection submissions are safe | Repeating a request with the same idempotency key and equivalent payload returns the same logical operation; reuse with a conflicting payload is rejected; concurrency tests show no duplicate logical collection | Transaction and idempotency design |
| COL-004 | P0 | Pending and unknown outcomes are handled safely | A timeout or missing callback never becomes a fabricated failure or success; reconciliation/status recovery can resolve the operation; the system prevents an unsafe duplicate financial submission while outcome is unknown | Provider status APIs and written finality/retry rules |
| COL-005 | P0 | Only verified events can change transaction state | Invalid callback signatures/authentication are rejected; duplicate, replayed, stale and out-of-order events are handled idempotently; raw sensitive payloads are not indiscriminately logged | Provider callback security specification |
| COL-006 | P1 | Payment communications reflect verified state | Receipts and merchant/payer notifications use the authoritative transaction state, are not sent as final success before verification, and record delivery attempts/outcomes | Notification rules; privacy and message templates |

| COL-007 | P1 | Payment links and collection sessions have bounded lifecycles | Expiry, cancellation, revocation and usage limits are enforced server-side; concurrent attempts cannot exceed permitted use; all terminal transitions are audited | Approved checkout/payment-link rules |
| COL-008 | P0 | API abuse is bounded without breaking recovery | Rate limits, payload validation and abuse controls are scoped by credential, tenant and endpoint; recovery/status queries remain available under documented safe limits; limit decisions are observable | Threat model and API policy |

### C. Money-out, refunds and reversals

| ID | Priority | Requirement | Acceptance criteria | Dependencies / evidence |
|---|---|---|---|---|
| OUT-001 | P1 / Blocked | Authorized users can initiate supported payouts | A payout requires an eligible merchant, verified beneficiary, supported method, available approved funding source, limit/fee checks, required approvals and an auditable request; unsupported or unapproved paths are disabled | Written provider permission, legal review, funds-flow and funding-source approval |
| OUT-002 | P0 | Payout uncertainty cannot cause duplicate payment | Timeouts and ambiguous responses enter an explicit unknown/pending state; recovery uses provider status/reference and approved retry rules; blind retries are prohibited; tests cover concurrent requests and late callbacks | Provider idempotency/status/retry documentation |
| OUT-003 | P1 / Blocked | Refunds are linked to original collections | A refund references a verified original transaction, enforces remaining refundable amount and provider-specific eligibility, prevents concurrent over-refunds, and preserves the original history | Written refund support, rules, settlement and accounting review |
| OUT-004 | P1 / Blocked | Reversals and returns are recorded without erasing history | Provider-confirmed reversals/returns create traceable state changes and appropriate compensating financial records; the original transaction and evidence remain visible to authorized reviewers | Provider lifecycle rules and ledger approval |
| OUT-005 | P0 | High-risk financial actions are auditable | Initiator, approver where required, reason, timestamps, amount/currency, destination reference, provider reference, state transitions and resolution evidence are retained under an approved retention policy | Permission model; privacy/retention review |

| OUT-006 | P1 | Beneficiary changes are controlled | Beneficiary identity/destination changes require verification, permission checks, risk-based step-up/approval and audit history; pending payouts cannot silently switch destination | Provider requirements and risk policy |

### D. Ledger, fees and reconciliation

| ID | Priority | Requirement | Acceptance criteria | Dependencies / evidence |
|---|---|---|---|---|
| FIN-001 | P0 | Financial amounts use exact monetary representation | Currency and minor-unit/precision rules are explicit; authoritative calculations do not use binary floating-point; invalid precision and unsupported currencies are rejected | Currency policy and finance review |
| FIN-002 | P0 | Financial history is append-only/auditable | Posted financial records are not silently overwritten or deleted; corrections are represented through linked compensating records with reason and actor; invariant tests cover duplicate and concurrent posting | Approved ledger model and independent finance review |
| FIN-003 | P0 | Fees and net settlement are explainable | Each fee component, payer, calculation basis, rounding rule, version, gross/net amount and applicable tax treatment is traceable to an approved schedule; no invented provider cost or customer price is used | Provider fee schedule; tax/legal and owner approval |
| REC-001 | P0 | Transactions reconcile to provider evidence | Each supported flow can be matched by stable identifiers to provider transaction/status and settlement/payout evidence; unmatched, duplicate and amount-mismatched items enter a queue with owner, age and resolution audit | Provider reports/API access; funds-flow model |
| REC-002 | P0 | Reconciliation discrepancies cannot be hidden | The system records detection time, category, financial impact where known, accountable owner, investigation notes and resolution evidence; unresolved material breaks block affected release/settlement decisions under approved policy | Operations policy; finance review |
| REP-001 | P1 | Reports and exports are permissioned and reproducible | Reports respect tenant/role access, define date range/time zone and status inclusion, expose relevant totals and filters, and produce consistent results for the same source data and parameters | Reporting definitions and privacy review |

| FIN-004 | P1 | Disputes and transaction complaints are trackable | A case links to the original transaction and evidence, records owner, deadlines, status, communications and outcome, and cannot silently modify financial records | Provider dispute/complaint obligations; support policy |
| FIN-005 | P0 | Financial state transitions are formally specified | A reviewed state machine defines permitted transitions, terminality, provider-specific exceptions and reconciliation overrides; invalid transitions are rejected and audited | DOC-10 state-machine design; provider lifecycle evidence |

### E. Developer platform, security and operations

| ID | Priority | Requirement | Acceptance criteria | Dependencies / evidence |
|---|---|---|---|---|
| DEV-001 | P1 | API credentials have a secure lifecycle | Secrets are shown only at creation where appropriate, stored as non-recoverable hashes or protected according to approved design, scoped, revocable, rotated and audited; secrets never appear in logs or source control | Security architecture and threat model |
| DEV-002 | P1 | Webhooks are authenticated and recoverable | Events have stable IDs and documented schemas; signatures and timestamps are verifiable; retries are bounded and observable; receivers can deduplicate; manual replay is permissioned and audited | Event contract and signing design |
| SEC-001 | P0 | Authentication and sessions are protected | MFA/step-up decisions, session expiry/revocation, account recovery, brute-force protection and sensitive-account notifications are documented and tested against the threat model | Security design; identity provider choice |
| SEC-002 | P0 | Sensitive information is minimized and protected | Verification evidence, personal data, API secrets and provider payloads have defined access, encryption, logging redaction, retention and deletion/hold rules; support access is restricted and auditable | Data inventory, privacy review and provider terms |
| OPS-001 | P0 | Failures and incidents have owned runbooks | Runbooks cover provider outage, delayed callbacks, unknown financial outcome, duplicate event, payout return, reconciliation break, credential compromise, data incident, restore and escalation; each has an accountable role | Named operational owners; provider SLA/support |
| OPS-002 | P0 | Production release requires evidence-based gates | Release checklist includes security, finance/reconciliation, provider authorization, legal/compliance, monitoring, backup/restore, incident response, support, rollback/disablement and independent sign-off; failed hard gates block launch | DOC-03 G5–G8; independent reviewers |

| DEV-003 | P1 | API behavior is versioned and diagnosable | Public contracts define versioning/deprecation, authentication, pagination, stable error codes, correlation IDs and safe request/response logging; compatibility tests cover supported versions | Approved API contract |
| SEC-003 | P0 | Network and browser access controls are safe | CORS, CSRF protections where applicable, trusted-proxy handling, IP allowlist validation, lockout-safe recovery and administrative access are defined and tested | Threat model and deployment topology |
| OPS-003 | P0 | Reliability targets and recovery are measurable | Availability/latency objectives, maximum acceptable pending age, alert thresholds, backup/restore objectives and recovery ownership are approved and tested; no targets are invented without owner review | Owner decisions; provider SLAs; operational capacity |
| OPS-004 | P1 | Accessibility and responsive operation are verified | Critical dashboard and checkout journeys are keyboard operable, labeled for assistive technology, responsive at supported widths, and have understandable loading/empty/error/pending states | UI acceptance plan and accessibility review |

### F. Card payments and conditional extensions

Card payments are part of the intended multi-provider payment-method package, alongside both MTN MoMo and Airtel Money. Card processing remains **P1 / Blocked** until an eligible processor/acquirer, supported schemes, commercial permission, technical access, security responsibilities, settlement/funding model, fees, dispute handling and compliance requirements are evidenced. Other methods beyond the named core package remain conditional.

| ID | Priority | Requirement | Acceptance criteria | Dependencies / evidence |
|---|---|---|---|---|
| EXT-004 | P1 / Blocked | Card payments are available through an eligible processor/acquirer | Checkout presents card payment only when the merchant, transaction, currency and environment are eligible; authorization/capture or equivalent lifecycle, 3-D Secure/step-up where required, tokenization and card-data handling boundaries, declines, timeouts, refunds, disputes/chargebacks, fees, settlement, reconciliation and webhooks are documented and tested for the chosen processor. HARAI must not store sensitive authentication data or raw card data unless a reviewed, explicitly authorized architecture and applicable requirements permit it. | Processor/acquirer selection and written permission; PCI/security scope review; fees and settlement terms; legal/compliance review; official API and sandbox evidence |
| EXT-001 | P2 / Blocked | Additional payment methods and currencies beyond the planned MTN MoMo, Airtel Money and card package | Each additional method/currency has separate capability, eligibility, fee, lifecycle, settlement, compliance, reconciliation and acceptance evidence before activation | Provider/processor contracts and legal review |
| EXT-002 | P2 / Blocked | Scheduling, split settlement and marketplace payouts | Each feature has explicit timing, authorization, failure, cancellation, ledger and reconciliation behavior and is enabled only where providers and applicable rules permit | Separate product decision and feasibility review |
| EXT-003 | P2 / Blocked | Regional expansion | Each new market has its own regulatory, provider, currency, data, support, tax and operating review before any market-specific launch claim | Country-specific professional review |

## 4. Cross-cutting acceptance rules

All applicable requirements must satisfy these rules before acceptance:
1. **Traceability:** linked to a product decision, design artefact, test case and evidence record.
2. **Authorization:** actors and permissions are explicit; no implicit trust in client-supplied tenant or role identifiers.
3. **Idempotency and concurrency:** duplicate, replayed, concurrent and reordered requests/events are tested.
4. **Unknown outcomes:** uncertain external outcomes remain explicit until resolved by approved evidence; no invented finality.
5. **Auditability:** privileged decisions and financial state changes are attributable and reviewable.
6. **Tenant isolation:** cross-tenant negative tests cover API, jobs, webhooks, reports, exports and support tools.
7. **Observability:** failures, backlog, pending-age anomalies and reconciliation breaks have measurable alerts and owners.
8. **Privacy:** logs and test fixtures do not expose real secrets or unnecessary personal data.
9. **Failure recovery:** tests include provider outage, late response, partial failure, retry exhaustion, return/reversal and restore.
10. **Independent review:** financial-integrity and security-critical requirements receive review by someone other than the author/implementer.

## 5. Evidence status and traceability workflow

For each requirement, maintain a tracker with these fields:
- Requirement ID and current text.
- Priority and status: Draft, In review, Approved, Blocked, Implemented, Verified, Deferred, or Rejected.
- Accountable product owner and technical owner.
- Related decision ID and source document.
- External dependency/evidence reference.
- Design/API/data artefact.
- Test case IDs and latest result.
- Reviewer and review date.
- Known limitations, residual risk and release gate.

No requirement should be marked **Approved** merely because it is written. No requirement should be marked **Verified** without linked evidence. Do not store provider secrets, confidential agreements or sensitive onboarding records in this public repository.

## 6. Immediate review actions

1. Product owner has approved the DOC-36 product-direction defaults; review the detailed actors, requirement wording, and P0/P1/P2 classifications with accountable reviewers. Approval of defaults does not mark individual requirements verified.
2. Provider inquiry responses populate the blocked dependencies in DOC-29 and DOC-28.
3. Qualified Ugandan counsel/compliance adviser reviews DOC-30 against the actual proposed funds flows.
4. Technical architecture maps accepted requirements to API contracts, state machines, tenant boundaries, ledger and reconciliation designs.
5. Independent security and finance reviewers challenge the acceptance criteria before implementation approval.

## 7. Exit criteria for the requirements phase

The requirements phase is not complete until:
- Product owner approval of the DOC-36 defaults is recorded; detailed requirement acceptance, exclusions and capability-by-capability evidence still require accountable review. Intended methods include both MTN MoMo and Airtel Money, cards through an eligible processor/acquirer, and additional suitable methods.
- Provider-dependent requirements are either supported by current written evidence or remain explicitly blocked.
- Funds flow, settlement, money-out funding, refund/reversal behavior and reconciliation are reviewed.
- Security, privacy, tenant isolation, audit, failure recovery and operational requirements have named owners.
- Each approved requirement has testable acceptance criteria and traceability to planned evidence.
- Unresolved decisions and release gates are accurately reflected in DOC-00 and DOC-03.

**Current disposition:** Product-owner defaults POD-01–POD-12 are approved, but this remains a draft requirements baseline and G1 has not passed. Provider onboarding/contact, production implementation, live credentials, real transactions, pull-request merge and deployment remain behind their separate evidence and authorization gates.


## 8. Supplemental requirements identified by DOC-32

The following ten stable supplemental requirement IDs convert the high-priority gaps in DOC-32 into reviewable acceptance criteria. They expand the working catalogue from 43 to **53 unique requirement IDs**. They remain **Draft / In review** until the product owner and accountable reviewers accept, amend, defer or reject them. Test identifiers below are proposed evidence references, not existing tests or claimed results.

| ID | Priority | Requirement | Acceptance criteria | Proposed verification evidence / dependencies |
|---|---|---|---|---|
| SUP-001 | P0 | API abuse and recovery-request rate limits are explicit | Authentication, collection creation, status polling, webhook-management and recovery endpoints have documented per-tenant and per-credential limits; limits avoid unbounded provider polling; throttled responses are consistent and observable; legitimate retry behavior is documented; tests cover threshold, reset, concurrent requests and isolation between tenants | Proposed test: API-RATE-001; threat model; provider rate limits; numeric thresholds require owner/technical review |
| SUP-002 | P0 | Payment links and checkout sessions have controlled lifecycle and usage | Expiry, cancellation, revocation, allowed payment methods, amount/currency binding and any usage limit are explicit; expired/revoked links cannot start a new payable operation; concurrent requests cannot exceed a configured use limit; already-started transactions follow a documented non-destructive policy | Proposed test: LINK-LIFE-001; COL-001/COL-002; state-machine design |
| SUP-003 | P0 | Beneficiary and payout-destination changes are protected | Destination changes require re-authentication or step-up controls appropriate to risk, an auditable change record, configured cooling-off/approval rules where required, and safe handling of pending payouts; initiator/approver separation applies where policy requires it; tests prove unauthorized and stale changes cannot redirect money-out | Proposed test: PAYEE-CHANGE-001; IAM-002; qualified risk/compliance review |
| SUP-004 | P0 | Transaction state transitions and finality are canonical | A documented state machine lists allowed transitions, terminal states, duplicate/out-of-order event behavior, timeout/unknown outcomes and recovery actions for each supported provider method; no callback or client redirect alone can incorrectly mark a transaction paid; tests cover every allowed transition and representative forbidden transitions | Proposed test: STATE-MATRIX-001; provider-specific lifecycle evidence; ledger/reconciliation design |
| SUP-005 | P1 | Disputes and complaints have owned, traceable case handling | Cases record source, transaction reference, merchant/customer context subject to minimization, owner, deadlines, evidence references, status, communication history and outcome; escalation and retention rules are configurable; tests cover reassignment, overdue cases, access restrictions and closure | Proposed test: CASE-DISPUTE-001; support policy; provider and legal requirements |
| SUP-006 | P0 | Public API contracts remain versioned and predictable | Supported versions and deprecation policy are documented; errors use stable machine-readable codes and correlation IDs; pagination is bounded and deterministic; unknown fields/versions and malformed requests fail safely; compatibility tests cover supported versions and error schemas | Proposed test: API-CONTRACT-001; API authentication and observability design |
| SUP-007 | P0 | Browser, proxy and IP-allowlist trust boundaries are defined | CORS origins are explicit; CSRF protections are applied where cookie-authenticated browser flows require them; forwarded headers are trusted only from configured proxies; client IP is not accepted from arbitrary headers; allowlist changes and lockout recovery are authenticated, audited and tested | Proposed test: EDGE-TRUST-001; deployment topology; credential/IP policy; security review |
| SUP-008 | P0 | Reliability and recovery objectives are measurable | Availability and latency indicators, alert thresholds, maximum pending age, recovery-time objective (RTO), recovery-point objective (RPO), backup frequency, restore verification and incident escalation are documented and approved before production; a backup is not considered valid until restore evidence exists | Proposed test/evidence: OPS-RECOVERY-001; service capacity and operational ownership; numeric targets remain open |
| SUP-009 | P1 | Critical journeys meet accessibility and responsive-use criteria | Merchant and payer journeys identify keyboard access, visible focus, labels/errors, contrast, zoom/reflow and narrow-screen acceptance checks; automated checks are supplemented by manual keyboard and screen-reader review for critical journeys; defects are tracked to disposition | Proposed test: UX-A11Y-001; design system and supported-browser policy |
| SUP-010 | P0 | Security and financial events are correlated end-to-end | API request, provider request, callback, ledger posting, reconciliation item and support case can be linked using non-secret correlation identifiers; logs exclude credentials, tokens, full payment secrets and unnecessary personal data; access to audit trails is restricted and tamper-resistance/retention requirements are defined | Proposed test: TRACE-AUDIT-001; DOC-11/DOC-14/DOC-18; privacy and retention review |

### 8.1 Supplemental review rules

1. These ten requirements are additions to the 43 previously catalogued requirements, not evidence that the gaps are implemented or verified.
2. The requirement-level matrix in DOC-34 must be extended to all 53 IDs with role-based owner proposals, dependency links, proposed test/evidence references and gate disposition. Do not invent named people or claim tests exist.
3. DOC-03 must map these requirements to design, implementation, verification and launch-readiness phases after the relevant gates are authorized.
4. Numeric rate limits, expiry defaults, RTO/RPO, availability/latency targets, retention periods and accessibility support policy remain open decisions until reviewed and approved.
5. G1 remains open until the product owner and accountable reviewers explicitly accept, amend, defer or reject each supplemental requirement and the traceability package is consistent.
