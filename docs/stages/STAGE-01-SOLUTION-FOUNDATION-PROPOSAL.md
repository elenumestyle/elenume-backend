# Stage 1 — .NET Solution Foundation (IMPLEMENTATION PROPOSAL)

**Status:** PROPOSED — **NOT AUTHORIZED**  
**Architecture pack:** 1.1.0 (2026-11-18)  
**Approved decisions relevant:** D-001–D-010, D-023–D-029  
**New source-control policy:** D-029 / ADR-0008, human-exclusive Git/GitHub operations

## 1. Objective and outcomes

Establish a minimal, reproducible, AI-friendly new .NET solution in a **human-provided local working directory** connected, solely by the human, to a private GitHub repository on the user's chosen different account. This is a clean system, not a migration or fork of the Supabase MVP.

## 2. Exact in-scope deliverables — after separate Stage 1 approval only

- Locally create the minimal backend `.slnx`, pinned .NET 10 SDK configuration (`global.json`), common build settings (`Directory.Build.props`), central package versions (`Directory.Packages.props`), `.editorconfig`, `.gitignore`, `.gitattributes`, and developer README, with actual filenames subject to user review in this proposal.
- Locally create `backend/src/Hosts/Elenume.Core.Api/` with a minimal ASP.NET Core host, DI composition convention, structured logs, OpenTelemetry-ready foundations, correlation identifiers and one liveness endpoint such as `GET /health/live`. No database or authentication dependency required for startup.
- Create architecture tests and a small host smoke test under `backend/tests/`; use test-only fixtures to demonstrate module registration if needed, rather than placeholder business modules.
- Prepare a minimal `.github/workflows/backend-ci.yml` file with narrowly scoped build/test verification and least-privilege permissions. **AI only writes the file; the human publishes it and is responsible for any remote CI run.**
- Locally place approved canonical `AGENTS.md`, complete versioned `docs/` pack, governance/ADR files and architecture manifest in the user's workspace; maintain internal references. If the human already copied them, validate rather than overwrite without review.
- Report exact files edited, package versions, local build/test outcomes, scope adherence, and any manual setup/publishing steps. No Git commands may be used for the report.

## 3. Explicit out-of-scope work

- **All Git/GitHub operations by agents**: no repository creation, clone, init, branch creation/switch, commits, pushes, pulls, PRs, repository configuration or other Git commands, including read-only commands.
- Identity, OpenIddict, registration, passwords, OTP, Accounts/Sellers business features, or any other implemented domain module.
- PostgreSQL services, EF DbContexts, database schemas/migrations, seeded data, connections to MVP/production databases.
- Storefront/Seller/Admin Next.js or BFF applications, live API contracts/clients, business CRUD endpoints.
- Hosting/deployment, production DNS/credentials, package publishing, cloud actions, Redis, Kafka, RabbitMQ, Kubernetes, speculative microservices.

## 4. Technical approach and alternatives

Use one minimal Core API host, standard ASP.NET Core DI, native hosting, ordinary test tools, and package versions pinned to .NET 10-compatible releases. Avoid nine empty module projects, layered boilerplate, generic repository systems or an unneeded framework. The approved monorepo remains human-created and independently managed from the MVP repo. If package versions cannot be verified locally, disclose limitations and seek approval before material deviations.

Alternative of cloning/forking MVP or mass scaffolding all modules is rejected for Stage 1 scope, not a mandate for any future implementation decision.

## 5. Database and schema implications

**None.** No PostgreSQL server is required to launch the Stage 1 host, and no data, migrations, entities or schemas will be created. Future module-owned EF Core schemas remain separate approval items.

## 6. API, BFF and frontend implications

One simple unauthenticated liveness route only (`/health/live` is a **proposed name**, not an approved contract). No business APIs, authentication routes, BFFs, OIDC clients, OpenAPI domain contracts or frontend code during Stage 1.

## 7. Security, privacy and failure implications

Do not commit or generate real secrets. No accounts/tokens/credentials in fixtures or logs. Ensure the liveness route reveals no sensitive internal information. GitHub workflow file uses minimal permissions, no production secrets and no automatic release/deployment. AI must not use existing GitHub account access or GitHub connectors. The human chooses visibility (private recommended), manages local credentials, and reviews manual publication.

## 8. Dependencies and prerequisites

- **Human:** choose the new GitHub account and private repo name, create/clone or otherwise prepare the workspace, choose branch, manage credentials, keep old MVP repository untouched, and supply its directory to the agent.
- **Human:** approve this specific implementation stage **separately from D-029**.
- **Agent:** read canonical docs and verify local .NET 10 SDK/tooling availability; report blockers. Do not initialize Git, checkout, install remote repository apps or change repository ownership.
- Unresolved implementation detail: exact SDK/package versions, final paths, and CI runners/tooling (Q-014), to confirm during stage authorization; no silent architectural expansion.

## 9. Tests and acceptance criteria

1. Clean `dotnet restore` and solution build pass using pinned .NET 10 SDK and agreed compile/analyzer settings.
2. Core API starts with no database, SMS provider, Identity host, Redis or other infrastructure.
3. Liveness endpoint responds successfully in a local smoke test.
4. Architecture tests verify existing valid boundaries and do not claim to test nonexistent modules.
5. CI workflow file is syntactically reviewed; **remote CI is not run by agents**. Human may later publish and verify its execution.
6. No unapproved domain models/features, DB changes, authentication shortcuts, credentials, or remote side effects.
7. Documentation references and manifest are consistent; agent report lists edited local files and all test results/limitations without Git execution.
8. Agent does not commit/push/PR or modify repo settings; the human handles those actions after review.

## 10. Deferred capabilities

Identity/Accounts/Sellers implementations and all nine domain features, full BFFs/frontends, actual EF Core migrations, live deployment, staging and production infrastructure, real OIDC/SMS/session integrations. Each requires its own later stage authorization.

## 11. Items needing explicit owner sign-off

- **Stage implementation authorization (NOT YET GRANTED).**
- Human-selected GitHub owner/repository and human-supplied local workspace (details are not required to approve architecture policy).
- Accept or revise proposed Stage 1 file paths/project names, liveness route, local tooling and minimum CI workflow file.
- Confirm Stage 1 acceptance requires **local test evidence**, while remote PR/CI/merge activities remain owner-only and outside agent execution.

## 12. Authorization request

**Please approve implementation of Stage 1 — .NET Solution Foundation, limited to the local files, build/test validation, and explicit exclusions described above.** This approval would not authorize agents to run Git/GitHub commands, create/merge PRs, use credentials, implement other stages or deploy anything. Until that separate approval is given, this document is architecture/implementation planning only.
