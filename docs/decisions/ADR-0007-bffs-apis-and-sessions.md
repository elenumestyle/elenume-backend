# ADR-0007 — Three Isolated BFFs, Same-Origin Facades and Persistent OIDC Sessions

- **Status:** ACCEPTED
- **Decision IDs:** D-007, D-008, D-009, D-018
- **Recorded:** 2026-10-12 (handoff, seven-item structure review and five-item Identity review)
- **Supersedes:** none
- **Canonical detail:** `../architecture/06-APPLICATIONS-AND-APIS.md`, `03-IDENTITY-AND-SECURITY.md`

## Context

One frontend/BFF with role-dependent modes would create unnecessary coupling and shared security risk. Browser-managed OAuth tokens increase exposure, and the Core API should remain free to evolve internally.

## Decision

Three independent Next.js applications with respective ASP.NET Core BFFs (Storefront, Seller, Admin), each same-origin for its browser routes and BFF endpoints. Separate secure host-only browser sessions stored persistently server-side. Use OIDC Authorization Code + PKCE; keep OAuth tokens out of browser JavaScript. BFF-facing APIs use separate REST/OpenAPI/TypeScript client contracts, backed by a separately contracted Core API; business rules/authorization remain with modules.

## Consequences and trade-offs

More hosts and surface contracts than a single all-in-one app, but clearer trust boundaries and independent deployment. Persistent session storage and CSRF protection are necessary. Storage engine, exact auth cookie options, OpenIddict validation settings and routes are later design choices.

## Not authorized by this ADR

Shared `.elenume.com` login cookie, universal BFF, embedded business module logic in BFFs, Redis as mandatory session store, or a particular frontend implementation stage.
