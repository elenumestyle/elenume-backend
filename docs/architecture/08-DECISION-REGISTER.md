# 08 — Approved Decision Register

**ID:** ARC-08 | **Status:** APPROVED record | **Version:** 1.1.0 | **As of:** 2026-11-18  
**Source:** Original architecture handoff and subsequent explicit user approvals of seven structural recommendations and five Identity/Accounts/Sellers refinements. ADRs record consolidated approvals, not verbatim meeting transcripts.

The IDs below are stable references for agent prompts and implementation-stage proposals. A proposed change is not an approved decision until explicitly accepted and recorded.

| Decision ID | Status | Binding decision | ADR |
|---|---|---|---|
| D-001 | APPROVED | New clean system, not direct Supabase/MVP port | ADR-0001 |
| D-002 | APPROVED | .NET 10, ASP.NET Core 10, C#, EF Core 10, PostgreSQL 18, Next.js | ADR-0001 |
| D-003 | APPROVED | Modular monolith initially; future extractability without speculative distribution | ADR-0001 |
| D-004 | APPROVED | One monorepo; lightweight module implementation assemblies/contracts on demand | ADR-0002 |
| D-005 | APPROVED | Nine logical modules: Accounts, Sellers, Catalog, Offers, Commerce, Payments, Reviews, Collections, Discovery | ADR-0002 |
| D-006 | APPROVED | Commerce includes cart/checkout/orders; simple inventory may initially be in Offers | ADR-0002 |
| D-007 | APPROVED | Storefront, Seller and Admin are independent Next.js apps each with its own ASP.NET Core BFF | ADR-0007 |
| D-008 | APPROVED | Per-surface same-origin and independent server-side browser sessions | ADR-0007 |
| D-009 | APPROVED | Internal Core API + three frontend-facing BFF/OpenAPI contracts; REST/OpenAPI/TS clients | ADR-0007 |
| D-010 | APPROVED | Separate Identity host: ASP.NET Core Identity + OpenIddict; two strongly separated realms | ADR-0003 |
| D-011 | APPROVED | Public permanent UserId independent of username/email/mobile/auth account/Seller | ADR-0003 |
| D-012 | APPROVED | `IdentityAccountId` distinct from `UserId`, uniquely mapped; workforce identity distinct | ADR-0003 |
| D-013 | APPROVED | Identity authoritative for verified login mobile | ADR-0003 |
| D-014 | APPROVED | Mobile-only verified registration; username optional; public username/password or mobile/OTP login | ADR-0004 |
| D-015 | APPROVED | Identity-coordinated, recoverable, idempotent User provisioning | ADR-0004 |
| D-016 | APPROVED | Public passwords >=8 no mandatory composition; Admin >=8 plus four classes and SMS MFA | ADR-0004 |
| D-017 | APPROVED | Seller optional stronger username/password/SMS-OTP authentication in Seller context | ADR-0004 |
| D-018 | APPROVED | OIDC Authorization Code + PKCE and persistent BFF-side sessions | ADR-0007 |
| D-019 | APPROVED | Public User may also own Seller; at most one Seller per User in V1; Seller has independent SellerId | ADR-0005 |
| D-020 | APPROVED | Seller approval before trading; Seller lifecycle separate from User; future ownership transfer deferred | ADR-0005 |
| D-021 | APPROVED | Seller V1 owner-only full access through CanManageSeller abstraction; CanTrade is separate | ADR-0005 |
| D-022 | APPROVED | Admin authorization hierarchy must be redesigned, not copied from MVP | ADR-0003 |
| D-023 | APPROVED | One PostgreSQL DB, module-owned schemas/DbContexts; review cross-module constraints individually | ADR-0006 |
| D-024 | APPROVED | Portability review for DB features; avoid RLS as default business authorization | ADR-0006 |
| D-025 | APPROVED | Published inter-module contracts; no arbitrary reads of another module's tables/EF types | ADR-0006 |
| D-026 | APPROVED | S3-compatible storage abstraction; localized content strategy; search abstraction; OpenTelemetry | ADR-0001 |
| D-027 | APPROVED | No speculative Organizations, seller staff, passkeys, social login, microservices, infrastructure | ADR-0005 |
| D-028 | APPROVED | Every implementation stage requires explicit scope/technical/security/schema/API/test review and separate user approval | ADR-0001 |
| D-029 | APPROVED | All Git/GitHub operations and repository administration are human-controlled; AI agents only edit approved local files and run authorized local validation, with no Git commands | ADR-0008 |

## Approval provenance and chronology

- **Original handoff (prior agreed decisions):** all D-001 through D-003, D-007 through D-011, D-014, D-016, D-017, D-019, D-020, D-022, D-024, D-026, D-028, and foundational aspects of remaining decisions. See `docs/history/ORIGINAL-ARCHITECTURE-HANDOFF.md`.
- **Backend module-boundary review (accepted by user before 2026-10-12):** monorepo, packaging, nine-module inventory, Identity deployment, Commerce/Inventory granularity, module database ownership, BFF/internal contracts (D-004 through D-006 and confirmations of D-007/D-009/D-010/D-023/D-025).
- **Identity–Accounts–Sellers review (explicitly accepted 2026-10-12):** distinct IdentityAccountId/UserId, Identity-coordinated recoverable registration, OIDC code/PKCE + persistent BFF sessions, CanManageSeller vs CanTrade, verified login mobile owned by Identity (D-012, D-013, D-015, D-018 and D-021 refinement).

- **Human-controlled Git workflow (explicitly accepted 2026-11-18):** user accepted the proposed split under which ChatGPT/Codex generate/modify local files and the user handles Git/GitHub operations personally (D-029, ADR-0008).

**No application implementation stage has been approved as of this release.** The Stage 1 proposal remains awaiting a separate explicit authorization.

## Updating this register

Add new stable `D-###` IDs rather than reusing old ones. New records must link an approved ADR, affected canonical specs, the explicit approval event/date and any superseded decisions. Current/proposed/open states must be kept distinct. See `00-GOVERNANCE.md`.
