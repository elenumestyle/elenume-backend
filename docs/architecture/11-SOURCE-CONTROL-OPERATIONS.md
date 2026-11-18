# 11 — Human-Controlled Git, GitHub and Repository Operations

**ID:** ARC-11 | **Status:** APPROVED | **Version:** 1.1.0 | **As of:** 2026-11-18  
**Decision:** D-029 / ADR-0008

## Ownership and authorization

The ELENUMÉ project owner retains exclusive control over **all Git, GitHub and repository-administration operations**. AI assistants and coding agents (including ChatGPT and Codex) MAY edit approved local working-directory files and run permitted local build/test/validation tools **only inside a separately authorized implementation stage**. Their authorization to edit files NEVER includes authority to operate Git or GitHub.

A different GitHub account for the new .NET monorepo is intended; its owner name, repository URL, access and credentials MUST be supplied or configured by the human. The chosen repository is independent of the legacy Supabase MVP. Do not infer the account from local credentials, current working directory or prior project GitHub connections.

## AI agent prohibitions (MUST NOT)

Unless the user explicitly approves a **future revision of D-029**, AI agents must not:

1. Execute **any Git command**, including read-only `git status`, `git diff`, `git log`, `git branch` or `git remote`, as well as `git init`, `clone`, `fetch`, `pull`, `push`, `add`, `commit`, `merge`, `rebase`, `reset`, `checkout`, `switch`, `stash`, `cherry-pick`, or tag operations.
2. Create, select, rename, switch, delete or change branches/tags; stage or commit changes; alter Git history, remotes, hooks, `.git/` internals, credentials, or local repository Git configuration.
3. Create, edit, comment on, review, close, or merge pull requests; initiate repository synchronization; change GitHub repository settings, rules, secrets, permissions, or access controls.
4. Use the GitHub CLI, GitHub APIs/connectors, Git transport, Git hosting UI automation, or other tools to perform equivalent repository operations on the user's behalf.
5. Trigger remote CI runs or deployments, publish packages or releases, or make any remote infrastructure changes as a side-effect of code edits without a separate explicit authorization and policy review.

The prohibition concerns *tool execution by agents*, not authoring text explaining optional commands for the human. Agents MAY provide copyable Git/CLI instructions, suggested commit messages, proposed PR descriptions, and CI workflow **files** when those files are within an explicitly approved stage. The human alone chooses whether to run Git commands, push files, configure CI, open PRs or deploy.

## Allowed local development work (only in approved stages)

- Inspect the provided working-tree files through file and filesystem tools **without accessing `.git/`**; do not execute Git commands even for discovery.
- Create, edit, rename or remove **in-scope, non-Git-control project files**, including source, tests, documentation and proposed workflow YAML.
- Run ordinary local build, compiler, formatter, static-analysis and test tools permitted by the current implementation-stage scope.
- Document the files touched based on actual file operations, compare file contents where possible with ordinary filesystem tools, report tests/failures and identify any edits the human must review.
- Preserve preexisting user files and uncommitted changes; if their ownership or purpose is uncertain, avoid destructive overwrites and seek clarification.

## Operator workflow

1. **Human:** Create/select the new GitHub account/repository; authenticate; initialize/clone/pull; choose the branch; provide a local working directory with reference docs.
2. **ChatGPT + human:** Review a complete stage proposal and record the user's explicit implementation approval.
3. **AI agent:** Read `AGENTS.md`, governance, register, affected specs, and exact stage approval; modify only approved local files; validate; report outputs. **No Git/GitHub operations.**
4. **Human:** Inspect local changes, choose what to stage and commit, push/pull, create and manage PRs, merge, synchronize documentation to Project knowledge, and separately approve deployment if applicable.

AI agents must not claim that a commit, push, PR, branch protection or Project-file synchronization happened unless the human or an independently verifiable source actually established it.

## Governance, source of truth and scope

After the new repository exists, its **human-reviewed and human-committed** architecture documents are canonical. ChatGPT Project files are synchronized snapshots. An AI agent MAY prepare updated docs and regenerate a snapshot locally in an approved documentation task, but **the human performs every Git action and manages the Project upload/synchronization**. An architecture ADR does not authorize application implementation or repository activity.

Stage 1 remains **PROPOSED**, not authorized by the approval of this operational policy. Any exception to this policy requires a separate explicit owner decision and a coordinated new/superseding ADR and spec update.
