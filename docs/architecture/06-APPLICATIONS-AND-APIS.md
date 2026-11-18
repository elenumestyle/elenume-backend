# 06 — Frontend Applications, BFFs, APIs and Authorization

**ID:** ARC-06 | **Status:** APPROVED | **Version:** 1.0.0 | **As of:** 2026-10-12

## Required independent browser surfaces

| Surface | Browser app | BFF | Authentication realm |
|---|---|---|---|
| Storefront | Next.js Storefront | ASP.NET Core Storefront BFF | Public |
| Seller Panel | Next.js Seller | ASP.NET Core Seller BFF | Public with Seller-context assurance |
| Admin Panel | Next.js Admin | ASP.NET Core Admin BFF | Workforce |

Same-origin rule is **per pair**: `elenume.com/*` and `elenume.com/api/*` share a Storefront origin; `seller.elenume.com/*` with `seller.elenume.com/api/*`; `admin.elenume.com/*` with `admin.elenume.com/api/*`. Routing via reverse proxy is permitted. Distinct subdomains are distinct browser origins; do not set a shared `.elenume.com` authentication cookie.

## BFF responsibilities

Each BFF independently owns frontend-facing endpoint composition, authenticated server-side sessions, OIDC client interaction, CSRF defense, server-side token handling, response shaping and the surface-specific OpenAPI contract. The Core API hosts actual module-owned business operations and independent authorization rules. **Do not duplicate Sellers/Catalog/Commerce domain logic in BFFs.**

Use separate frontend-generated TypeScript clients for Storefront, Seller and Admin; the internal Core API exposes a separate contract to trusted callers. REST + OpenAPI is the agreed starting style. Exact routes/DTOs/contract versioning rules require stage review; persistence entities are never API contracts.

## Trust and least privilege

- Each BFF must authenticate with the intended public or workforce realm and audience.
- Core API must validate caller/token attributes and resource access **again**, without trusting UI visibility, request-supplied UserId, SellerId ownership claims, or BFF location alone.
- Seller V1 management authorization uses `CanManageSeller`, with owner-only access; trading uses separate `CanTrade` commercial eligibility.
- Admin operation authorization requires a separate redesigned workforce permission system; **the MVP role list is NOT accepted**.
- Browser JavaScript must not receive access or refresh tokens; opaque secure host-only cookies reference revocable persistent server-side sessions.

## Frontend reuse and shared packages

Reuse visual style, user experience, assets, design primitives and accessibility work where appropriate. Review the old code before reusing Supabase-specific clients, authentication, SSR sessions, generated DB types, data actions or backend-coupled infrastructure. Shared packages must remain generic; do not share three app shells or business authorization logic by accident.

## No premature contracts

Don't generate a huge CRUD API, one global OpenAPI schema for all three public surfaces or speculative role-dependent routes. Build API methods for approved workflows only; verify compatibility with generated clients and security tests.
