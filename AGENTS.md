# ELENUMÉ .NET — Mandatory AI Agent Instructions

**Status:** APPROVED governance summary, reference pack v1.1.0 (2026-11-18).  
**Read order:** `README.md` → `docs/architecture/00-GOVERNANCE.md` → `docs/architecture/08-DECISION-REGISTER.md` → domain documents applicable to task.

## Non-negotiable operating rules

1. This is a **new .NET ELENUMÉ system**, not an automatic translation of the existing Supabase MVP. Existing code is reference evidence, not architectural authority.
2. **Never implement a stage without explicit user approval of that stage.** First prepare a scope review with inclusions, exclusions, technical approach, schema, APIs, security, decisions, dependencies, tests, deferred features and approval requests. Architecture acceptance does not count.
3. **Do not expand scope silently.** Missing requirements or inconsistencies are proposals/questions, not permission to make design decisions or implement features.
4. Treat `docs/architecture/` and APPROVED ADRs as normative. If they conflict, STOP affected work, present the conflict and ask for a decision; do not silently prefer whichever text seems newer.
5. Preserve three **independent** Next.js application/BFF pairs: Storefront, Seller Panel, Admin Panel. Never share their browser sessions, use a common BFF, or turn them into modes of one frontend without a new approval.
6. Use .NET 10 / ASP.NET Core 10, EF Core 10, PostgreSQL 18, Next.js, ASP.NET Core Identity and OpenIddict. Start as a **modular monolith**, one PostgreSQL database, module-owned persistence, no speculative microservices.
7. Identity is a separate host with two strongly separated security realms: public users and workforce. Permanent `UserId` is not username, email, phone, Identity-account ID, or `SellerId`.
8. A public User can be customer and Seller owner; V1 at most one Seller/User; Seller is an independent commercial entity, approved before trading. Seller management and trading eligibility are separate checks.
9. Initial module inventory is **Accounts, Sellers, Catalog, Offers, Commerce, Payments, Reviews, Collections, Discovery**. Inventory may stay inside Offers initially; do not create empty modules for hypothetical work.
10. Browser sessions must be server-side, separately stored per BFF with secure host-only HttpOnly cookies. Browser JavaScript must not manage OAuth access/refresh tokens. BFFs use OIDC authorization code + PKCE. Protect against CSRF and validate access at the Core API.
11. Public users can register with **verified mobile only**, can later set username, and authenticate via username/password or mobile/SMS OTP. Public passwords: minimum 8 characters, no mandatory composition. Workforce Admin passwords: minimum 8 plus upper/lower/digit/symbol and mandatory SMS MFA. Seller stronger login is optional seller-context username/password/SMS OTP.
12. Do not copy MVP Admin authorization. Admin permissions redesign, Organizations, seller staff, passkeys, social login, ownership transfer and microservices are deferred.
13. Cross-module calls use published contracts; not another module's entities, DbContext, arbitrary tables or an unrestricted generic repository. Database-specific logic stays in infrastructure and is reviewed for portability.
14. No raw OTP, credential, token or secret in logs, test fixtures shared publicly, generated docs, or prompts. Never assume that a UI control supplies authorization.
15. **D-029: All Git and GitHub operations are human-only.** AI agents MUST NOT run *any* Git command (including `git status`/`git diff`), clone/fetch/pull/push, stage/commit, manage branches, tags, PRs, remotes, credentials, repository settings, CI runs, or deployments. Do not use GitHub APIs/connectors for equivalent actions. Work only on approved local files and tests; provide suggested commands/messages for the human if useful. See `docs/architecture/11-SOURCE-CONTROL-OPERATIONS.md`.

## Workflow for every task

- **Before:** identify referenced APPROVED decision IDs; classify work as documentation/design vs approved implementation; enumerate open dependencies and proposed changes.
- **During:** do not execute Git/GitHub operations; modify only in-scope local files; enforce ownership/security boundaries; select risk-relevant tests; record justified trade-offs.
- **After:** report files changed without Git commands, affected decisions, tests performed/results, any new decision needed, and whether Project/reference documents need synchronization.
- If code appears to contradict an approved rule, **report conflict** and request an explicit architecture decision before altering that rule or implementing the conflicting design.

## Source of truth / changes

Update accepted decisions through `docs/ai/ARCHITECTURE-CHANGE-WORKFLOW.md`. AI agents can draft changes but **cannot approve them**. On approved changes, update the relevant specs, decision register, ADRs, open questions, root instructions and ChatGPT Project snapshot **in one coordinated change**. Never retroactively rewrite accepted ADRs; supersede them explicitly.

**This file does not authorize coding or repository operations.** Stage 1 implementation is not approved.
