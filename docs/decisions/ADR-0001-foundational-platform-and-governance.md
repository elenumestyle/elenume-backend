# ADR-0001 — Clean .NET Platform, Modular Monolith and Stage Governance

- **Status:** ACCEPTED
- **Decision IDs:** D-001, D-002, D-003, D-026, D-028
- **Recorded:** 2026-10-12 (consolidating the earlier approved architectural handoff)
- **Supersedes:** none
- **Canonical detail:** `../architecture/00-GOVERNANCE.md`, `01-SYSTEM-BASELINE.md`, `07-ENGINEERING-STANDARDS.md`

## Context

ELENUMÉ's existing Next.js/Supabase MVP is valuable evidence, but its schema/security/API choices should not determine a clean new system. Initial cost, security and maintainability matter; future partial service extraction is likely but not yet required.

## Decision

Use .NET 10 LTS, ASP.NET Core 10, EF Core 10, PostgreSQL 18 and Next.js. Build a modular monolith, one database, explicit domain contracts, REST/OpenAPI, S3-compatible storage abstraction, search abstraction, localization designed for future languages, and OpenTelemetry. Optimize for simplicity and extraction readiness rather than speculative infrastructure. Implementation happens in separately approved stages only.

## Consequences and trade-offs

Lower operating/coordination cost now; modules can later be extracted with explicit contract/data migration work, not with zero effort. Portability requires feature-level DB reviews. The current MVP remains a reference, not a rewrite blueprint. Approved architecture does not grant coding authority.

## Not authorized by this ADR

Microservices, Kubernetes, brokers, multiple physical databases, fully implemented search infrastructure or any particular implementation stage.
