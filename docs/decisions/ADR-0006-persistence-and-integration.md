# ADR-0006 — Module-Owned EF Core Persistence and Explicit Module Contracts

- **Status:** ACCEPTED
- **Decision IDs:** D-023, D-024, D-025
- **Recorded:** 2026-10-12 (handoff plus approved backend module/database review)
- **Supersedes:** none
- **Canonical detail:** `../architecture/05-PERSISTENCE-AND-INTEGRATION.md`, `02-MODULE-BOUNDARIES.md`

## Context

Shared databases ease startup but arbitrary cross-module joins, entity navigations and transactions would frustrate service extraction. PostgreSQL features are useful but must not dictate application authorization.

## Decision

Start with one PostgreSQL database, per-module ownership of tables/schema and EF Core contexts. Integrate through explicit application contracts or reviewed events. Review cross-module FKs and transactions individually; do not directly consume another module's private EF/database representation. Keep domain/business rules out of PostgreSQL-specific workflows/RLS where reasonable; review all database feature decisions for portability.

## Consequences and trade-offs

Local relational integrity remains strong; some cross-module associations need application validation/reconciliation. Reliable events and distributed consistency are introduced when actual workflows justify them, not as premature infrastructure. Controlled production migrations are expected.

## Not authorized by this ADR

Specific SQL schema names, PostgreSQL extensions, physical separate databases, broker selection, mandatory outbox in every module, or precise stock/payment consistency protocol.
