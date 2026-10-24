# HARAI Payment Gateway

HARAI is being designed as a reusable, multi-tenant payment gateway for HARAI products and, subject to verified permissions and approved operating arrangements, future external merchants.

**Current repository status:** discovery and architecture documentation plus an initial, incomplete workspace scaffold. This repository is **not yet a working payment gateway**. No production-ready payment processing, provider integration, automated test suite, CI pipeline, database migrations, or deployment has been verified.

## Vision

HARAI aims to provide one payment integration for applications such as Shigona, Herupu, and future systems.

```text
Application
    ↓
HARAI Payment Gateway
    ↓
Provider Adapters
    ↓
Mobile Money / Cards / Banks
```

This diagram expresses the intended product direction; it does not mean any provider capability, contract, production entitlement, funds-flow model, or live payment operation has been approved.

## Proposed architecture — not yet approved or implemented

The following is a working proposal subject to the documented requirements and architecture gates:

- API: NestJS + TypeScript
- Database: PostgreSQL + Prisma
- Cache/queues: Redis, with financial state and durable work remaining authoritative outside cache
- Web applications: Next.js
- Package management: pnpm workspaces
- CI/CD: GitHub Actions

The `feat/initial-foundation` branch currently contains root workspace configuration and placeholder README files; it does not contain a functioning application implementation. The `.env.example` template is present on that foundation branch only and is intended for local development. Do not reuse sample values in shared, staging, or production environments. Never commit real credentials, production secrets, or personal/financial data.

## Working branch and pull requests

The active branch for discovery and remediation is `docs/julypay-discovery-and-requirements`. PR #1 proposes integrating the initial scaffold into that branch and remains draft/unmerged pending review. PR #2 proposes the discovery documentation changes to `main` and remains draft/unmerged. `main` is not being updated as part of the current discovery/remediation work.

## Required gates

- **G1 — Requirements approval:** NOT PASSED.
- **G2 — Architecture/security approval:** NOT APPROVED.
- **Implementation authorization:** not granted by the existence of the scaffold or this README.

Do not implement provider integrations, enable money movement, contact providers, create/use production credentials, run live financial operations, merge into `main`, or deploy until the applicable decisions, evidence, reviews, and explicit authorizations are recorded.

## Development status

A proposed stack and placeholder files are not evidence that build, lint, typecheck, test, migration, or deployment commands work. Those checks must be implemented and actually run in an authorized engineering phase before being described as passing.

## Source-of-truth documents

- [Master index and decision register](docs/00-master-index-and-decision-register.md)
- [End-to-end implementation roadmap](docs/03-end-to-end-implementation-roadmap.md)
- [Repository-wide remediation and verification plan (DOC-62)](docs/62-repository-remediation-and-verification-plan.md)

Follow DOC-62 in order. Record unresolved decisions and blockers rather than assuming they are approved.