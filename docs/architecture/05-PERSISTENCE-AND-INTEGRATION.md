# 05 — Persistence, Database Portability and Module Integration

**ID:** ARC-05 | **Status:** APPROVED | **Version:** 1.0.0 | **As of:** 2026-10-12

## Database topology and ownership

Initially **one PostgreSQL 18 database**. Modules own their schemas and persistence via module-specific EF Core 10 DbContexts. Identity maintains separate public/workforce security realms with separately owned persistence; exact schema/context split must be reviewed in the Identity stage.

Candidate module schemas are `accounts`, `sellers`, `catalog`, `offers`, `commerce`, `payments`, `reviews`, `collections`, and a `discovery` schema **only if needed**. Candidate identity schemas are `identity_public`, `identity_workforce`, `identity_oidc`. These are **illustrative names**, not preapproved migrations or a mandate to create empty schemas/contexts.

## Constraints

- Each persisted record/table has one accountable owner.
- EF Core is the default persistence approach; do not expose EF entities directly through APIs, module contracts or unrestricted cross-module repositories.
- No cross-module EF navigation by default. Cross-module physical foreign keys and transactions require **specific architectural review**.
- A module normally owns its migrations. Exact tooling, history schema and execution mechanism will be approved at the relevant stage.
- Production migrations should be controlled through deployment procedures, not run blindly from each application startup.
- Referential integrity within a module (PK/FK/UNIQUE/CHECK) is normal, after review.
- Cross-module logical references require validation and a failure/reconciliation policy; rejecting a cross-module FK does not eliminate the need to handle stale/orphaned references.

## Feature-by-feature database review

**Ordinary tools, still reviewed:** relational tables, PK/FK, unique/check constraints, conventional indexes/transactions/types, UUIDs.

**Requires explicit portability assessment:** JSON/JSONB, views, PostgreSQL enums/arrays, partial indexes, generated columns, materialized views, native full-text search and provider-specific indexes.

**Avoid as defining architecture:** triggers/stored procedures that implement domain workflows, PL/pgSQL application logic, PostgreSQL RLS as primary business authorization and extension-dependent business behavior. Use an otherwise provider-specific feature only if documented benefits outweigh lock-in.

Business authorization belongs in application policies, not database-specific RLS. Infrastructure may use targeted SQL for measured performance without leaking SQL/vendor assumptions into the domain layer.

## Synchronous integration / events

- Prefer explicit in-process application contract calls initially. No network call required between modules in the same host.
- Distinguish *domain events* (inside domain/module) from *integration events* (published cross-boundary facts).
- In-process events are adequate for noncritical reactions. **Do not rely on fire-and-forget volatile events for critical order, payment or inventory consistency.**
- Use a durable outbox/inbox or equivalent once a workflow requires reliable delivery/retries; exact technology and scope remain stage decisions.
- Design retriable operations to be idempotent; specify data ownership, ordering, concurrency and consistency before implementing multi-module transactions.

## Commerce-specific unresolved safety boundaries

The approved modules are Commerce (cart, checkout, orders), Offers (simple stock) and Payments (gateway outcomes). **Not yet approved:** stock-reservation algorithm, transaction/compensation protocol, payment callback semantics, order finalization, shipment fulfillment or seller settlement. Preserve explicit boundaries; do not invent a distributed saga framework or assume a monolithic shared transaction solves everything.

## Search, localization, object storage

- Search via application abstraction; PostgreSQL FTS may be used if justified, but domain must not depend permanently on its semantics.
- Localization supports Persian, English and future languages. Do not blindly create `NameFa/NameEn/...` on every model; translations must be reviewed by domain.
- S3-compatible object storage abstraction; database stores asset keys/metadata/reference/lifecycle when needed. Do not store large binary assets in PostgreSQL by default.
