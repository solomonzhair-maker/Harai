# HARAI Architecture

## High-level flow

Application → HARAI API → Provider Adapter → Payment Provider

## Core principles

1. Multi-tenant from the beginning.
2. Provider-independent payment domain.
3. Idempotent payment operations.
4. Immutable financial records.
5. Webhook delivery with retries.
6. Secrets never stored in source control.
7. Every production integration is observable and auditable.

## Planned domains

- Tenants / merchants
- Applications
- API credentials
- Customers
- Payment intents
- Transactions
- Payment providers
- Webhooks
- Refunds
- Payouts
- Ledger
- Reconciliation
- Audit logs
