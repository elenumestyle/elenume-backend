# 01 — Approved System Baseline

**ID:** ARC-01 | **Status:** APPROVED | **Version:** 1.1.0 | **As of:** 2026-11-18

## Technology baseline

- Runtime/backend: **.NET 10 LTS, ASP.NET Core 10, C#**.
- Persistence: **EF Core 10; PostgreSQL 18** initially.
- Frontend: **Next.js**; reuse the MVP's UI/design/assets when suitable, but evaluate backend coupling before reusing code.
- Authentication stack: **ASP.NET Core Identity + OpenIddict** in a separate Identity host.
- Browser integration: three separate BFFs with same-origin browser endpoints and server-side sessions.
- Public APIs: REST + OpenAPI; generated TypeScript clients where appropriate.
- Storage: S3-compatible object storage through a domain-independent abstraction.
- Telemetry: OpenTelemetry abstractions, structured logs, metrics, traces, health checks and correlation IDs.

## Target deployable boundaries

```text
Browser ── elenume.com          ── Next.js Storefront ── Storefront BFF ──┐
Browser ── seller.elenume.com   ── Next.js Seller     ── Seller BFF     ──┼── Core API (modular monolith)
Browser ── admin.elenume.com    ── Next.js Admin      ── Admin BFF      ──┘
                                            │
                               Elenume.Identity (OIDC/credentials)
                                            │
                             PostgreSQL (initially one database)
```

These are illustrative domain names from the approved handoff, not purchased-domain/deployment validation. All BFFs consult Identity for appropriate authentication; the Core API is not publicly exposed as an unrestricted browser CRUD service. Each frontend and BFF is independently buildable/deployable. Independent applications may initially share economical physical infrastructure.

## Core backend architecture

A **modular monolith** is required initially, with low-cost future module extraction. Logical modules have owned persistence and explicit public contracts; one Core API host composes them. No initial requirement for microservices, distributed databases, Kubernetes, Kafka, RabbitMQ, Redis, service mesh or event sourcing.

Do not optimize for hypothetical peak scale at the cost of straightforward V1 engineering. Microservices are a possible later evolution, not current permission to create them.

## Frontend independence

The Storefront, Seller Panel and Admin Panel MUST be separate Next.js applications, **not three modes in one app**. May share design tokens, reusable UI primitives, fonts, localization, icons and generated contracts. MUST NOT share by default app route tree, shells, browser sessions, security policies, BFFs, runtime config, deployment units or surface-specific business logic.

## Scope of original MVP reuse

MVP domain knowledge, behavioral tests and visual designs can inform requirements. Supabase Auth/Data API, RLS-centric authorization, schema, server actions and generated database types are **not** to be copied automatically. The new system must stand on its own business-domain requirements.

## Solution/repository layout

**Approved policy:** one monorepo; lightweight .NET module assemblies, separate contracts only when genuinely consumed; distinct hosts and independent frontend apps. **Illustrative, not frozen names or authorization to create all directories:** `backend/src/Hosts`, `backend/src/Identity`, `backend/src/Modules`, `frontend/apps/{storefront,seller,admin}`, `frontend/packages`, `docs`, `deploy`, `scripts`, `backend/tests`.

Exact `.slnx` name, folder names, packages and which projects are created in Stage 1 must be reviewed in its stage proposal. Nine defined logical modules do **not** mean nine prebuilt code assemblies at startup.

## Repository operation policy (D-029)

The new .NET source lives in a human-selected GitHub account/repository independent of the legacy MVP. **The human handles all Git and GitHub operations**; AI agents may only modify approved local project files and run authorized local validation. No automated repo creation, commit, push or PR creation. See `11-SOURCE-CONTROL-OPERATIONS.md`.
