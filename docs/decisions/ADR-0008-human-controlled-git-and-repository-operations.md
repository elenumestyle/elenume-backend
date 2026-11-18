# ADR-0008 — Human-Controlled Git and GitHub Operations

- **Status:** ACCEPTED
- **Decision ID:** D-029
- **Recorded:** 2026-11-18, explicit user acceptance of the recommendation that ChatGPT/Codex modify project files while the user handles all Git/GitHub operations
- **Supersedes:** none
- **Canonical detail:** `../architecture/11-SOURCE-CONTROL-OPERATIONS.md`

## Context

The new ELENUMÉ .NET monorepo will reside under a different GitHub account than the existing MVP. The owner wants AI agents focused on code, documentation and local validation, while retaining direct control of repository identity, branches, commits, synchronization, pull requests and merges.

## Decision

The human project owner exclusively handles all Git and GitHub operations, including read-only Git commands. ChatGPT/Codex must not execute Git commands or GitHub operations, create or manipulate branches/PRs, commit/push/pull, or change repository configurations. Within a separately approved implementation stage, agents may inspect normal working files, produce/modify code and docs, author CI workflow *files*, run approved local tests and report results. The human handles publishing, Git history, repository administration and PR review/merge. The new .NET repository is independent of the old MVP repository.

## Consequences and trade-offs

- Stronger human control of publication, source history, and permissions; reduces accidental remote changes.
- AI agents cannot automatically inspect Git status/diff/history, so the human must provide branch/commit context when needed and independently review the working tree.
- CI workflow files may be written locally, but will run remotely only after the human performs relevant Git/GitHub operations.
- The reviewed repository documentation is canonical only after human Git operations; ChatGPT Project snapshots require deliberate human synchronization.

## Explicit non-authorization

This ADR does **not** authorize Stage 1 application implementation, repository initialization, commits, pushes, PR creation, hosting configuration or deployment. Stage 1 must receive its own separate explicit approval. The repository owner, URL and credentials remain for the human to specify.
