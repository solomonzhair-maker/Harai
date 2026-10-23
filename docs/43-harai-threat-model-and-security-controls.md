# DOC-43 — HARAI Threat Model and Security Controls

**Status:** Draft threat model — independent security review required  
**Date:** 2026-10-19  
**Related:** DOC-31, DOC-34, DOC-40, DOC-41, DOC-42  
**Purpose:** Identify principal trust boundaries, misuse cases and minimum control evidence for a multi-tenant payment gateway. This is an initial design artifact, not a completed penetration test, certification or authorization to implement.

## 1. Scope and assumptions

In scope: merchant/admin web app, public API, tenant identity, API credentials, checkout/payment links, provider adapters, inbound provider callbacks, outbound merchant webhooks, money-out and beneficiary changes, ledger/reconciliation, background workers, support/admin access, logs, backup and deployment environments.

Assumptions to validate:
- multiple unrelated merchants share the platform;
- provider results can be delayed, duplicated, out of order or ambiguous;
- money-moving operations may be retried after process/network failure;
- support and operations personnel may have privileged access;
- API endpoints may be targeted by automated abuse;
- HARAI's exact funds-flow model, provider authorization, legal role and production topology remain undecided.

Out of scope for any claim of completeness: formal regulatory assessment, penetration test, code review, provider security certification and environment-specific configuration review. These are separate evidence requirements.

## 2. Assets and trust boundaries

| Asset / boundary | Why it matters | Minimum protection |
|---|---|---|
| Tenant records and merchant data | Cross-tenant disclosure or mutation can expose transactions and credentials | Server-side tenant derivation, object-level authorization, database constraints and negative tests |
| API credentials and sessions | Credential theft can permit unauthorized actions | Scoped/revocable credentials, secure one-time display, hashed or managed-secret storage, rotation and redaction |
| Payment intents and provider attempts | State tampering or duplicate submission can create false outcomes or duplicate money movement | Canonical transition service, idempotency, durable outbox and provider enquiry |
| Provider callback endpoint | Forged/replayed callbacks could mark false success or trigger duplicate effects | Provider-specific signature/authentication verification, replay/deduplication and durable receipt |
| Merchant webhook sender | SSRF or data leakage could reach internal services or leak transaction data | HTTPS policy, public-IP/DNS/redirect checks, egress restrictions, signed minimal payloads |
| Beneficiaries and payouts | Destination manipulation can divert money-out | Step-up, change audit, cooling-off/approval where required, pending-payout safeguards |
| Ledger and reconciliation evidence | Silent changes or duplicate postings can misstate financial records | Append-only postings, unique identities, compensating corrections, reconciliation and independent review |
| Support/admin tools | Privileged abuse can expose or alter many tenants | Least privilege, just-in-time access, reason codes, separation of duties, audit and alerts |
| Logs, traces and reports | Secrets/personal data can persist in observability and exports | Data minimization, structured redaction, access controls and retention policy |
| Build/deployment/secrets | Compromised dependency or environment may expose credentials or code | Environment separation, protected CI, dependency review, managed secrets, least-privilege deploy access |
| Backups and restore path | Backup theft or bad restore may expose data or duplicate jobs | Encryption/access controls, restore exercises, safe job pause/replay and documented RPO/RTO |

## 3. Threat register

Likelihood and impact are preliminary qualitative design judgments, not measured risk scores. They must be reviewed by an independent security reviewer and updated after the selected architecture and deployment topology are known.

| ID | Threat / misuse case | Impact | Proposed prevention/detection | Required validation evidence | Status |
|---|---|---|---|---|---|
| TH-01 | Tenant A changes an object ID to access Tenant B's transaction, key, webhook or report (IDOR/BOLA) | Cross-tenant data disclosure or unauthorized action | Derive tenant from authenticated membership/credential; scope every query/job/export; deny by default; DB constraints | Negative tests for read/write/export/background jobs and support views across tenants | Open |
| TH-02 | Stolen or over-scoped API key is used to create collections or money-out | Unauthorized financial activity/data exposure | Per-key scopes, rate limits, revocation, rotation, display-once handling, separate money-out permissions and alerts | Credential lifecycle and revoked/insufficient-scope tests; owner-approved limits | Open |
| TH-03 | Forged, replayed, duplicated or out-of-order provider callback marks false success | False paid state, duplicate ledger entries or reconciliation mismatch | Verify provider-specific auth/signature, timestamp/replay controls where supported, durable receipt, dedupe, state-machine mapping and enquiry | Invalid signature, replay, duplicate, reordered and conflicting-evidence tests per adapter | Open |
| TH-04 | Network timeout causes blind repeat of a request that provider already accepted | Duplicate collection/payout | Durable idempotency record, outbox, UNKNOWN state, provider enquiry and safe retry policy | Fault injection around pre-send, in-flight timeout, process crash and retry | Open |
| TH-05 | Race condition creates duplicate payment intents, payouts, refunds or postings | Financial loss/inconsistent ledger | Database unique constraints, transaction boundaries, atomic amount reservations, operation locking/versioning | Concurrent same-key, conflicting-key, refund-limit and worker-redelivery tests | Open |
| TH-06 | Attacker supplies forged forwarded headers or exploits IP allowlist recovery | Bypass network restrictions or lock out merchant | Trust only configured proxies; derive client IP at trusted edge; authenticated/audited allowlist changes and recovery | Spoofed X-Forwarded-For, untrusted proxy, IPv4/IPv6 and lockout-recovery tests | Open |
| TH-07 | Merchant configures webhook URL to cloud metadata, localhost, private network or a redirect to an internal host (SSRF) | Internal data/service exposure | HTTPS, reject private/link-local/reserved destinations, DNS rebinding and redirect revalidation, egress policy | DNS/redirect/IP-rebinding test matrix; egress logs and deny evidence | Open |
| TH-08 | Merchant webhook signing secret is exposed or old signature is replayed | Forged merchant events or data leakage | Versioned signature, timestamp/nonce where feasible, display-once secret, rotation overlap policy, minimal payload | Signature verification, replay, rotation and secret-redaction tests | Open |
| TH-09 | Beneficiary is changed before a payout or an insider changes destination | Money-out diverted | Step-up authentication, out-of-band notification, audit, cooling-off/approval policy, recheck destination at payout execution | Unauthorized/stale change and pending-payout race tests | Open |
| TH-10 | Support user abuses cross-tenant access or manually changes transaction state | Unauthorized disclosure or financial manipulation | Just-in-time role, ticket/reason, narrow scope, dual control for consequential correction, immutable audit | Privilege review, access-expiry, approval separation and audit tests | Open |
| TH-11 | Reconciliation import or manual adjustment is replayed/altered | Duplicate or false postings | Source hashes, unique import identity, append-only postings, compensating entries, correction case | Re-import, changed-content-under-same-ID, duplicate posting and correction tests | Open |
| TH-12 | Secrets or personal data leak into logs, traces, errors, screenshots, fixtures or offline cache | Account compromise/privacy harm | Allowlist logging, redaction, data classification, retention, no private evidence/live secrets in offline cache | Automated secret scans, sample-log review, offline-cache inspection | Open |
| TH-13 | API abuse exhausts resources or triggers provider polling/costs | Availability degradation/provider suspension | Per-tenant/key limits, bounded polling/backoff, concurrency caps, quotas and alerting | Threshold/reset/concurrency and load-abuse tests; provider quota confirmation | Open |
| TH-14 | CSRF/CORS/session weaknesses enable unauthorized browser actions | Account takeover or unauthorized sensitive action | Explicit origin policy, CSRF protection for cookie sessions, secure cookie/session settings and re-authentication | Origin/CSRF/session expiry and privilege-change tests | Open |
| TH-15 | Worker/job replay after restore or queue outage repeats a money-moving side effect | Duplicate operation or inconsistent state | Durable job identity, idempotent handlers, outbox/inbox, replay controls and reconciliation after restore | Kill/restart, duplicate delivery, restore-and-replay tests | Open |
| TH-16 | Backup compromise or incomplete restore exposes data or leaves financial state inconsistent | Data loss, disclosure or duplicate work | Encrypted backups, least privilege, restore tests, post-restore consistency checks, jobs paused until validated | Documented restore exercise, access evidence and reconciliation checks | Open |
| TH-17 | Dependency, CI credential or deploy identity is compromised | Malicious code/configuration reaches service | Least-privilege CI, protected branches, dependency review, secret scanning, reviewed deploy permissions | CI permission audit, dependency/SBOM evidence as appropriate, secret scan and rollback exercise | Open |
| TH-18 | Merchant receives or exposes another merchant's webhook or report through misrouted event | Cross-tenant disclosure and operational confusion | Tenant-bound event subscription, endpoint ownership validation, minimal payload, per-tenant delivery log | Cross-tenant subscription and endpoint-change tests | Open |
| TH-19 | Ambiguous/contradictory provider evidence is manually resolved without authority | Incorrect transaction/ledger state | Evidence-based discrepancy case, documented resolution rule, separation of initiator/approver where required | Case workflow, evidence retention and independent correction review | Open |
| TH-20 | Card data is accidentally captured in HARAI forms/logs | Sensitive payment data exposure and expanded security scope | Prefer eligible processor-hosted/tokenized components; explicit data-flow review; do not store CVV or raw card data absent approved architecture and applicable requirements | Processor architecture, data-flow diagram, security-scope assessment and negative logging tests | Open |

## 4. Minimum security design requirements

- **Authentication and authorization:** secure session lifecycle, MFA/step-up policy for privileged or high-risk actions, tenant-scoped authorization, revocation, least privilege and separation of duties.
- **API edge:** schema validation, abuse limits, stable errors, correlation IDs, explicit CORS/CSRF policy, trusted-proxy rules and safe IP-allowlist recovery.
- **Financial state:** idempotency, atomic constraints, unknown outcomes, append-only ledger, safe corrections and reconciliation.
- **Provider integration:** adapter-specific auth/signature, secret rotation, status enquiry, no unsafe retries and capability flags that default off.
- **Outbound webhooks:** SSRF-resistant destination checks, signing, dedupe, bounded retries, secret rotation and data minimization.
- **Payout safety:** eligibility, funding, limits, beneficiary verification, re-authentication, approvals and pending-operation rechecks.
- **Privacy:** purpose limitation, minimum data, approved retention/deletion, restricted evidence, safe logs/exports and incident response.
- **Operations:** protected deployment, secret management, backups/restore, monitoring, response ownership and documented incident handling.

## 5. Threat-model validation plan

Before G2 approval:
1. Name an independent security reviewer and record their scope.
2. Review data-flow diagrams and trust boundaries for selected architecture.
3. Threat-model the actual provider and funds-flow arrangements, not just the generic API.
4. Convert TH-01–TH-20 into risk-linked test cases and assign real test artefacts after implementation authorization.
5. Define severity, risk acceptance authority, remediation deadlines and exception expiry.
6. Perform independent design review; later, authorized security testing and penetration testing must produce their own evidence.
7. Confirm no production enablement until critical/high risks are resolved or explicitly accepted by the proper accountable authority, and all external permissions are verified.

## 6. Current disposition

This initial threat model identifies principal risks and proposed controls. It is not exhaustive and does not claim code review, test execution, penetration testing, certification, legal approval or G2 passage. Independent review and implementation evidence remain outstanding.


## 7. Tenant-isolation abuse cases — review extension (2026-10-23)

The existing TH-01, TH-10, TH-15 and TH-18 risks overlap but test different trust boundaries. Their future validation should explicitly include:

- **Forged context:** caller changes tenant/project/environment IDs in body, path, query, headers or idempotency metadata while presenting a valid credential for another tenant.
- **Async context drift:** a job is queued under one tenant and later altered, replayed, run after membership/key revocation, or points to a child record owned by another tenant.
- **Callback misbinding:** callback contains a forged tenant/reference, collides with another tenant's provider reference, or cannot be uniquely matched to a previously persisted provider attempt.
- **Derived-data leakage:** tenant/environment omitted from cache keys, search indexes, report filters, export artifacts, object-storage policy or download authorization.
- **Privileged access misuse:** support agent can browse arbitrary tenants without a case/reason, continues after access expiry, or can change a financial state outside the canonical correction path.
- **Restore/replay:** recovered queues, outbox rows, caches or export links revive stale authorization or duplicate an operation.

Required future evidence is a threat-linked negative-test matrix, actual execution artifacts for each applicable path, access-policy/configuration review for the selected storage and deployment architecture, and disposition of all findings by the accountable authority. This extension is documentation-only; no tests were executed and no control is marked verified.
