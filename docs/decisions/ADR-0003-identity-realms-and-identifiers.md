# ADR-0003 — Separate Identity Host, Public/Workforce Realms and Stable Business IDs

- **Status:** ACCEPTED
- **Decision IDs:** D-010, D-011, D-012, D-013, D-022
- **Recorded:** 2026-10-12 (original handoff plus explicitly accepted Identity refinements)
- **Supersedes:** none
- **Canonical detail:** `../architecture/03-IDENTITY-AND-SECURITY.md`, `04-ACCOUNTS-AND-SELLERS.md`

## Context

User identity must survive login identifier changes; Seller is not a login role; public customers/sellers cannot inherit staff privileges. An Identity vendor schema should not become the domain model.

## Decision

Separate ASP.NET Core Identity/OpenIddict host with strongly separated public and workforce realms. Distinguish `IdentityAccountId` from the permanent business `UserId`, using a unique mapping; workforce has distinct authentication identity. Identity owns verified login mobile numbers. Admin authorization is a separate future redesign, not a copy of MVP roles.

## Consequences and trade-offs

Public and workforce policy isolation in one host keeps initial operations economical, but the host retains a shared operational blast radius. Extra identity-to-business mapping requires reliable account provisioning. Splitting workforce deployment later remains possible.

## Not authorized by this ADR

Exact identity table schema, physically separate workforce Identity host, Admin permission model, external identity providers or passkeys.
