# Architecture Change Control — For ChatGPT, Codex and Human Reviewers

**Status:** APPROVED governance process | **Pack:** 1.1.0 | **Updated:** 2026-11-18

## Core promise

No AI assistant, coding agent, CI job, test outcome or existing application implementation can silently turn a new architectural assumption into APPROVED policy. **Only an explicit user decision does that.** Similarly, an architecture decision never grants implementation-stage authorization.

## Change workflow

**1. Identify.** Name exact existing Decision IDs, affected canonical files, implementation stages and problem. Distinguish contradiction, new requirement, design refinement, typo and legacy-MVP behavior.

**2. Draft.** Open PROPOSED ADR using `../templates/ADR-TEMPLATE.md` with alternatives, security/privacy, data/API, deployment cost, portability, testing and non-goals. Update `09-OPEN-QUESTIONS.md` as a proposal if needed; do not silently edit accepted normative paragraphs.

**3. Approve.** Get explicit user approval of the decision or a clearly identified option. Record date and evidence in ADR. If user declines, retain proposal as rejected/closed historical context, without changing approved rules.

**4. Synchronize as one change.** Update the accepted ADR (or write a new superseding ADR; keep older accepted ADR immutable), the relevant `docs/architecture/` specifications, `08-DECISION-REGISTER.md`, `09-OPEN-QUESTIONS.md`, `AGENTS.md` if constraints change, and ChatGPT Project prompt guidance. Increment reference pack version per `00-GOVERNANCE.md`.

**5. Validate.** Review relevant accepted rules, cross-references, status labels, no scope inflation, accidental implicit feature approvals, privacy/security/infrastructure impacts, and whether new rules have tests/acceptance gates.

**6. Publish (human-controlled).** AI agents prepare revised documentation/snapshot and report the affected files. Once a repository exists, **only the human** reviews, stages, commits, pushes and manages PRs. The human-reviewed commit is canonical. The **human** re-uploads changed Markdown files or the regenerated consolidated snapshot to ChatGPT Project, records **repo commit SHA + pack version** in Project notes, and supplies refreshed context to active Codex tasks. AI agents must not execute any Git command, even read-only, or use GitHub APIs/connectors. If the repository doesn't exist, publish one versioned reference pack as baseline.

**7. Implement only after a separate approved stage.** Copy approval scope into task prompt and constrain edits. Do not treat approved documentation PR as implementation permission.

## Suggested decision-request message

> Proposed architecture change: <title>. Current approved rule: <D-### + file>. Problem/trigger: <why>. Options: <A, B, C>. Recommendation and trade-offs: <...>. Affected domain/API/security/persistence/deployment: <...>. What remains deferred: <...>. Please explicitly approve, reject or request revision. No implementation is requested at this point.

## Conflict protocol

When current specs, accepted ADRs and AGENTS disagree: stop only the affected work; produce a short conflict report quoting both passages, proposing a resolution and requesting approval. Do not treat 'latest generated document' as enough authority. Proceed with unrelated authorized work only if the conflict cannot affect it.

## Practical update cadence

- During a stage: record a newly needed architecture proposal immediately; secure approval before implementation relies on it.
- At every completed stage: confirm spec/ADR/register alignment, log no-op when no changes; give human the documentation snapshot for manual Project sync if needed.
- Before major Codex tasks: read manifest/pack version + affected specifications + stage authorization.
- Periodically: perform a light documentation review for outdated dependencies/assumptions and missing links, **without** modifying approved architecture unless separately approved.

## Example of an acceptable update

If Checkout later needs reliable stock reservations, create a proposed ADR scoped to Commerce/Offers/Payments, compare alternatives, get explicit user approval, then amend the appropriate data/integration contract sections. Do **not** silently introduce a message broker, new Inventory service or a distributed saga in Stage 1.
