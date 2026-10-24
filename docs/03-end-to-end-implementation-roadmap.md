# DOC-03 — HARAI End-to-End Implementation Roadmap

**Status:** Draft planning baseline — not an implementation authorization.  
**Planning rule:** Dates, estimates, costs and provider commitments will be added only after scope, dependencies and resourcing are verified.

## Release gates

- **G0 Discovery:** reference workflows, market and business model understood.
- **G1 Requirements:** scope, requirements, decisions and blockers reviewed.
- **G2 Architecture/security:** data model, tenancy, financial design and threat model approved.
- **G3 Foundation:** repository, CI, environments and deployment approach verified.
- **G4 Provider sandbox:** authorized provider contracts and callbacks proven in test environments.
- **G5 Staging:** end-to-end workflows and failure recovery pass.
- **G6 Independent review:** security, finance, operations and compliance evidence reviewed.
- **G7 Production authorization:** explicit approval for live credentials and money movement.
- **G8 Post-launch validation:** transactions reconciled and operations stabilized.

No phase skips its exit criteria.

## Phase 0 — Governance and discovery (G0)

### 0.1 Project control
- 0.1.1 Maintain master index and decision register.
- 0.1.2 Assign stable requirement, decision, risk and test IDs.
- 0.1.3 Define statuses, source-of-truth and change-control rules.
- 0.1.4 Keep foundation integration unmerged until DOC-62's branch-integration review and applicable acceptance conditions are met. Branch names and PR numbers are operational status, not permanent roadmap requirements.
- 0.1.5 Identify who can approve product, technical, security and production decisions.

### 0.2 Reference-product discovery
- 0.2.1 Inventory dashboard and navigation.
- 0.2.2 Inventory collections and payment links.
- 0.2.3 Inventory mobile-money flows by provider and direction.
- 0.2.4 Inventory bank and card options, if present.
- 0.2.5 Inventory payouts, scheduled withdrawals, refunds and reversals.
- 0.2.6 Inventory API keys, scopes, IP controls and rate limits.
- 0.2.7 Inventory webhook setup, events, delivery history and retries.
- 0.2.8 Inventory reporting, reconciliation and exports.
- 0.2.9 Inventory verification, support and security settings.
- 0.2.10 Label each item observed, partially observed, unconfirmed or proposed.
- 0.2.11 Do not perform live actions or collect secrets/customer data.

### 0.3 Business and market scope
- 0.3.1 Confirm initial countries, currencies and merchant segments.
- 0.3.2 Decide gateway routing versus custody/settlement responsibilities.
- 0.3.3 Identify provider onboarding and commercial prerequisites.
- 0.3.4 Define pricing, fees, settlement expectations and transaction limits.
- 0.3.5 Identify legal, regulatory, privacy and contractual review requirements.
- 0.3.6 Define V1 inclusions and exclusions.

**Exit:** DOC-00 through DOC-03 contain a coherent discovery baseline; assumptions remain visibly unresolved.

## Phase 1 — Product and functional requirements (G1)

### 1.1 Actors and merchant lifecycle
- 1.1.1 Define actors, roles and permissions.
- 1.1.2 Define registration, invitations, account recovery and sessions.
- 1.1.3 Define merchant onboarding, verification, rejection and resubmission.
- 1.1.4 Define activation gates and live-operation eligibility.
- 1.1.5 Define support workflow and privileged support access.

### 1.2 Collections and checkout
- 1.2.1 Define API-created collection requests.
- 1.2.2 Define hosted checkout and payment links.
- 1.2.3 Define payer consent, amount, currency and expiry.
- 1.2.4 Define provider selection and capability checks.
- 1.2.5 Define pending, success, failure and cancellation experience.
- 1.2.6 Define merchant notification and webhook expectations.
- 1.2.7 Specify payment-link/session expiry, revocation, cancellation and concurrent usage rules (SUP-002).
- 1.2.8 Define canonical transaction states, allowed transitions, terminality and unknown-outcome recovery (SUP-004).
- 1.2.9 Define API abuse limits for creation, status polling and recovery; specify per-tenant behavior and safe throttled responses (SUP-001).

### 1.3 Money-out and refunds
- 1.3.1 Decide which payout/withdrawal rails are in scope.
- 1.3.2 Define limits, authorization, step-up and dual approval.
- 1.3.3 Define beneficiary validation and management if applicable.
- 1.3.4 Define scheduled execution, cancellation and timezone rules if applicable.
- 1.3.5 Define ambiguous, failed, returned and reversed outcomes.
- 1.3.6 Define full/partial refund behavior only where provider-supported.
- 1.3.7 Define beneficiary-change step-up, audit, approval/cooling-off rules and handling of pending payouts (SUP-003).

### 1.4 API and event requirements
- 1.4.1 Define API authentication, scopes and versioning.
- 1.4.2 Define idempotency and request-conflict behavior.
- 1.4.3 Define rate limits, pagination and error taxonomy.
- 1.4.4 Define webhook signatures, IDs, retries and replay handling.
- 1.4.5 Define compatibility policy and developer examples.
- 1.4.6 Define correlation IDs and redaction rules (SUP-010).
- 1.4.7 Define CORS, CSRF, trusted-proxy, client-IP and IP-allowlist recovery boundaries (SUP-007).
- 1.4.8 Define stable machine-readable errors, bounded deterministic pagination, and compatibility/deprecation policy (SUP-006).

### 1.5 Financial/operational requirements
- 1.5.1 Define transaction state machine.
- 1.5.2 Define ledger, fees, rounding and settlement.
- 1.5.3 Define reconciliation and discrepancy handling.
- 1.5.4 Define audit, support access and data retention.
- 1.5.5 Define availability, alerting and incident targets.
- 1.5.6 Define backup, recovery and operational ownership.
- 1.5.7 Define measurable availability/latency, pending-age, incident escalation, RTO/RPO and restore-verification objectives (SUP-008).
- 1.5.8 Define dispute/complaint ownership, deadlines, evidence access and outcomes (SUP-005).
- 1.5.9 Define accessibility and responsive acceptance for keyboard, focus, contrast, labels/errors, zoom/reflow and critical payer/merchant journeys (SUP-009).

**Exit:** every in-scope requirement has an ID, dependencies, acceptance criteria and verification method; V1 scope is approved.

## Phase 2 — Provider capability and commercial verification

### 2.1 MTN MoMo
- 2.1.1 Confirm country, currency and exact API product.
- 2.1.2 Confirm authorized business/developer access and contracts.
- 2.1.3 Confirm sandbox and credential issuance.
- 2.1.4 Map collection, callback, status query, timeout and reversal.
- 2.1.5 Map disbursement only if separately supported and approved.
- 2.1.6 Record fees, limits, settlement and support.
- 2.1.7 Define sandbox contract scenarios.

### 2.2 Airtel Money
- 2.2.1 Confirm country, currency and exact API product.
- 2.2.2 Confirm authorized business/developer access and contracts.
- 2.2.3 Confirm sandbox and credential issuance.
- 2.2.4 Map collection, callback, status query, timeout and reversal.
- 2.2.5 Map disbursement only if separately supported and approved.
- 2.2.6 Record fees, limits, settlement and support.
- 2.2.7 Define sandbox contract scenarios.

### 2.3 Other methods
- 2.3.1 Decide whether bank collections are V1.
- 2.3.2 Decide whether bank payouts are V1.
- 2.3.3 Decide whether card acquiring is V1 and identify the acquirer.
- 2.3.4 If cards are included, specify authentication, disputes and chargebacks.
- 2.3.5 Record supported markets, currencies, limits, fees and settlement per method.
- 2.3.6 Remove unsupported methods from product/API claims.

**Exit:** every in-scope method has official references, authorized access path, capability matrix and sandbox plan.

## Phase 3 — Architecture, security and data design (G2)

### 3.1 Architecture
- 3.1.1 Compare candidate stack against requirements and deployment constraints.
- 3.1.2 Define dashboard, checkout, API, provider adapter, worker and data boundaries.
- 3.1.3 Define synchronous/asynchronous flows and queue behavior.
- 3.1.4 Define environments, secrets injection and deployment topology.
- 3.1.5 Record architecture decisions and trade-offs.

### 3.2 Data model
- 3.2.1 Define tenants, users, memberships, roles and permissions.
- 3.2.2 Define merchant profile and verification records.
- 3.2.3 Define API keys, scopes, allowlists and audit events.
- 3.2.4 Define payment intents, transactions and provider attempts.
- 3.2.5 Define idempotency records and request hashes.
- 3.2.6 Define webhook endpoints, events and delivery attempts.
- 3.2.7 Define ledger entries, fees, settlements and reconciliation cases.
- 3.2.8 Define payout/refund/reversal records if in scope.
- 3.2.9 Define support records and safe diagnostic references.
- 3.2.10 Define constraints, indexes, retention and deletion rules.

### 3.3 Security and threat model
- 3.3.1 Map trust boundaries and attack surfaces.
- 3.3.2 Define tenant isolation across API, database, jobs and webhooks.
- 3.3.3 Define authentication, MFA, session security and recovery.
- 3.3.4 Define API-key creation, one-time display, storage, rotation and revocation.
- 3.3.5 Define IP allowlist semantics, proxy trust and safe recovery.
- 3.3.6 Define webhook signing, timestamp tolerance and replay protection.
- 3.3.7 Define privileged-action approval and separation of duties.
- 3.3.8 Define encryption, secret management, logging redaction and access review.
- 3.3.9 Define abuse, fraud, rate-limit and anomaly controls.
- 3.3.10 Review risks and assign accountable closure owners.

### 3.4 API and event contracts
- 3.4.1 Define schemas, versioning and compatibility.
- 3.4.2 Define errors, correlation IDs and safe diagnostics.
- 3.4.3 Define idempotency headers and conflict responses.
- 3.4.4 Define canonical event payloads and signatures.
- 3.4.5 Define provider status/capability mapping.
- 3.4.6 Create schema and contract tests.

**Exit:** architecture, threat model, financial data model and API contracts are reviewed and approved before payment-engine coding.

## Phase 4 — Repository and delivery foundation (G3)

### 4.1 Repository quality
- 4.1.1 Review the foundation PR against approved architecture.
- 4.1.2 Finalize workspace boundaries and dependency policy.
- 4.1.3 Define formatting, lint, typecheck, build and test commands.
- 4.1.4 Define branch protection, required checks and review rules.
- 4.1.5 Define issue, PR, release and migration conventions.

### 4.2 CI and environments
- 4.2.1 Define local, test, staging and production configuration.
- 4.2.2 Keep environment examples placeholder-only.
- 4.2.3 Configure CI for lint, typecheck, tests and build.
- 4.2.4 Add dependency and secret scanning where available.
- 4.2.5 Define migration validation and rollback policy.
- 4.2.6 Define deployment approvals and artifact provenance.

### 4.3 Operations foundation
- 4.3.1 Configure structured logs and correlation IDs.
- 4.3.2 Configure health/readiness checks.
- 4.3.3 Define metrics, alert thresholds and escalation.
- 4.3.4 Define backup, restore and disaster recovery.
- 4.3.5 Verify no secrets or personal data are committed.

**Exit:** repeatable CI and safe development environments exist; no payment capability is implied.

## Phase 5 — Identity, tenancy and merchant onboarding

### 5.1 Authentication
- 5.1.1 Implement approved registration/invitation flow.
- 5.1.2 Implement login, logout, session expiry and recovery.
- 5.1.3 Implement MFA/step-up controls where required.
- 5.1.4 Test brute-force resistance and session invalidation.

### 5.2 Authorization
- 5.2.1 Implement membership, roles and permission checks.
- 5.2.2 Enforce tenant scope in every data access.
- 5.2.3 Enforce tenant context in queues, scheduled jobs and webhooks.
- 5.2.4 Test cross-tenant read/write denial and identifier tampering.
- 5.2.5 Audit role changes and privileged access.

### 5.3 Merchant onboarding
- 5.3.1 Implement approved profile and business details.
- 5.3.2 Implement verification states and protected evidence handling.
- 5.3.3 Implement review, rejection, resubmission and activation.
- 5.3.4 Block live operations until required activation gates pass.
- 5.3.5 Test privacy, evidence access and retention.

**Exit:** tenant-isolation and access-control tests pass; activation policy is enforced.

## Phase 6 — Transaction model and ledger

### 6.1 Transaction lifecycle
- 6.1.1 Implement approved states and guarded transitions.
- 6.1.2 Record provider attempts and safe references.
- 6.1.3 Record transition history and correlation IDs.
- 6.1.4 Handle pending/unknown outcomes without false finality.
- 6.1.5 Test duplicate and out-of-order events.

### 6.2 Idempotency and concurrency
- 6.2.1 Implement scoped idempotency and request-hash validation.
- 6.2.2 Reject conflicting reuse of an idempotency key.
- 6.2.3 Prevent duplicate provider operations under concurrency.
- 6.2.4 Define key retention and replay behavior.
- 6.2.5 Test process restart and database races.

### 6.3 Ledger and reconciliation
- 6.3.1 Implement auditable ledger entries and balance derivation.
- 6.3.2 Implement approved fee, rounding and net-amount rules.
- 6.3.3 Implement settlement and provider reconciliation.
- 6.3.4 Implement discrepancy investigation queues.
- 6.3.5 Implement authorized compensating entries for corrections.
- 6.3.6 Test financial invariants and reconciliation accuracy.

**Exit:** financial invariants and concurrency tests pass; finance/security reviewers approve the model.

## Phase 7 — Provider adapters and collections (G4)

### 7.1 Adapter contract
- 7.1.1 Implement common provider interface.
- 7.1.2 Map capabilities, errors, statuses and timeouts.
- 7.1.3 Implement provider authentication without logging secrets.
- 7.1.4 Add safe diagnostics and correlation IDs.
- 7.1.5 Add contract tests and safe mock fixtures.

### 7.2 MTN MoMo collections
- 7.2.1 Configure sandbox secrets outside source control.
- 7.2.2 Implement collection initiation and payer-consent flow.
- 7.2.3 Authenticate callbacks and verify provider status.
- 7.2.4 Implement status query and timeout recovery.
- 7.2.5 Test success, failure, pending, duplicate and late-callback cases.

### 7.3 Airtel Money collections
- 7.3.1 Configure sandbox secrets outside source control.
- 7.3.2 Implement collection initiation and payer-consent flow.
- 7.3.3 Authenticate callbacks and verify provider status.
- 7.3.4 Implement status query and timeout recovery.
- 7.3.5 Test success, failure, pending, duplicate and late-callback cases.

### 7.4 Hosted checkout and payment links
- 7.4.1 Implement checkout-session creation and expiry.
- 7.4.2 Validate amount, currency, merchant and methods server-side.
- 7.4.3 Implement payer-facing pending/success/failure states.
- 7.4.4 Implement link lifecycle, revocation and reuse rules.
- 7.4.5 Test tampering, expiry, duplicate payment and redirect spoofing.

**Exit:** authorized provider sandbox tests and collection lifecycle pass with reconciled outcomes. Live credentials are not used.

## Phase 8 — API access, webhooks and developer experience

### 8.1 API credentials
- 8.1.1 Generate scoped keys and display the secret only at creation.
- 8.1.2 Store only the approved verifier/hash or encrypted form.
- 8.1.3 Implement rotation, revocation, expiry and audit.
- 8.1.4 Implement optional IP allowlisting and safe recovery.
- 8.1.5 Enforce rate limits and test abuse cases.

### 8.2 Public API
- 8.2.1 Implement approved endpoint contracts.
- 8.2.2 Implement validation, errors, pagination and versioning.
- 8.2.3 Implement request correlation and redacted audit records.
- 8.2.4 Publish developer documentation and examples.
- 8.2.5 Add compatibility and schema tests.

### 8.3 Webhooks
- 8.3.1 Record events reliably with the corresponding state change.
- 8.3.2 Sign payloads and include event IDs/timestamps.
- 8.3.3 Implement bounded retry/backoff and delivery history.
- 8.3.4 Define deduplication and safe replay.
- 8.3.5 Protect against SSRF and unsafe endpoint configuration.
- 8.3.6 Test duplicates, delays, reordering, timeouts and endpoint changes.

**Exit:** API/webhook contracts are verified, including replay, rotation and delivery failures.

## Phase 9 — Money-out, refunds/reversals and conditional additional methods (provider/legal gates apply)

### 9.1 Payouts / withdrawals
Payouts/withdrawals are part of the intended comprehensive launch package, not a forgotten optional feature. They remain blocked from live implementation until provider capability, contractual permission, legal/compliance review, funds-flow and funding-source design, and operational controls are verified.
- 9.1.1 Verify provider capability and authorization requirements.
- 9.1.2 Validate beneficiaries, amounts and limits.
- 9.1.3 Enforce approval/dual-control gates where required.
- 9.1.4 Implement submission, status resolution and reconciliation.
- 9.1.5 Define ambiguous outcome, cancellation and reversal handling.
- 9.1.6 Test duplicate execution and unauthorized attempts.

### 9.2 Refunds / reversals
Refunds and reversals are part of the intended launch requirements where the specific provider flow supports them. Unsupported flows remain explicitly blocked; this does not imply that every provider offers every refund or reversal operation.
- 9.2.1 Define full/partial support per provider.
- 9.2.2 Validate refundable amounts against confirmed transactions.
- 9.2.3 Prevent over-refunds and concurrent refund races.
- 9.2.4 Record provider response and ledger consequences.
- 9.2.5 Test delayed, failed, repeated and reversed outcomes.

### 9.3 Bank/card methods
- 9.3.1 Implement only approved provider methods and markets.
- 9.3.2 Define bank references and reconciliation.
- 9.3.3 For cards, verify acquiring and authentication/dispute requirements.
- 9.3.4 Implement settlement, chargeback and evidence handling if approved.
- 9.3.5 Remove UI/API claims for unsupported methods.

### 9.4 Scheduling and reporting
- 9.4.1 Implement schedule/timezone rules only if approved.
- 9.4.2 Implement cancellation, execution locking and audit.
- 9.4.3 Define dashboard metric semantics.
- 9.4.4 Implement report/export permission and privacy controls.
- 9.4.5 Verify report accuracy against ledger/reconciliation data.

**Exit:** each enabled capability passes authorization, financial-integrity, provider and recovery tests.

## Phase 10 — Dashboard, support and accessibility

### 10.1 Merchant dashboard
- 10.1.1 Implement responsive navigation and summary cards.
- 10.1.2 Implement transaction search, filters, details and event timeline.
- 10.1.3 Display pending/unknown outcomes clearly.
- 10.1.4 Implement payment-link and webhook-delivery views.
- 10.1.5 Implement secure profile, verification and credential screens.

### 10.2 Operations
- 10.2.1 Implement support tickets and safe diagnostics.
- 10.2.2 Implement reconciliation discrepancy queues.
- 10.2.3 Implement risk/verification queues with explicit permissions.
- 10.2.4 Implement audited, time-limited support access.
- 10.2.5 Prevent silent staff mutation of financial records.

### 10.3 Usability
- 10.3.1 Design empty/loading/pending/error/success states.
- 10.3.2 Verify keyboard access, contrast, labels and status announcements.
- 10.3.3 Test mobile layouts and slow-network behavior.
- 10.3.4 Clearly identify merchant, amount and currency at checkout.
- 10.3.5 Verify target-market timezone and localization assumptions.

**Exit:** core workflows are usable, accessible and backed by server-side authorization.

## Phase 11 — Comprehensive verification (G5)

### 11.1 Automated coverage
- 11.1.1 Unit tests for domain rules and state transitions.
- 11.1.2 Database integration tests and migration verification.
- 11.1.3 Provider adapter contract tests.
- 11.1.4 API integration tests with sandbox/mock boundaries.
- 11.1.5 UI end-to-end tests for merchant and payer journeys.
- 11.1.6 Webhook/queue recovery tests.
- 11.1.7 Load/concurrency tests for idempotency and ledger invariants.
- 11.1.8 Regression tests for corrected defects.

### 11.2 Failure and adversarial scenarios
- 11.2.1 Cross-tenant access and IDOR.
- 11.2.2 Credential leakage, rotation and revocation.
- 11.2.3 IP allowlist/proxy edge cases.
- 11.2.4 Forged, replayed and duplicate callbacks.
- 11.2.5 Provider timeouts, outages and inconsistent responses.
- 11.2.6 Queue redelivery, worker restart and dead-letter recovery.
- 11.2.7 Database failure during partial workflows.
- 11.2.8 Double submission and concurrent payout/refund attempts.
- 11.2.9 Reconciliation discrepancies and correction audit.
- 11.2.10 Backup restoration and recovery exercise.

### 11.3 Traceability
- 11.3.1 Map every approved requirement to tests.
- 11.3.2 Record test environment and evidence.
- 11.3.3 Keep untested requirements blocked.
- 11.3.4 Run regression and review failures before sign-off.
- 11.3.5 Add explicit tests for SUP-001–SUP-010; map each proposed test ID to a real test case, environment, run and result only after implementation is authorized.
- 11.3.6 Verify API throttling, payment-link concurrency, beneficiary-change controls, forbidden state transitions, stable API errors, trusted-proxy behavior, restore evidence, accessibility, dispute-case access and audit-correlation scenarios.
- 11.3.7 Include the EXT-004 card-processor contract and security tests only after an eligible processor/acquirer, written permission, applicable data-security scope and sandbox access are confirmed.

**Exit:** traceability is complete; no critical/high risk is left unresolved without explicit accountable acceptance.

## Phase 12 — Independent security, finance and compliance review (G6)

### 12.1 Security
- 12.1.1 Review threat model and tenant isolation.
- 12.1.2 Review authentication, sessions and credential lifecycle.
- 12.1.3 Review webhook security and outbound endpoint protection.
- 12.1.4 Review logs, secrets, encryption and retention.
- 12.1.5 Review dependency risks and deployment permissions.
- 12.1.6 Verify privileged approvals and audit evidence.

### 12.2 Financial integrity
- 12.2.1 Independently review ledger and fee calculations.
- 12.2.2 Verify transaction/settlement reconciliation.
- 12.2.3 Review payout/refund concurrency and correction procedures.
- 12.2.4 Verify all money movement is attributable and auditable.
- 12.2.5 Verify support/admin access cannot silently alter financial truth.

### 12.3 Compliance/provider review
- 12.3.1 Obtain appropriate legal/regulatory review for the chosen model.
- 12.3.2 Confirm provider contracts and production permissions.
- 12.3.3 Confirm privacy notices, retention and user-rights procedures.
- 12.3.4 Confirm merchant verification and risk policies.
- 12.3.5 Confirm complaints, incident and reporting procedures.

**Exit:** independent findings have accountable owners; blockers are closed before production authorization.

## Phase 13 — Production readiness and launch authorization (G7)

### 13.1 Environment readiness
- 13.1.1 Inject production secrets through managed secret storage.
- 13.1.2 Separate test/live credentials, callbacks and data.
- 13.1.3 Verify database backups and restoration.
- 13.1.4 Configure TLS, access controls, rate limits and egress controls.
- 13.1.5 Configure monitoring, alerting and escalation.
- 13.1.6 Validate migrations and rollback/forward-fix strategy.
- 13.1.7 Confirm support and reconciliation ownership.

### 13.2 Runbooks
- 13.2.1 Provider outage and delayed confirmation.
- 13.2.2 Unknown transaction outcome.
- 13.2.3 Webhook backlog and dead-letter recovery.
- 13.2.4 Ledger/reconciliation discrepancy.
- 13.2.5 Suspected fraud or compromised credential.
- 13.2.6 Data incident and privacy request.
- 13.2.7 Rollback, restore and incident communication.

### 13.3 Release decision
- 13.3.1 Verify all previous gate exit criteria.
- 13.3.2 Review and explicitly accept or block each remaining risk.
- 13.3.3 Confirm live provider access and approved test plan.
- 13.3.4 Approve rollout limits and kill switches.
- 13.3.5 Record release decision and rollback owner.
- 13.3.6 Deploy only after explicit authorization.

**Exit:** documented release decision authorizes a defined live scope; this roadmap does not itself authorize launch.

## Phase 14 — Controlled launch and stabilization (G8)

### 14.1 Limited rollout
- 14.1.1 Enable only approved merchants, methods, markets and limits.
- 14.1.2 Monitor success rate, pending duration, provider errors and webhook delay.
- 14.1.3 Reconcile launch transactions against provider records.
- 14.1.4 Review support cases and operational alerts.
- 14.1.5 Disable affected methods safely if critical anomalies occur.

### 14.2 Stabilization
- 14.2.1 Resolve launch defects under change control.
- 14.2.2 Verify fees, settlement and reconciliation over a complete cycle.
- 14.2.3 Review security logs and unexpected access.
- 14.2.4 Reconfirm backup and recovery.
- 14.2.5 Update requirements, tests and runbooks from lessons learned.

### 14.3 Expansion decision
- 14.3.1 Review operating evidence and outstanding risks.
- 14.3.2 Decide separately whether to expand merchants, volumes, markets or methods.
- 14.3.3 Update provider capability and capacity plans.
- 14.3.4 Record explicit approval for the next release.

**Exit:** operations and reconciliation are evidenced; expansion is a separate decision.

## Cross-cutting differentiator work (DOC-24)

These proposed capabilities are integrated into the existing phases rather than treated as unreviewed scope expansion:

- **Discovery / requirements:** classify every differentiator as proposed, approved for V1, deferred or rejected; capture user value and evidence.
- **Architecture / security:** design tenant-safe integration boundaries, credential lifecycle, event/outbox reliability, auditability, recovery and provider capability mapping.
- **Transaction / ledger:** define unknown-outcome recovery, transaction timeline evidence, reconciliation queues and immutable correction history.
- **Provider integration:** add only verified provider capabilities; make routing/fallback conditional on safe status resolution and explicit policy.
- **API / developer experience:** provide sandbox/live separation, contract-driven docs, idempotency guidance, safe diagnostics and webhook testing/replay controls.
- **Dashboard / operations:** provide permission-aware reports, support diagnostics, status timelines, operational alerts and accessible responsive layouts.
- **Verification / launch:** test each approved feature's authorization, tenant isolation, replay/idempotency, failure recovery, audit trail and financial invariants.

DOC-24 is a proposal catalogue, not authorization to build every item. V1 prioritization must be resolved in DOC-00 and reflected in approved requirements before implementation. Advanced features such as multi-currency, cards, cross-border payments, scheduled payouts, split settlements, subscriptions and automated risk scoring remain conditional until provider, legal and operational feasibility is established.

## Cross-phase artefacts

Keep these current throughout:
- Requirement, decision and risk/blocker registers.
- Provider capability matrix and architecture decisions.
- Requirement-to-test traceability.
- Migration and release history.
- Security and operational evidence.
- Explicit separation of proposed, approved, implemented, tested, deployed and independently reviewed work.

## Current status

Discovery and documentation are in progress. This roadmap is provisional. Do not invent dates, cost estimates, provider commitments or production-readiness claims before evidence is available.
