# ELENUMÉ .NET — Architecture Reference Pack

**Release:** 1.1.0  
**Baseline date:** 2026-10-12; **latest accepted operations policy:** 2026-11-18  
**Status:** Approved architectural baseline; **no application implementation approved**  
**Scope:** New, clean ELENUMÉ .NET project (not a port of the Supabase MVP)  
**Maintainers:** ELENUMÉ decision owner (approval), architecture/AI assistants (drafting), implementing agents (compliance)

## START HERE

1. Read [`AGENTS.md`](AGENTS.md) before any AI-assisted architecture or coding task.
2. Read [`docs/architecture/00-GOVERNANCE.md`](docs/architecture/00-GOVERNANCE.md) for authority, change control and approval rules.
3. Consult [`docs/architecture/08-DECISION-REGISTER.md`](docs/architecture/08-DECISION-REGISTER.md) to distinguish approved decisions from open choices.
4. Read the relevant domain standards before preparing a stage proposal or changing code.
5. For an implementation request, complete the [stage proposal template](docs/templates/STAGE-PROPOSAL-TEMPLATE.md), obtain explicit user approval, and implement only that scope.

## Canonical specifications

| File | Purpose |
|---|---|
| [00 — Governance](docs/architecture/00-GOVERNANCE.md) | Normative terms, authority, approvals, change process |
| [01 — System baseline](docs/architecture/01-SYSTEM-BASELINE.md) | Stack, topology, deployment and deferred strategy |
| [02 — Modules](docs/architecture/02-MODULE-BOUNDARIES.md) | Nine logical domains, ownership, dependencies |
| [03 — Identity](docs/architecture/03-IDENTITY-AND-SECURITY.md) | Identity realms, authentication, sessions, OTP |
| [04 — Accounts and Sellers](docs/architecture/04-ACCOUNTS-AND-SELLERS.md) | IDs, registration, owner model, seller rules |
| [05 — Persistence](docs/architecture/05-PERSISTENCE-AND-INTEGRATION.md) | DbContexts, schema ownership, events, portability |
| [06 — Applications and APIs](docs/architecture/06-APPLICATIONS-AND-APIS.md) | Three BFFs, contracts, internal API, frontend |
| [07 — Engineering standards](docs/architecture/07-ENGINEERING-STANDARDS.md) | Boundaries, testing, security, observability |
| [08 — Decision register](docs/architecture/08-DECISION-REGISTER.md) | Accepted decisions and traceable ADR IDs |
| [09 — Open questions](docs/architecture/09-OPEN-QUESTIONS.md) | Explicitly undecided/deferred items |
| [10 — Stage gates](docs/architecture/10-STAGE-GATES.md) | Mandatory requirements before implementation |
| [11 — Source control operations](docs/architecture/11-SOURCE-CONTROL-OPERATIONS.md) | Human-exclusive Git, GitHub, branches, PRs, merges and publication (D-029) |

## Records, templates and agent-specific instructions

- [Accepted ADRs](docs/decisions/) — historical context, reasons, trade-offs and consequences (eight records).
- [Architecture change workflow](docs/ai/ARCHITECTURE-CHANGE-WORKFLOW.md) — how to request, approve and synchronize decisions.
- [Codex task prompt](docs/ai/CODEX-TASK-PROMPT.md) — reusable task scaffold.
- [ChatGPT Project instructions](docs/ai/CHATGPT-PROJECT-INSTRUCTIONS.md) — concise instructions to paste into Project settings.
- [Stage 1 solution foundation proposal](docs/stages/STAGE-01-SOLUTION-FOUNDATION-PROPOSAL.md) — PROPOSED, not authorized.
- [Original comprehensive handoff](docs/history/ORIGINAL-ARCHITECTURE-HANDOFF.md) — preserved historical source; **not** a higher-priority specification.
- [ADR template](docs/templates/ADR-TEMPLATE.md) and [stage template](docs/templates/STAGE-PROPOSAL-TEMPLATE.md).

## How to use these with ChatGPT Project and Codex

**Before a code repository exists**, this versioned reference pack is the working approved architectural baseline. Upload the individual Markdown reference files to the current ChatGPT Project's files/knowledge, and paste the short Project instructions into its Instructions field where supported. Do not rely only on uploading a ZIP to make its contents searchable.

**When the monorepo is initialized by the human owner**, the human reviews and commits `AGENTS.md` and `docs/` alongside the code. The *reviewed repository revision* becomes the canonical maintained copy; the ChatGPT Project copy is a synchronized reference snapshot, **not a second independently editable source of truth**. After the human commits accepted changes, the human replaces/re-uploads the relevant Project files and record the pack revision/commit in the Project instructions or project notes.

**Codex** should read `AGENTS.md` and applicable docs from the local working directory, without executing Git commands or GitHub operations. Any standalone prompt should point to these docs and name the approved implementation stage.

## Status policy

- **APPROVED:** explicitly accepted architecture, applies to future designs and approved implementations.
- **PROPOSED:** requires user decision; not a coding mandate.
- **DEFERRED:** explicitly not approved for initial implementation.
- **UNRESOLVED:** architectural detail not decided; must not be guessed when material.
- **HISTORICAL:** retained for provenance; superseded portions are not binding.

**Architecture approval never implies implementation approval.**

## D-029: Git/GitHub are human-controlled

The project owner exclusively operates Git, the new GitHub account/repository, branches, staging, commits, pushes/pulls, PRs, merges, CI execution and repository settings. AI agents can modify permitted local source/docs and run allowed local verification only after a stage-specific approval; they MUST NOT execute even read-only Git commands. See [ARC-11](docs/architecture/11-SOURCE-CONTROL-OPERATIONS.md) and [ADR-0008](docs/decisions/ADR-0008-human-controlled-git-and-repository-operations.md). This acceptance is **not** approval for Stage 1 implementation.

## Release 1.1.0 — Approved human-controlled Git operations (2026-11-18)

Added D-029 / ADR-0008 and ARC-11. Updated agent prompts, engineering standards, governance and Stage 1 planning. **No implementation stage approved**, and no GitHub operations have been executed. All Git actions and Project uploads remain human-owned.
