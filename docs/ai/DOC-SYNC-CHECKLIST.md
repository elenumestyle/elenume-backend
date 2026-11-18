# Documentation Update / Synchronization Checklist

**Use at each approved architecture change and at implementation-stage completion.**

- [ ] Identify APPROVED decisions and any PROPOSED/UNRESOLVED items (Decision IDs / Q-IDs).
- [ ] Obtain explicit architecture approval for any changed rule; record date/evidence.
- [ ] Add accepted or superseding ADR; do **not** rewrite prior ADR history.
- [ ] Update canonical relevant spec sections without introducing unapproved specifics.
- [ ] Update `08-DECISION-REGISTER.md` and `09-OPEN-QUESTIONS.md`.
- [ ] Update `AGENTS.md`, Project Instructions and Codex task template if agent-facing rules changed.
- [ ] Review scope creep, dependency conflicts, security/privacy implications, module boundaries, cross-module references and portability.
- [ ] Validate links, Markdown readability, metadata/status/version consistency, and acceptance wording.
- [ ] Increment version (MAJOR/MINOR/PATCH policy).
- [ ] Give the human the changed files and recommended commit/PR notes; **only the human** reviews, stages, commits, pushes and handles PRs. Record the human-confirmed reviewed commit SHA.
- [ ] Regenerate consolidated Project knowledge snapshot locally; **human** uploads/synchronizes the ChatGPT Project files and records pack version/human-confirmed commit.
- [ ] Update active agent prompts/context where they still reference an old pack.
- [ ] Verify **D-029**: agent did not execute even read-only Git commands or GitHub operations.
- [ ] Keep implementation-stage authorization **separate** from architecture approval.

If no architectural decision changed, mark checklist N/A in the stage report; do not create a governance-only PR merely to assert nothing changed unless required by the user.
