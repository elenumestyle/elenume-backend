# 00 — Governance, Authority and Document Lifecycle

**ID:** ARC-00 | **Status:** APPROVED | **Version:** 1.1.0 | **As of:** 2026-11-18

## Mission and boundaries

These files govern the **new ELENUMÉ .NET application**. The existing Next.js/Supabase MVP is a source of product/domain knowledge, UI/UX reference, test cases and lessons; it is **not** a default database, identity, authorization, frontend or backend blueprint.

Optimize for simplicity, correctness, security, maintainability, clear domain ownership, low cost and reasonable portability. Preserve extension boundaries rather than prematurely implementing future features.

## Normative terminology

- **MUST / MUST NOT:** accepted requirement or hard constraint.
- **SHOULD / SHOULD NOT:** accepted default; deviations require documented reasoning and, where architectural, explicit approval.
- **MAY:** optional within an approved scope; not a grant of implementation authority.
- **APPROVED:** explicitly accepted by the user.
- **PROPOSED:** suggested, not accepted.
- **DEFERRED:** not approved for initial scope; preserve a practical extension path only.
- **UNRESOLVED:** no accepted choice exists.

An example, illustrative type, file tree, tentative status enum, future service, or sample endpoint is **not** an approved feature or schema.

## Source authority and conflict resolution

1. Explicit user-approved decisions govern architectural policy.
2. `docs/architecture/` consolidates their current normative consequences, indexed by `08-DECISION-REGISTER.md`.
3. `docs/decisions/ADR-*.md` preserves the accepted decision, trade-offs and approval provenance.
4. `AGENTS.md` is an operational synopsis, not permission to override detailed standards.
5. `docs/history/` and legacy MVP source are historical/reference materials, not normative over current approved records.

**On apparent conflict among current APPROVED sources: stop the affected task, identify both documents, propose reconciliation, get explicit approval, then update them together.** Do not silently choose one. More recent **proposed** writing cannot supersede accepted decisions.

## Mandatory stage authorization

Before **every** implementation stage, present and obtain explicit user approval of:

1. In-scope work.
2. Out-of-scope work.
3. Technical approach.
4. Database/schema implications.
5. API/contract implications.
6. Security/privacy implications.
7. Significant architecture decisions and alternatives.
8. Dependencies and assumptions.
9. Planned tests and acceptance criteria.
10. Deferred capabilities.
11. Remaining decisions needing approval.

Do not create application code, repository scaffolding, migrations, UI features or infrastructure for that stage until approval is explicit. An approved stage may contain ordinary in-scope edits, tests and fixes; no separate approval is necessary for each trivial file change unless the user limits authorization. Any material scope change needs new approval.

**Documentation and architecture proposals are allowed when requested; they do not authorize implementation.** This reference pack is a documentation deliverable.

## Keeping the reference pack current

For every new architectural decision:

1. Draft a change proposal showing current rule, alternative(s), trade-offs, impact and what remains deferred.
2. Mark it PROPOSED; do not amend APPROVED normative text as if accepted.
3. Obtain explicit user decision; record date and decision context.
4. Create an accepted ADR (or superseding ADR), update the canonical affected specs and register, adjust `09-OPEN-QUESTIONS.md`, `AGENTS.md`, prompts/templates if necessary, and increment pack version.
5. Link the decision to an implementation stage only if the user separately authorizes that stage.
6. When a repository exists, **the human owner** reviews and commits the coordinated documentation change and manually refreshes the ChatGPT Project copy. AI agents prepare local files and report the proposed synchronization steps; they do not execute Git/GitHub operations. Note the human-provided commit/version and update any running Codex context.
7. Audit for contradictory rules, dead links, stale status markers and unapproved feature creep before completing the change.

**Human-controlled source control (D-029):** AI agents MUST NOT execute Git or GitHub operations, including read-only Git commands; see `11-SOURCE-CONTROL-OPERATIONS.md`. Local code editing requires separate approved stage scope.

**Single maintained authority:** before the repository exists, this versioned reference set is the working baseline. After the repo exists, its human-reviewed and committed `AGENTS.md` and `docs/` are canonical; Project files are synchronized snapshots. Never silently update only one copy.

## Versioning and review

Use `MAJOR.MINOR.PATCH`: MAJOR for a breaking approved architectural reversal; MINOR for accepted new architectural rules/modules; PATCH for clarification, formatting and nonsemantic fixes. Record the release and approval details in the decision register. A project instruction or AI prompt with a different revision must be refreshed or flagged.

The decision owner is the user. AI assistants may propose, assemble, evaluate and check documents, not independently authorize new standards.
