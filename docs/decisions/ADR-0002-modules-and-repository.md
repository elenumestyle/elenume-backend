# ADR-0002 — Monorepo, Lightweight Module Assemblies and Nine Domain Boundaries

- **Status:** ACCEPTED
- **Decision IDs:** D-004, D-005, D-006
- **Recorded:** 2026-10-12 (prior explicit seven-item architecture review)
- **Supersedes:** none
- **Canonical detail:** `../architecture/02-MODULE-BOUNDARIES.md`, `01-SYSTEM-BASELINE.md`

## Context

One giant backend assembly weakens ownership while 4–5 class libraries per domain create needless up-front overhead. The project needs a manageable place for Next.js, BFFs, Identity, tests and API contracts.

## Decision

Maintain one monorepo. Define nine *logical* domains: Accounts, Sellers, Catalog, Offers, Commerce, Payments, Reviews, Collections, Discovery. Start active modules as one implementation assembly each, plus published contracts assembly only when justified. Commerce initially contains cart/checkout/orders; simple stock may remain in Offers. Identity remains a separate trust boundary and host.

## Consequences and trade-offs

Fewer projects and faster setup, offset by architecture tests, explicit interfaces and disciplined ownership. As domains expand, split assemblies or services when warranted. The nine boundaries do not mandate creating nine empty projects/schemas on day one.

## Not authorized by this ADR

Exact folder paths, all module implementations, separate Inventory/Reporting/Fulfillment modules, or creating unapproved commerce features.
