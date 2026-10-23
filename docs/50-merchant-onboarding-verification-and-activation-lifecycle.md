# DOC-50 — Merchant Onboarding, Verification and Activation Lifecycle

**Status:** Draft operating/product proposal — policy, legal/compliance review and G1/G2 approval remain open  
**Date:** 2026-10-23  
**Related documents:** DOC-00, DOC-03, DOC-30, DOC-31, DOC-34, DOC-43, DOC-45, DOC-46, DOC-48, DOC-49  
**Purpose:** Define a controlled lifecycle for a business applying to use HARAI, from initial registration through verification, sandbox use, production activation, review, suspension and closure.

> **Authority boundary:** This is a proposed lifecycle, not a legal/KYC determination or approved operating procedure. It does not establish that HARAI may aggregate payments for other businesses, handle funds, or activate any provider capability. Provider contracts, qualified Uganda legal/compliance advice, privacy review, named accountable operators and explicit product-owner approval are still required. No provider contact, applications, live credentials, implementation, merge, deployment or real financial activity is authorized by this document.

## 1. Objectives and non-negotiable principles

1. Make merchant identity, organization, ownership/control, eligibility and approval status explicit and auditable.
2. Separate account registration, identity verification, risk review, sandbox access, provider capability approval and production activation.
3. Default-deny production access and every money-moving capability until all applicable evidence and approvals are recorded.
4. Enforce tenant isolation across merchant users, credentials, API resources, webhook destinations, support cases, reports and audit trails.
5. Use risk-based, proportionate verification and data minimization; collect only what is necessary for a documented purpose and lawful basis.
6. Keep the person submitting an application from silently self-approving a consequential decision where separation of duties is required.
7. Distinguish HARAI internal approval from provider onboarding/permission, processor approval, legal compliance, and operational readiness.
8. Support suspension, restriction, remediation and closure without erasing financial/audit history or accidentally triggering duplicate financial actions.
9. Keep all statuses and decision records evidence-backed. A checklist or proposed test ID is not evidence that a check was completed.

## 2. Scope and non-goals

### In scope
- Business workspace and initial application.
- Proposed verification evidence, review queues, risk assessment and decision recording.
- Merchant users, roles, credential issuance and tenant boundaries.
- Sandbox enablement and integration readiness.
- Capability-by-capability production activation.
- Periodic review, material-change review, suspension, reinstatement and closure.
- Appeals/escalation, record retention, privacy controls and audit evidence.

### Not determined here
- Definitive KYC/KYB document list, statutory thresholds, retention periods or legal basis.
- Final prohibited/restricted merchant-category list or risk scoring model.
- Whether HARAI may legally act as payment aggregator, intermediary or funds handler.
- Provider-specific onboarding rules, contracts, fees, reserves, settlement or production approval.
- Final account authentication, identity provider, credential model or numeric risk/transaction limits.
- Automatic approval rules or any production permission.

These require decisions and qualified review. Do not turn examples in this document into legal requirements without evidence.

## 3. Lifecycle model

The following states are proposed. Final names, transitions and permissions require product, compliance, security and operations review.

| State | Meaning | Default access |
|---|---|---|
| `DRAFT` | Application started but not submitted | Applicant can edit draft only |
| `SUBMITTED` | Applicant submitted required initial fields | No production access |
| `NEEDS_INFORMATION` | Reviewer requested specific additional information | Applicant can provide requested data only |
| `UNDER_REVIEW` | Evidence and eligibility checks are in progress | No production access |
| `REVIEW_ESCALATED` | Specialist, compliance or authority decision is required | No production access; escalation queue only |
| `VERIFIED_PENDING_APPROVAL` | Verification checks have evidence, but final internal approval is outstanding | No production access |
| `APPROVED_SANDBOX` | Approved for isolated sandbox access only | Sandbox resources and credentials only |
| `PRODUCTION_PENDING` | Internal review is complete; external/provider/legal/operational gates remain | No production credential or live transaction access |
| `ACTIVE_RESTRICTED` | Production account active with a restricted, explicitly approved capability set | Only listed enabled capabilities |
| `ACTIVE` | Production merchant approved for its explicit capability set | Approved capabilities only |
| `SUSPENDED` | Temporarily blocked pending investigation, risk review, evidence or incident response | No new consequential operations; safe read-only access may be separately allowed |
| `REMEDIATION_REQUIRED` | Merchant must resolve a specified issue before reinstatement or expansion | Remediation and permitted read-only access only |
| `CLOSURE_PENDING` | Closure requested or initiated; obligations and pending operations are being reviewed | No new operations; existing cases remain visible to authorized roles |
| `CLOSED` | Access terminated and closure controls completed | No normal API access; controlled records retained as required |
| `REJECTED` | Application declined with a recorded reason category | No access beyond appeal/support channels |

**Important:** `ACTIVE` is not a universal provider capability. Activation must include an explicit capability matrix, e.g. collection methods/currencies, refunds, money-out, webhooks and production API access. A merchant approved for collections is not automatically approved for payouts, refunds or card processing.

## 4. Application and organization profile

### 4.1 Proposed initial application fields
- Legal/registered business name where applicable and trading name.
- Business type/legal form and country of registration/operation.
- Business address and operating contact details.
- Authorized representative and a secure method to verify authority to act for the business.
- Business website/app/domain and intended integration type.
- Plain-language description of goods/services and intended payment use cases.
- Requested methods and capabilities: collections, refunds, money-out, cards or other methods, each separately requested.
- Expected transaction pattern and relevant business context where justified by a documented risk purpose.
- Beneficial ownership/control information only to the extent required by approved legal/provider policy.
- Acceptance of applicable terms, privacy notice and data-use explanation, with version and timestamp.
- Applicant attestation that submitted information is accurate and that they are authorized to submit it.

This list is a design starting point, not a final legal document checklist. Do not collect national ID numbers, bank account details, beneficial-owner personal data or other sensitive evidence until purpose, lawful basis, access controls, secure storage, retention and deletion rules are approved.

### 4.2 Input validation
- Normalize and validate formats without silently altering legal names or identifiers.
- Detect duplicate applications using approved privacy-conscious matching rules; a match triggers review, not automatic rejection.
- Do not expose whether another account exists or disclose another merchant's details.
- Keep applicant-provided fields clearly distinguished from reviewer-verified values.
- Record source, timestamp and reviewer for every verification claim.
- Protect uploads from malware and unsafe content; limit file size/type, scan in quarantine and prevent public access.
- Never place uploaded verification documents in public GitHub repositories, application logs, analytics, or unsecured support attachments.

## 5. Verification and due diligence workflow

A proposed workflow should separate checks into categories and record each as `NOT_STARTED`, `IN_PROGRESS`, `PASS`, `FAIL`, `NEEDS_REVIEW`, `NOT_APPLICABLE` or `EXPIRED`, with a reason, evidence reference, timestamp and reviewer.

| Check family | Proposed check | Required guardrail |
|---|---|---|
| Business identity | Confirm legal/existing business identity using accepted authoritative evidence | Evidence source and date recorded; no assumption from a website alone |
| Representative authority | Establish that the applicant can act for the business | Escalate inconsistent or unclear authority |
| Contact/domain control | Confirm control of business contact channel/domain where appropriate | Do not treat domain control as proof of legal identity |
| Business activity | Understand actual goods/services, business model and intended use | Restricted-category policy must be approved separately |
| Ownership/control | Assess ownership/control only to the extent required by approved policy | Sensitive data minimized and access restricted |
| Sanctions/adverse-risk screening | Only if legally appropriate and operationally approved | Qualified compliance review, false-positive handling and documented escalation |
| Settlement/funding details | Verify only when the approved funds-flow model requires them | Never collect or activate money-out destinations without stronger controls |
| Provider eligibility | Record written provider permission and merchant-specific prerequisites | Public API docs or portal visibility are not permission |
| Security/integration | Review webhook destination, credential custody and integration readiness | Sandbox first; do not request production secrets in chat or tickets |
| Privacy/terms | Record applicable notices, consent/acknowledgement where required and policy version | Legal basis and retention must be reviewed |
| Risk review | Assess unusual patterns and exceptions under approved policy | No invented numeric score/threshold; human escalation where required |

A reviewer must record the result and evidence reference, not copy full sensitive documents into a general-purpose audit log. A “pass” means the defined check was performed under an approved procedure; it does not replace other checks.

## 6. Review, decision and separation of duties

### 6.1 Proposed roles (roles are not assigned people)
- **Applicant/merchant administrator:** submits and maintains the application; cannot approve their own production activation.
- **Merchant operations reviewer:** checks completeness and manages information requests.
- **Verification/compliance reviewer:** evaluates identity, activity and applicable compliance evidence.
- **Risk reviewer:** evaluates risk concerns and restrictions under an approved policy.
- **Technical integration reviewer:** checks integration/security prerequisites where needed.
- **Provider/partnership owner:** tracks written provider authorization and account-specific prerequisites; does not infer permission from API availability.
- **Finance/reconciliation reviewer:** verifies the approved funds-flow, settlement and reconciliation prerequisites relevant to the requested capability.
- **Approving authority:** records the accountable final decision, based on a defined delegation/threshold matrix.
- **Security/operations owner:** controls credentials, activation, monitoring, suspension and incidents.
- **Independent reviewer:** reviews policy and control design where independence is required; cannot be replaced by self-review.

All role assignments remain unassigned until named people are explicitly appointed.

### 6.2 Decision outcomes
- **Approve for sandbox:** only after minimum application and account-security requirements are met.
- **Request information:** list each missing item, why it is needed, who can provide it and a review target if policy defines one.
- **Escalate:** preserve the unresolved issue, assign a qualified reviewer and block consequential activation.
- **Approve with restrictions:** state the precise allowed methods, currencies, capabilities, limits, expiry/review date and rationale; restrictions must be enforceable by the system.
- **Reject:** record a controlled reason category and reviewer authority; disclose only appropriate information.
- **Defer:** use where external evidence or a decision is pending. Do not represent deferred status as approval.

High-risk decisions, overrides, production activation and capability expansion require a recorded approver under a policy that is still to be approved. Dual control should be considered for sensitive money-out enablement, beneficiary/destination changes, high-risk exceptions and reinstatement after serious incidents.

## 7. Evidence and decision records

Each check/decision should record:
- Unique application and decision identifiers.
- Merchant tenant identifier and application version.
- Check/decision type and exact policy version.
- Outcome, reason category and concise rationale.
- Evidence reference, source/type, collection time, reviewer and review time.
- Expiry/review date where relevant.
- Required follow-up, accountable role and blocking capability.
- Approver identity/role and approval timestamp when applicable.
- Related risk/incident/exception record and linked decision IDs.
- Change history, including who changed a field or status and why.

Evidence storage must be access-controlled and separated from public documentation. Audit records should be tamper-evident and retain only necessary metadata. Avoid duplicating personal information into every event. Define retention/deletion, legal hold and access-request processes with qualified privacy/legal review.

## 8. Sandbox access and developer readiness

Sandbox approval is separate from business verification and production activation.

Before sandbox access:
1. Establish a secure merchant workspace and administrator identity.
2. Apply minimum anti-abuse and acceptable-use controls.
3. Issue environment-specific sandbox credentials through the approved secret-display flow.
4. Require least-privilege scopes and support revocation/rotation.
5. Document that sandbox outcomes are simulated or test-environment outcomes and are not real settlement.
6. Require merchant-server handling of credentials; no secret keys in browser/mobile code.
7. Validate webhook destination ownership and safety before sending events.
8. Provide integration guidance consistent with DOC-49 and test scenarios.
9. Ensure sandbox data, logs, webhook secrets and simulated transactions cannot enter production ledgers or reports.

Sandbox enablement does not mean that the merchant is verified for production, that a provider has approved the merchant, or that HARAI is authorized to aggregate payments.

## 9. Production activation gates

Production activation must be an explicit, evidence-backed decision. The proposed gate checklist below is cumulative and capability-specific.

| Gate | Required evidence | Default if missing |
|---|---|---|
| Merchant identity/eligibility | Approved verification results and accepted merchant-category policy | Block |
| Representative authority | Verified authority and recorded reviewer | Block |
| Legal/compliance | Qualified review of HARAI operating model and relevant merchant activity | Block |
| Provider permission | Written, applicable provider/processor authority for HARAI's actual role and the specific capability | Block |
| Commercial terms | Applicable fees, settlement, reserves, limits, support and obligations documented | Block |
| Funds-flow | Approved funds-flow model, funding source, settlement responsibility and financial-risk treatment | Block |
| Finance/reconciliation | Approved ledger/accounting assumptions and reconciliations for the capability | Block |
| Security | Credential, tenant isolation, callback/webhook, abuse and sensitive-action controls reviewed | Block |
| Integration readiness | Approved API/webhook contract and relevant executed conformance evidence | Block |
| Operations | Named incident/reconciliation owners, runbooks, alerts, recovery and support coverage | Block |
| Independent review | Required independent sign-off recorded | Block |
| Release authorization | Explicit approval by the accountable release authority | Block |

The gate is not passed merely because a portal account exists, API documentation is public, a sandbox request succeeds, a document has been written or a developer says a test should pass.

### 9.1 Capability-level activation
Maintain an explicit capability record for each merchant, including:
- Capability name and provider/method.
- Environment and market/currency scope.
- Status: requested, blocked, sandbox-enabled, production-pending, enabled, restricted, suspended or revoked.
- Required evidence and decision references.
- Approved limits/policies only after they are explicitly set.
- Activation approver, timestamp, expiry/review date and revocation reason.
- Whether collections, refunds, reversals, payouts/money-out, card processing or other operation families are independently enabled.

An overall merchant status must not override a blocked capability. If MTN collection is approved, this does not imply Airtel, disbursements, refunds or card processing are approved.

## 10. Credential issuance and access controls

- Issue credentials only after the relevant account/environment approval.
- Generate secrets securely, display them only through a controlled flow, and never retrieve or reveal them again in plaintext.
- Store credential verifiers or secrets using an approved secure design; encrypt where secret recovery is required.
- Bind every credential to one merchant, environment, scopes, creation event and owner.
- Support immediate revocation, rotation, expiry where applicable, usage review and compromise response.
- Notify authorized merchant administrators of creation, rotation and revocation without including the secret.
- Prevent credential issuance when the merchant/capability is blocked, suspended or closed.
- Separate dashboard users from API credentials; enforce MFA or equivalent strong authentication for privileged administration after the identity design is approved.
- Enforce tenant checks at every API/resource lookup and asynchronous worker boundary.
- Do not treat IP allowlisting as identity or authorization; safely handle proxy trust and IP changes.
- Audit every privileged action and review failed-login/credential-abuse patterns.

## 11. Ongoing monitoring and periodic review

Active status is conditional on continued eligibility and control effectiveness. Define a proportionate schedule only after legal, provider, risk and operational review.

Review triggers may include:
- Material changes in legal name, ownership/control, representative, country, business activity, website/domain or settlement details.
- New requested method, currency, provider, refund capability or money-out capability.
- Repeated failed/ambiguous transactions, unusual volume, complaint/dispute patterns or reconciliation discrepancies.
- Credential compromise, webhook abuse, data incident or security control failure.
- Provider notices, contract changes, policy changes or loss of access/permission.
- Expired verification evidence or failed periodic review.
- Dormancy, suspected account takeover or credible fraud concerns.

A trigger should create a traceable review case, not automatically infer wrongdoing. Where an immediate risk exists, apply proportionate temporary restrictions under a pre-approved authority matrix and record the reason. Do not invent thresholds in code before the policy is approved.

## 12. Suspension, restriction and reinstatement

### 12.1 Suspension/restriction reasons
Proposed reason categories include suspected compromise, expired/invalid verification, unresolved material inconsistency, provider permission withdrawal, compliance hold, severe security issue, unresolved financial/reconciliation discrepancy, repeated policy violation, merchant request or closure process.

### 12.2 Required controls
- Record who initiated the restriction, authority, timestamp, reason category, evidence reference and affected capabilities.
- Apply restrictions atomically where possible; ensure API keys, sessions, queued jobs and webhook configuration honor the new state.
- Prevent new operations on blocked capabilities. Existing in-flight operations must be safely observed/reconciled; do not blindly cancel, retry or mark them failed.
- Preserve access for authorized investigation/support and legally required record access without leaving normal transaction access open.
- Notify the merchant when appropriate, without disclosing sensitive investigation information or undermining legal obligations.
- Record appeal/remediation steps and review ownership.
- Reinstatement requires fresh evidence and approval at the required authority level; the original activation decision must not silently reactivate access.
- After serious compromise, rotate/revoke credentials and revalidate endpoints before reactivation.
- Keep audit/financial history append-only; suspension does not delete transactions or ledger records.

## 13. Closure and data lifecycle

Closure can be initiated by the merchant, HARAI under approved policy, provider/contractual requirement or legal obligation. The exact grounds and notice requirements require review.

Proposed closure workflow:
1. Authenticate and authorize the closure request or record the authorized initiator.
2. Confirm pending operations, unknown payment outcomes, refunds, disputes, reconciliation discrepancies and support cases are assigned for resolution.
3. Disable new operations and revoke or schedule revocation of API credentials and sessions according to the approved closure plan.
4. Disable new webhook configuration and safely manage outstanding event delivery/replay.
5. Reconcile and preserve required financial/audit records; never erase or rewrite posted ledger history.
6. Identify data subject requests, contractual/legal retention, legal holds and deletion/anonymization obligations.
7. Remove unnecessary personal/verification data when permitted and after retention requirements are resolved; retain only what is justified and authorized.
8. Record the final closure decision, actor, timestamp, unresolved obligations and evidence.
9. Mark the account `CLOSED` only when required steps are complete or a formally approved exception is recorded.

Closure must not imply that all underlying financial obligations are settled. Keep a restricted, audited path for authorized records access and post-closure legal/financial issues.

## 14. Exceptions, appeals and manual overrides

- No informal chat message or undocumented manual change may grant production access.
- Every exception needs a unique record, reason, scope, affected controls, risk assessment, compensating controls, expiry, named approver and review date.
- Exceptions cannot waive provider authority, applicable law, mandatory security/financial controls or the production release gate.
- Record rejected and expired exceptions, not just approved ones.
- Appeals should be handled by a role not solely responsible for the original decision when feasible.
- Manual status changes must validate allowed transitions, preserve the prior state and create an audit event.
- A manual override must not directly mark a payment successful, post ledger entries, retry an ambiguous provider operation or enable a blocked money-moving capability without the relevant separate authority and verified evidence.

## 15. Privacy and data-protection requirements

- Maintain a data inventory and purpose for every application field and verification artifact.
- Confirm lawful basis, notices, retention, access, correction, deletion, cross-border handling and processor/controller responsibilities with qualified Uganda privacy/legal review.
- Minimize data collected and visible to each role. Support staff should see masked/summary information unless full access is necessary and authorized.
- Encrypt sensitive data in transit and at rest using approved key management; restrict downloads and record access.
- Keep verification documents out of logs, metrics, general analytics and public documentation.
- Define secure upload, malware scanning, quarantine, access expiry and deletion.
- Protect against insecure direct object references and cross-tenant file access.
- Do not use unapproved third-party screening services or transfer personal data externally without a reviewed purpose, authority and agreement.
- Provide a documented breach/incident escalation route and preserve only evidence needed for investigation and legal duties.

## 16. Proposed test and acceptance catalogue

These identifiers are planning proposals only. No test is claimed to exist, have run or passed.

| Test ID | Scenario | Expected property |
|---|---|---|
| ONBOARD-001 | Incomplete application submitted | Cannot move into approval/activation; required fields identified safely |
| ONBOARD-002 | Applicant attempts to approve own application | Separation-of-duties rule blocks the action |
| ONBOARD-003 | Duplicate/possible duplicate application | Review flag created without leaking another merchant's data |
| ONBOARD-004 | Evidence is expired or reviewer outcome missing | Production activation remains blocked |
| ONBOARD-005 | Reviewer requests more information | Status and request are audited; only authorized fields are exposed |
| ONBOARD-006 | Conflicting identity/activity evidence | Escalation required; no silent auto-approval |
| ONBOARD-007 | Sandbox merchant tries production credential/API | Denied; environment isolation enforced |
| ONBOARD-008 | Active collection merchant requests money-out | Money-out remains blocked until separate capability gates pass |
| ONBOARD-009 | Provider approval evidence absent | Production capability activation denied |
| ONBOARD-010 | Merchant suspended while jobs are queued | New work blocked; in-flight/unknown operations handled safely |
| ONBOARD-011 | Credential used after revocation | Authentication denied within approved revocation expectations |
| ONBOARD-012 | User attempts cross-tenant verification-file access | Access denied and security event recorded |
| ONBOARD-013 | Material business change submitted | Review case created; applicable capability restrictions applied by policy |
| ONBOARD-014 | Reinstatement requested after serious incident | Requires remediation evidence and authorized approval |
| ONBOARD-015 | Closure requested with unknown payment outcome | Closure workflow preserves and assigns reconciliation; no false finality |
| ONBOARD-016 | Closed merchant attempts API access | Normal access denied; authorized records path remains controlled |
| ONBOARD-017 | Unauthorized staff attempts manual activation | Denied and audited |
| ONBOARD-018 | Exception has expired | Exception no longer grants access; review/alert raised |
| ONBOARD-019 | Verification file uploaded with unsafe type/content | Quarantined/rejected under approved scanning policy |
| ONBOARD-020 | Audit record altered/deleted by ordinary role | Prevented; integrity monitoring detects privileged tampering |

Executed evidence must identify the actual test file/case, commit/build, environment, command/run ID, timestamp, result and artifact as required by DOC-45. Test IDs above are not execution evidence.

## 17. Traceability to existing requirements and gates

- **DOC-31:** normative launch requirements; especially merchant onboarding, verification, access control, capability activation, tenant isolation, security and release gates.
- **DOC-34:** requirement-level mapping; add explicit mapping from ONBOARD-* IDs to relevant requirement IDs and actual tests when implementation is authorized.
- **DOC-43:** threat model; verify cross-tenant access, account takeover, support abuse, credential compromise and unsafe manual resolution.
- **DOC-45:** evidence standards; decisions, checks and tests require reviewable records.
- **DOC-46:** G2 gap closure; assign named accountable people and closure evidence.
- **DOC-48:** provider-specific capabilities and evidence levels; portal/API documentation alone is not written permission.
- **DOC-49:** merchant API authentication, tenant scoping, idempotency, webhooks and API errors; lifecycle state must be enforced consistently by every endpoint and worker.
- **DOC-30:** qualified Uganda legal/compliance review; this document does not determine statutory obligations.

## 18. Open decisions and blockers

1. What legal/compliance framework and operating role applies to HARAI's chosen funds-flow model in Uganda?
2. Which merchant categories are allowed, restricted or prohibited, and who approves the policy?
3. Which business/representative/ownership evidence is required, from which authoritative sources, and how are exceptions handled?
4. What lawful basis, notice, retention, deletion and cross-border controls apply to each category of personal data?
5. Which roles and named people perform verification, risk review, approval, credential operations, suspension and appeals?
6. What separation-of-duties and dual-control rules apply to production activation, money-out, overrides and reinstatement?
7. What provider/processor written permission is required per merchant, method, market and capability?
8. What exact capability/limit matrix can be enforced safely by the system?
9. What re-verification cadence and trigger policy is proportionate and contractually/legal required?
10. What notice/appeal requirements apply to restriction, rejection, suspension and closure?
11. Which retention periods and legal-hold requirements apply to verification and financial records?
12. Which production readiness evidence and independent reviews must be recorded before activation?
13. How will onboarding and capability state synchronize atomically with API keys, webhooks, queued jobs, provider adapters and financial operations?
14. Which final lifecycle states, transition permissions and service-level targets are approved?

## 19. Current disposition

DOC-50 is a proposed lifecycle and control model for future review. State names, evidence requirements, roles, retention, decision authority, restrictions and operational timing remain subject to accountable product decisions and qualified legal/compliance, privacy, security and operations review. No named reviewer or approver is assigned by this document.

**G0 — Discovery and requirements: IN PROGRESS. G1 — Requirements approval: NOT PASSED. G2 — Architecture approval: NOT APPROVED.** No implementation or tests are claimed. This document does not authorize provider outreach, onboarding, production credential issuance, real financial activity, merge or deployment.
