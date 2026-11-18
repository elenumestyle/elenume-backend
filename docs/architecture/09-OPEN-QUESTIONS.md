# 09 — Unresolved, Proposed and Deferred Decisions

**ID:** ARC-09 | **Status:** OPEN-ITEM REGISTER | **Version:** 1.1.0 | **As of:** 2026-11-18

Items here **must not** be silently implemented. "Deferred" means not part of initial approved scope, not a design requirement to build placeholders.

## Required only at the relevant implementation stage

| ID | Status | Question to decide | Earliest trigger |
|---|---|---|---|
| Q-001 | UNRESOLVED | Exact registration provisioning ordering, retries, recovery, reconciliation, schema and transactional reliability | Identity/Accounts stage |
| Q-002 | UNRESOLVED | Identity realm schema/context partition, OpenIddict client/token/validation configuration and assurance claims | Identity stage |
| Q-003 | UNRESOLVED | BFF persistent session storage implementation and revocation propagation | BFF/Identity stage |
| Q-004 | UNRESOLVED | OTP lifetime, attempt/resend throttles, SMS vendor, rate limits, anti-abuse, compromised-password validation | Identity stage |
| Q-005 | UNRESOLVED | Mobile recovery, verification on change, reassignment/history/recycled numbers | Identity stage |
| Q-006 | UNRESOLVED | Username policy (character/length/change controls), identity ID generation and serialization conventions | Identity stage |
| Q-007 | UNRESOLVED | Exact Seller onboarding fields, verification, approval authorities, lifecycle transitions, rejection/resubmission behavior | Sellers stage |
| Q-008 | UNRESOLVED | Seller Panel step-up authentication/session assurance and how changes revoke existing access | Seller/BFF security stage |
| Q-009 | UNRESOLVED | Catalog fragrance taxonomy, PKS detail, decants/size/variant/product relationships | Catalog/Offers stage |
| Q-010 | UNRESOLVED | Stock reservation and order/payment reliability, gateway callbacks, refunds, idempotency, consistency | Commerce/Offers/Payments stage |
| Q-011 | UNRESOLVED | Search backend and localization storage/translation modeling by module | Relevant domain stage |
| Q-012 | UNRESOLVED | Cross-module physical FKs, logical-reference reconciliation, migrations, per-module database schema names | Relevant DB stage |
| Q-013 | UNRESOLVED | Admin permissions/capabilities/roles and workforce operation policies | Admin authorization design stage |
| Q-014 | PROPOSED, NOT APPROVED | Stage 1 foundation scope, repo/project paths, CI, test fixtures and packages; see `../stages/STAGE-01-SOLUTION-FOUNDATION-PROPOSAL.md` | Stage 1 approval |

## Repository and GitHub ownership

**D-029 APPROVED:** The human exclusively performs all Git/GitHub operations. The new GitHub account/repo identity, credentials and local workspace remain **operator-supplied**, not agent-selected or agent-created. Stage 1 project/scaffolding work is still unapproved; see Q-014. This operational decision does not resolve any other UNRESOLVED item.

## Explicitly deferred or not approved for initial build

- **D-F01:** Organizations, memberships, seller employees, multiple sellers per public User, additional owners, ownership-transfer workflows.
- **D-F02:** External Google/Apple/Microsoft sign-in providers, passkeys and new MFA factors beyond initial SMS.
- **D-F03:** MVP Admin roles/permissions copied without redesign; any complete Admin permission hierarchy before its design stage.
- **D-F04:** Microservices, multiple physical business databases, Kafka/RabbitMQ, Kubernetes, service mesh, excessive CQRS, event sourcing and Redis by default.
- **D-F05:** Shipping/Fulfillment, Seller Settlement, Promotions, Reporting, separate Notifications workflow and separate Inventory service without scoped requirements.
- **D-F06:** Native mobile apps, external partner API clients and any unapproved future product functionality.

## Proposed examples **not** approved as schema or final policy

- Seller states such as `Pending`, `Active`, `Suspended`, `Rejected`, `Closed`.
- `identity_public`, `identity_workforce`, `identity_oidc` and business schema names.
- PostgreSQL as the BFF session-ticket store (practical candidate, not mandated).
- Exact REST route names, C# class/interface signatures, database tables and directory structure.

When a question becomes necessary, open a narrowly scoped design proposal with alternatives/trade-offs and explicit approval; on approval, update the appropriate specs and ADRs before or alongside the authorized stage.
