# 07 — Engineering and AI-Assisted Development Standards

**ID:** ARC-07 | **Status:** APPROVED foundational standards; implementation-specific details UNRESOLVED | **Version:** 1.1.0 | **As of:** 2026-11-18

## Implementation philosophy

Start practical and economical. Prefer a simple implementation plus strong ownership boundary to a feature-rich abstraction that attempts to future-proof every possibility. Build only approved features/stages. Stable extension points do not imply full implementations.

## Dependency and packaging conventions

- One lightweight implementation assembly per **active** module; small contracts assembly if needed by other modules.
- Each module's `Domain`, `Application`, `Infrastructure`, `Api` folders are illustrative internal organization, not an instruction to create empty folders.
- Core API is composition root, not owner of all domain behavior.
- Keep a genuinely minimal BuildingBlocks/shared kernel: use built-in .NET abstractions where adequate; do not create universal repositories, a global EF context or generic CRUD platforms.
- No cross-module implementation references except explicitly reviewed exceptional integration. Contracts remain persistence-agnostic.

## Git and GitHub operations (D-029)

The repository owner alone creates/manages accounts and repositories, clones/fetches/pulls, selects branches, stages/commits, pushes, raises/reviews/merges PRs, and configures permissions or releases. **No Git commands are authorized for AI agents, including read-only commands**, and no GitHub connector/API/CLI actions are authorized for equivalent operations. Agents work only on files in the authorized local workspace, running relevant approved local tests and producing review reports and optional suggested human commands. CI configuration files may be edited if stage-approved; triggering remote CI or deployment is not implied. Consult `11-SOURCE-CONTROL-OPERATIONS.md`.

## Security and privacy

- Enforce authorization on each sensitive business operation. Avoid trusting frontend checks, token role snapshots as lasting source of business truth, or cross-surface sessions.
- Threat-model workflows involving OTP, username/password, workforce MFA, user registration, Seller ownership, seller approval, payment callbacks and sensitive media.
- Follow principles of least privilege, minimal data exposure, secure secret handling and auditable administrative changes.
- Never record raw OTP, password, secret, access/refresh token or unreduced sensitive data in application logs.

## Observability and deployment

OpenTelemetry for traces/metrics; structured logs, request/correlation IDs and health checks from the initial foundation. Keep exporter/vendor choice replaceable. Applications/BFFs independently buildable and deployable even if one inexpensive host runs them initially. Do not add Kubernetes, service mesh, brokers, dedicated search cluster or Redis without reviewed need.

## Testing expectations by layer

| Area | Required protection when implemented |
|---|---|
| Domain | Invariants, state transitions, authorization decisions |
| Modules | Contract-driven application behavior and isolation |
| Architecture | Disallowed module/EF references and host/realm boundaries |
| DB integration | Actual EF mappings, PostgreSQL behavior, migrations and concurrency |
| Authentication | Verified mobile, OTP replay/rate limits, password policy, session revocation, MFA |
| Authorization | Seller owner vs trade checks, public/workforce separation, resource ownership |
| API contracts | OpenAPI contract validity and client compatibility |
| E2E | Sensitive interactions across Next.js, BFF, Identity and Core API |

Select risk-relevant tests per approved change; documentation-only work does not automatically need full application/database/E2E suites. CI should retain necessary security, isolation and production-safety gates.

## Delivery check for AI agents

When responding with architecture or code work, state:

1. Task/stage authorization reference and in-scope boundaries.
2. Approved decisions (including D-029) and any OPEN questions relevant to it.
3. Locally edited files and any architecture document deltas (identified without Git operations).
4. Tests/verification performed, including limitations.
5. New proposed decisions requiring human approval.
6. Whether synchronized ChatGPT Project reference files must be updated.

Never claim a plan, stage, migration or production change was approved without an explicit user decision. The next logical implementation **proposal** is Stage 1 solution foundation; it is not yet authorized.
