# Suggested ChatGPT Project Instructions (copy text below)

> Project: **ELENUMÉ .NET — new clean system**. The uploaded ELENUMÉ Architecture Reference Pack is the approved architecture source. Read `README.md`, `00-GOVERNANCE.md`, `08-DECISION-REGISTER.md` and affected domain specs before making architectural or coding recommendations. If uploaded files disagree, disclose the conflict and ask for an explicit decision; never invent approvals. Do not treat the original Supabase MVP schema/authorization as a design mandate.
>
> Technology: .NET 10 / ASP.NET Core 10 / EF Core 10 / PostgreSQL 18 / Next.js; modular monolith initially; one database and module-owned schemas/DbContexts; one monorepo. Nine logical modules: Accounts, Sellers, Catalog, Offers, Commerce, Payments, Reviews, Collections, Discovery. Separate Identity host (ASP.NET Core Identity + OpenIddict) with public/workforce realms. Three independent Next.js + ASP.NET Core BFF applications: Storefront, Seller, Admin. Each has its own same-origin facade and secure persistent server-side browser session; OIDC code+PKCE, no OAuth tokens in browser JS.
>
> Public UserId is permanent and distinct from credentials, IdentityAccountId and SellerId. Public registration supports verified-mobile-only; username/password or mobile/SMS OTP login. Identity coordinates idempotent, recoverable provisioning with Accounts and owns trusted login mobile numbers. Public passwords minimum 8 without mandatory composition; Admin minimum 8 with upper/lower/digit/symbol and mandatory SMS MFA. Seller is independent commercial entity, max one per User in V1, requires approval before trading. Seller Panel V1 management is owner-only using `CanManageSeller`; `CanTrade` is separate. Optional stronger Seller username/password/SMS OTP applies in Seller context.
>
> Do not carry over MVP Admin roles. Do not build unapproved Organizations, seller staff, social login, passkeys, ownership transfer, new microservices, Kafka/RabbitMQ/Kubernetes or other speculative features. Check `09-OPEN-QUESTIONS.md` for unresolved policies.
>
> **Repository control (D-029):** The human owner exclusively performs **all Git/GitHub operations**, including read-only Git commands, repository creation, clone/fetch/pull/push, branches, staging/commits, PRs and merges, settings, and releases. ChatGPT/Codex MUST NOT execute Git commands or GitHub connector/API/CLI actions. Agents may only modify stage-approved local files and run permitted local builds/tests; they may draft suggested commands/PR text for the human. Stage 1 remains unapproved.
>
> **Critical: Architecture approval is NOT authorization to implement.** Before every implementation stage, present scope, exclusions, technical approach, DB/schema, API, security/privacy, decisions, dependencies, tests, deferred capabilities and approval items, then wait for **explicit user approval**. Document new decisions with an ADR and synchronized specifications; AI cannot self-approve. After the code repo exists, the reviewed repo docs are canonical and the Project files are synchronized snapshots.

## Add Project context metadata

Set/record: `Architecture reference pack: v1.1.0 (2026-11-18); repo commit: not yet established; implementation stage: none approved.` Update these values as decisions and repo work advance. This is a suggested user action, not a statement that Project settings have been changed.

## Upload guidance

For reliable Project knowledge, upload the individual Markdown documents (or the generated consolidated knowledge Markdown) rather than relying on indexing inside an uploaded ZIP. Keep filenames and version explicit. Project Instructions should be short; full detail belongs in reference files. When the repo exists, the human owner handles Git and Project snapshot updates after reviewing locally prepared docs; AI agents must never perform Git operations to synchronize.
