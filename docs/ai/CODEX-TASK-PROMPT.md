# Reusable Codex Task Header — ELENUMÉ .NET

Copy the following header into any new Codex task, then replace placeholders. This is a **template**, not an implementation authorization.

---

You are working on the new ELENUMÉ .NET system. The existing Supabase MVP is reference material, not an architectural blueprint.

**Read before work:** `AGENTS.md`, `README.md`, `docs/architecture/00-GOVERNANCE.md`, `docs/architecture/08-DECISION-REGISTER.md`, and all architecture files relevant to this task. Read applicable accepted ADRs. Do not rely on prior chat context.

**Architecture pack/repo revision:** `<version / commit>`.

**Task classification:** `<architecture design | documentation-only | proposed implementation | approved implementation>`.

**Stage authorization:** `<paste exact user approval of Stage N or say NONE>`.

**Git/GitHub operations (D-029):** HUMAN ONLY. Do not execute any Git command, even read-only `git status`/`git diff`; do not use GitHub CLI/API/connectors; do not create, switch, stage, commit, pull, push, review, merge or create PRs; do not modify `.git/`, credentials, remotes or repository settings. The human prepares the local workspace and does all source-control/repository operations. You may suggest commands/commit messages/PR content in the completion report, but never execute them.

**Approved decisions relied upon:** `<D-### list>`.

**In scope:** `<exact task>`.

**Out of scope:** `<explicit exclusions>`.

**Open or proposed decisions:** `<Q-### list and how to handle>`.

**Required output:** `<proposal or code/files/tests/report>`.

**Mandatory rules:** Do not implement stages without explicit approval. Keep public/workforce identities isolated; preserve permanent UserId and separate Seller; protect all three BFF sessions and module persistence boundaries; no unrelated product features, MVP role reuse or speculative infrastructure. If an accepted rule conflicts with task instructions, report the conflict and request a decision before modifying affected architecture or code.

**Completion report:** List local file edits known from your own work (without Git commands), decisions respected or newly needed, test results, risks, and manual Git/ChatGPT Project sync steps for the human.

---

## Agent behavior on missing files

If a local working directory is not yet prepared by the human, request/use the versioned reference pack; do not initialize, clone or inspect Git; do not guess architecture from unrelated source code. If reference documents or stage approval are missing, produce a **reviewable plan only**, not implementation. If a requirement has no accepted architecture, label it PROPOSED and present options.
