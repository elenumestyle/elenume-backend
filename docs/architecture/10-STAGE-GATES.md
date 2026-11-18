# 10 — Implementation Stage Authorization and Acceptance Gates

**ID:** ARC-10 | **Status:** APPROVED governance | **Version:** 1.1.0 | **As of:** 2026-11-18

## Current state

Architecture baseline accepted. **No stage of new .NET application implementation is yet authorized.** A proposed **Stage 1: Solution Foundation** is documented in `../stages/STAGE-01-SOLUTION-FOUNDATION-PROPOSAL.md`, but is **not approved**. It is not permission to create hosts, folders, migrations or CI workflows.

## Required proposal before EACH stage

Produce one consolidated review using `docs/templates/STAGE-PROPOSAL-TEMPLATE.md`:

1. Stage objective and **specific in-scope deliverables**.
2. Explicit **out-of-scope** boundaries.
3. Technical approach and alternatives affecting decisions.
4. Data model/migration/provider impacts.
5. APIs/contracts/frontend and BFF impacts.
6. Authentication, authorization, privacy, abuse/failure impacts.
7. Architecture decisions being relied on/changed and any ADR needed.
8. Dependencies/prerequisites and operational assumptions.
9. Tests and objective acceptance criteria.
10. Deferred features and extension points not implemented.
11. Clear decisions requiring user sign-off.

A user must **explicitly authorize** that particular stage before code modifications. Acceptance of an architecture ADR is insufficient. Scope additions during execution need approval where they materially change behavior, security, architecture or resources.

## Stage 1 proposal — not approved

Possible minimal deliverable: repo/solution conventions, a minimal Core API host, DI/module-registration pattern demonstrated with an appropriately scoped boundary, architecture tests, basic health/telemetry and focused CI. It **does not** automatically include full Identity, accounts, Seller onboarding, seller flows, nine empty module projects, production infrastructure, SMS, open APIs or unreviewed schema creation. The pending proposal and choices are Q-014. The user retains all Git/GitHub operations under D-029; AI work is limited to locally edited files and permitted tests after separate stage approval.

## After implementation

Report actual locally edited files (without Git commands), test outcomes and scope adherence. The human handles staging, commits, pushes, pull requests and merges. Where design changed, identify required ADR/spec updates and approval. Keep the repo's canonical docs and the ChatGPT Project snapshot synchronized. Deployment/production activation is not implicitly authorized by local feature implementation.
