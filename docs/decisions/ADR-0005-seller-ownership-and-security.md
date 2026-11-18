# ADR-0005 — Independent Seller, One Owner in V1 and Separate Management/Trading Checks

- **Status:** ACCEPTED
- **Decision IDs:** D-019, D-020, D-021, D-027 (seller-related)
- **Recorded:** 2026-10-12 (original handoff plus explicit five-item refinement review)
- **Supersedes:** none
- **Canonical detail:** `../architecture/04-ACCOUNTS-AND-SELLERS.md`

## Context

A person can be both customer and seller. Seller identity/lifecycle and marketplace eligibility must survive changes to the owner's login. Owner-only control should not force a speculative membership system.

## Decision

Seller is its own commercial entity with permanent SellerId and OwnerUserId. One public User can own at most one Seller in V1; Seller approval is required before trading. User and Seller lifecycle are independent. Seller Panel V1 management access is owner-only full control through `CanManageSeller(UserId, SellerId)`. A separate `CanTrade(SellerId)` check determines trading eligibility.

## Consequences and trade-offs

The authorization checks form extension points for later permissions and transfer. A user may manage pending onboarding without necessarily being allowed to trade. Exact status transitions and management access during suspension must be designed later.

## Not authorized by this ADR

Organizations, staff memberships, seller role hierarchy, multiple sellers per user, multiple owners, ownership-transfer workflow, or a final status enum.
