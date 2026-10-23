# DOC-59 — API Security, Authentication, Authorization and Credential Lifecycle Specification

**Status:** Draft security-policy proposal — not approved for implementation
**Scope:** Merchant API access, dashboard identity and permissions, tenant isolation, IP restrictions, provider secrets, request abuse/replay controls, auditability and compromise response.
**Gate status:** G1 NOT PASSED. G2 NOT APPROVED.

> This document defines security outcomes and decision points, not a final cryptographic or deployment design. Exact algorithms, token formats, lifetimes, rate limits, IP ranges, secret-management products and implementation mechanisms remain open pending accountable security and architecture review. It authorizes no code, tests, provider contact, credentials, real transactions, merge or deployment.

## 1. Security objectives and principles

1. Authenticate every protected actor and authorize every sensitive operation on the server.
2. Apply least privilege, deny-by-default behavior, tenant isolation and separation of duties.
3. Treat merchant API keys, user sessions, webhook secrets and provider credentials as distinct credential classes with separate permissions and lifecycles.
4. Never treat an IP address, browser state, merchant-supplied identifier, UI visibility or possession of a transaction ID as sufficient authorization.
5. Never expose provider credentials or internal secrets to merchant users, customer browsers, mobile applications, logs, URLs or client-side bundles.
6. Make credential creation, use, rotation, revocation, failed authentication and sensitive administrative changes auditable without logging secret values.
7. Preserve payment-state and financial-integrity controls: authentication, HTTP acceptance, timeout, callback receipt or a successful signature check alone does not prove a payment succeeded.
8. Prefer secure failure behavior while providing controlled recovery and a documented path for legitimate lockouts.

## 2. Credential classes and trust boundaries

| Credential / identity | Intended holder | Allowed purpose | Must not be used for |
|---|---|---|---|
| Merchant API key | Merchant backend only | Call explicitly granted merchant API operations | Browser/mobile embedding, dashboard login, provider API access, cross-tenant access |
| Dashboard user identity and session | Named merchant or HARAI operator | Interactive dashboard access under assigned roles | Server-to-server API authentication or sharing between people |
| Customer payment action / checkout reference | Customer-facing flow | Authorize or continue a specific checkout action under its own controls | Merchant administration or provider credential access |
| Webhook signing secret or equivalent | HARAI sender and configured merchant receiver | Authenticate event deliveries, if webhook design selects shared-secret signing | General API authentication or dashboard login |
| Provider credential | HARAI-controlled provider adapter/runtime | Only approved provider operations under documented provider authority | Merchant API responses, frontend delivery, customer access or unrelated environments |
| Internal service identity | Approved HARAI workload | Narrow service-to-service calls | Interactive human access or broad unrestricted database access |

Credentials must not be interchangeable. A merchant key must not be accepted as a dashboard session; a dashboard session must not implicitly grant provider access; provider credentials must remain inside the approved server-side integration boundary.

## 3. Merchant API key lifecycle

### 3.1 Creation and display

- Only an authenticated, authorized merchant administrator may create or manage keys for the correct merchant and project/environment scope.
- Key creation must capture a human-readable label, environment, intended scopes, creation actor/time and any approved expiry or policy metadata.
- A newly issued secret should be displayed only through a controlled one-time reveal where technically feasible. Store only a suitable verifier/hash when the key design permits verification without retaining the raw secret; any exception requires a documented security rationale and protection plan.
- Never place raw keys in application logs, analytics, error messages, URLs, support tickets or source control.
- Test and production credentials must be distinct. Sandbox/test keys must not authorize production operations, and production keys must not be silently downgraded to test behavior.
- Key material, prefixes, lengths, formats and hashing/verification algorithm remain design decisions requiring security review; do not infer them from this document.

### 3.2 Scope and authorization

- Keys must have explicit, server-enforced scopes aligned to operation families and merchant entitlements; exact scope names and granularity remain open.
- Scope checks must occur on every request and be combined with merchant/project ownership, environment, operation status, transaction state, limits and any required approval.
- A key may not grant broader permissions merely because the associated user has dashboard privileges.
- High-impact operations such as refunds, money-out, credential management or changing payout/settlement details may require additional role checks, transaction limits, step-up authentication, dual control or out-of-band approval. Which controls apply must be determined by the risk policy and provider/legal constraints.
- Use separate keys for distinct applications or operational purposes where it improves containment and revocation; avoid shared, untraceable keys.

### 3.3 Rotation, expiry and revocation

- Provide an auditable lifecycle: create, reveal, use, rotate, expire, revoke and investigate.
- Rotation should support a controlled overlap period only if the approved policy needs it; the old key must have a clear retirement point. No default overlap duration is set here.
- Revocation must prevent subsequent authorization after the system's defined propagation bound. That bound and cache-invalidation behavior must be explicitly specified and verified later.
- Support emergency revocation without requiring the compromised key to authenticate the revocation request.
- Record actor, timestamp, key identifier/fingerprint, reason and outcome, but never record the secret.
- Decide whether expiry is mandatory, configurable or risk-based after considering merchant operations, risk and support recovery.

## 4. Dashboard identity, sessions and privileged actions

- Every human operator must have an individual identity; shared operator accounts are prohibited as a proposed policy.
- Authentication requirements, supported identity providers, MFA/step-up rules, session lifetime, idle timeout, refresh behavior, device/session revocation and recovery controls remain open pending review.
- Enforce server-side role and permission checks on every dashboard API operation. Hiding a button is not an access-control mechanism.
- Enforce tenant and project scope in data access, not merely in UI filters. Verify object ownership for every identifier supplied by a caller to prevent insecure direct-object references.
- Require re-authentication or step-up verification for selected high-risk actions if the approved risk model requires it, including credential changes, payout-detail changes, role grants, sensitive exports and money-out approvals.
- Separate merchant roles from HARAI internal support, operations, finance, security and administrative roles. Internal access must be explicitly granted, time-bounded where feasible, and audited.
- Support session revocation on account compromise, role change, user deactivation and other approved security events.
- Define secure account recovery and administrator recovery that do not let support staff bypass controls without an auditable, approved exception.

## 5. Tenant, project and environment isolation

- Derive the authorized merchant/project context from validated authentication and server-side records, never trust a caller-supplied tenant ID as proof of access.
- Scope reads, writes, exports, background jobs, cache keys, object storage paths, idempotency records, webhook destinations and audit views to the correct tenant/project/environment.
- Ensure asynchronous workers revalidate trusted ownership and authorization context rather than trusting a client-supplied message payload alone.
- Prevent test credentials, data and provider configurations from crossing into production; enforce environment separation in authorization and configuration.
- Treat identifiers as references, not secrets. Unpredictable identifiers do not replace authorization.
- Tenant-isolation verification must include negative cases across API, dashboard, exports, workers, callbacks, logs and support tooling. Proposed verification only; no tests are authorized or claimed as run.

## 6. IP allowlisting and network trust

### 6.1 Proposed policy

IP allowlisting is a supported, configurable additional control, **not a universal mandatory default at this planning stage**. A later approved policy may require it for selected merchant risk tiers, sensitive scopes or integration types.

- Configure allowlists against the merchant server's observed public outbound source IP address or approved CIDR range—not the customer's phone IP or the customer's mobile-money network address.
- Explain that NAT, hosting changes, serverless platforms, outbound gateways and multiple backend regions may affect the source address. Merchants should confirm the egress address with their hosting/network administrator.
- Treat allowlist changes as security-sensitive, auditable actions; consider step-up verification or approval based on risk.
- Enforce allowlists server-side and fail closed when an enabled policy cannot be evaluated safely.
- IP allowlisting does not replace key authentication, scopes, tenant checks, rate limiting, signature validation or fraud controls.

### 6.2 Proxy and address parsing requirements

- Define which reverse proxies/load balancers are trusted before using forwarded-IP headers. Ignore forwarded headers from untrusted peers.
- Do not let an arbitrary caller override the source address by supplying `X-Forwarded-For` or similar headers.
- Specify canonical IP parsing, IPv4/IPv6 support, CIDR semantics, duplicate/overlapping ranges and behavior for malformed addresses.
- Document legitimate proxy and hosting arrangements before activation; avoid a configuration that accidentally trusts the public internet as a proxy.
- The trusted-proxy topology, source-IP derivation algorithm, supported range formats and admin UX remain unapproved decisions.

## 7. Provider credentials and secret management

- Provider credentials must be held and used only by the approved server-side adapter/runtime, subject to documented provider permissions and environment separation.
- Never request that merchants paste provider secrets into a HARAI merchant API-key field. Merchant API keys and provider credentials serve different trust relationships.
- Select an approved secret manager/KMS or equivalent protected storage mechanism, access policy, encryption/key-management model, rotation process, backup/recovery process and audit integration before implementation.
- Restrict secret access to the smallest required workload and named operational roles; prefer runtime retrieval and short-lived access where supported.
- Keep production and sandbox/test provider credentials separate. Never use live credentials in local development, public fixtures or test output.
- Redact secrets from logs, traces, crash reports, support tooling, database exports and observability labels. Avoid secrets in command-line arguments where they may be exposed to process listings.
- Define how credentials are provisioned, verified, rotated, revoked, disabled after provider-side changes and removed when an integration is decommissioned.
- No provider credential, production access or provider authorization is established by this document. Written provider permissions and the approved funds-flow model remain prerequisite evidence.

## 8. Request abuse, replay and rate controls

- Define rate limits by relevant dimensions such as credential, merchant, endpoint/operation, source network and risk signals; exact limits, windows and burst behavior remain open.
- Apply stricter controls to authentication failures, credential creation/revocation, payout/refund operations, sensitive reads, webhook management and other abuse-prone endpoints as supported by the risk model.
- Return stable, documented errors without revealing secrets, internal stack traces, tenant existence unnecessarily or provider credentials.
- Use idempotency controls for supported mutation operations as defined by DOC-58 and related transaction documents. Bind an idempotency key to the authorized merchant scope and a canonical request fingerprint; define conflict behavior and retention before implementation.
- Idempotency is not a substitute for authentication or replay protection.
- Use appropriate timestamp/nonce/signature or protocol-level replay defenses where the selected protocol requires them; define clock tolerance, nonce storage and duplicate handling in the relevant contract.
- Do not retry consequential financial operations blindly after an ambiguous timeout. Follow DOC-41 state handling and DOC-42 reconciliation principles; an unknown result remains unknown until authoritative evidence resolves it.
- Define abuse detection, alert thresholds, automated throttling/lockout behavior and safe recovery so attackers cannot easily cause permanent denial of service for legitimate merchants.

## 9. Webhook and callback security dependencies

- Webhook receiver configuration and delivery authentication must be specified in DOC-14 and reconciled with this document.
- If signed deliveries are selected, specify signature construction, key identification/rotation, payload canonicalization, timestamp/replay window and constant-time verification behavior after security review.
- Verify inbound provider callbacks according to the provider's documented mechanism; source IP alone is not sufficient unless the provider's official protocol explicitly defines it as part of a validated scheme.
- Deduplicate deliveries and correlate them to the expected provider account, environment, merchant and transaction context.
- A valid delivery signature authenticates the message source/integrity under the selected scheme; it does not independently prove a transaction's final state or authorize arbitrary state transitions.
- Keep callback/webhook secrets distinct from merchant API keys and provider credentials.

## 10. Audit logging and security evidence

Record enough structured metadata to investigate security-sensitive events, including where applicable:

- actor or workload identity, tenant/project/environment and credential identifier (not the secret);
- action, target object identifier, authorization decision and policy version;
- timestamp, outcome, reason category, correlation/request ID and trusted source-network context;
- key/session creation, rotation, revocation, permission changes, allowlist edits, role grants, privileged approvals, secret access and recovery actions;
- authentication failures and relevant rate-limit/security events, with controls against log flooding.

Audit records must be access-controlled, tamper-resistant to the degree selected by the architecture, time-synchronized, retained according to an approved retention policy and exportable for authorized investigation. Avoid recording payment authentication secrets, full credentials, unnecessary personal data or sensitive request bodies. Define who may view/export logs and how that access is itself audited.

## 11. Credential compromise and incident recovery

Proposed response sequence:

1. Receive and classify a suspected compromise report without requesting that the reporter transmit the secret.
2. Identify affected credential IDs, merchant/project/environment, relevant operations and time window from safe metadata.
3. Contain exposure: revoke or disable affected credentials/sessions, suspend risky scopes or operations, and restrict access as authorized.
4. Rotate impacted secrets through an authenticated, approved process; verify dependent services are updated before retiring any planned overlap.
5. Assess requests, transaction attempts, provider responses, webhook events and audit evidence. Preserve the distinction between confirmed outcomes and unknown outcomes.
6. Use reconciliation and provider-supported enquiries to resolve ambiguous payment outcomes; do not infer failure or success from a timeout alone.
7. Notify the appropriate merchant, provider, legal/compliance or internal incident owner under the approved notification policy and applicable obligations.
8. Record timeline, decisions, evidence, remediation, lessons learned and any required follow-up review.

Response owners, severity levels, notification deadlines, service-level targets and escalation contacts must be assigned in the operational readiness documents; none is established here.

## 12. Proposed verification and acceptance evidence (not executed)

Before security acceptance, prepare and later execute an approved evidence plan covering at least:

- invalid, revoked, expired, wrong-environment and wrong-scope merchant keys are rejected;
- dashboard sessions and roles cannot access ungranted operations, and privileged changes are audited;
- one tenant cannot read, mutate, export or infer another tenant's records across synchronous and asynchronous paths;
- allowlist enabled/disabled behavior, IPv4/IPv6/CIDR parsing, trusted-proxy spoofing resistance and safe failure behavior;
- provider secrets never appear in browser bundles, merchant responses, logs, traces, support exports or repository history;
- rotation, revocation propagation, session termination and compromise recovery meet approved bounds;
- rate limits and abuse handling do not bypass authorization or corrupt financial state;
- idempotency/replay cases and ambiguous payment outcomes follow DOC-41, DOC-42, DOC-14 and DOC-58;
- security audit events are complete enough to investigate actions without exposing secrets;
- security-sensitive errors do not disclose internal implementation details or cross-tenant data.

For each eventual check, record requirement IDs, threat/control reference, environment, exact procedure, expected result, actual result, evidence location, reviewer and date. Test IDs and evidence are proposals only. **No tests have been run or authorized by this document.** Map applicable checks to stable DOC-31 requirements through DOC-34 and retain evidence under DOC-45.

## 13. Open decisions that require accountable review

1. Merchant key format, entropy, prefix, verifier/hash approach and any expiry policy.
2. Scope vocabulary, project/environment binding and any separate keys for high-risk operations.
3. Dashboard identity provider, MFA/step-up requirements, session/refresh/idle lifetimes and recovery design.
4. Roles, separation of duties, emergency access and approval requirements for refunds, money-out and settlement-detail changes.
5. IP allowlist default and any mandatory rules by merchant risk tier; IPv4/IPv6/CIDR support and safe admin recovery.
6. Trusted proxy/load-balancer boundaries and authoritative client-IP derivation.
7. Secret manager/KMS, runtime access model, encryption/key ownership, rotation and disaster recovery.
8. Rate limits, burst limits, abuse signals, lockout/throttling and operational alerts.
9. Replay/idempotency retention and exact protocol-specific callback/webhook signature rules.
10. Audit retention, access, tamper resistance, privacy/data minimization and incident-notification obligations.
11. Named accountable security reviewer, independent architecture reviewer, operational incident owner and evidence approvers.

Do not silently fill these gaps with arbitrary numeric defaults or assume that a proposed policy is approved.

## 14. Related documents

- DOC-00 — Master index and decision register.
- DOC-12 — API contracts and versioning.
- DOC-13 — API keys, IP controls and credential security.
- DOC-14 — Webhooks and event delivery.
- DOC-31 — Launch requirements and acceptance criteria.
- DOC-34 — Requirement-level traceability matrix.
- DOC-40–DOC-44 — Architecture, transaction, ledger, threat-model and review proposals.
- DOC-45 — Evidence register and acceptance-test planning standard.
- DOC-47 — Architecture review and contradiction register.
- DOC-49 — Merchant API specification and developer integration guide.
- DOC-50–DOC-55 — Onboarding, dashboard permissions, navigation, design and frontend/API mappings.
- DOC-56 — Backend domain model and data ownership matrix.
- DOC-57 — Backend service boundaries and end-to-end data flow.
- DOC-58 — Backend API and internal operation contract blueprint.

## 15. Gate and authorization statement

DOC-59 is a draft security-policy proposal. It does not constitute a completed security review, approved cryptographic design, provider authorization or implementation plan approval.

**G1 remains NOT PASSED. G2 remains NOT APPROVED.** No payment-engine implementation, test execution, provider contact, credential generation, live financial activity, merge or deployment is authorized by this document. Those activities require the relevant gates and separate explicit authorization.

## 16. Tenant-context and authorization decision rules — proposed clarification

For architecture and eventual acceptance criteria, interpret the isolation requirements as follows:

1. **Trusted tenant resolution:** authenticate the credential/session and resolve current tenant membership, project and environment from server-side records before accessing merchant data. A tenant ID in a request, URL, callback, webhook, queue payload or support search is only an untrusted selector until independently authorized.
2. **Resource-level authorization:** authorize every requested object against its stored owner and parent relationship, not merely against a route-level role or caller-selected tenant. Denials must not disclose another tenant's existence or sensitive metadata.
3. **Async revalidation:** background work stores stable resource references and expected ownership metadata; workers reload the authoritative resource and reject mismatches, revoked credentials/capabilities, invalid lifecycle state or incompatible environment before any side effect.
4. **Callback ownership:** validate the provider-specific protocol, correlate the callback to a persisted attempt, then derive tenant and operation through that attempt. Unmatched/conflicting callbacks are quarantined and cannot select a tenant or directly post a ledger effect.
5. **Derived-data isolation:** include tenant/environment/resource scope in cache and read-model keys, report/export jobs, object-storage authorization, webhook delivery selection, idempotency scope and audit views. Reauthorize artifact downloads; public URLs are not access control.
6. **Privileged support:** require a distinct permission, stated purpose/case, target tenant/resource, time-bounded access where feasible, audit trail and narrowly scoped actions. Financial state changes use the canonical state/correction workflow and any separately approved dual-control rule.
7. **Fail closed:** absent, stale or contradictory ownership data blocks the action and creates a safe investigation/audit record. Do not silently infer ownership from a provider reference, tenant name, merchant-supplied value or cached authorization.
8. **Revocation and cache invalidation:** define and verify how membership, session, key, project, provider capability and support grants become ineffective across API nodes, caches, workers and long-running jobs. Until a concrete propagation/consistency policy is approved, do not claim immediate revocation as a verified property.

These are design requirements, not an approved implementation or chosen technology. The corresponding negative tests, evidence artifacts and independent/specialist review remain outstanding.
