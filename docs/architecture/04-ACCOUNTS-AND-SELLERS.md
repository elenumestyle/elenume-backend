# 04 — Accounts, Seller Ownership and Registration

**ID:** ARC-04 | **Status:** APPROVED unless explicitly labeled | **Version:** 1.0.0 | **As of:** 2026-10-12

## Stable identifiers and ownership

| Identifier | Authoritative boundary | Approved meaning |
|---|---|---|
| `IdentityAccountId` | Identity public realm | Internal authentication-account ID |
| `UserId` | Accounts | Permanent ELENUMÉ business public-user ID |
| `WorkforceIdentityId` | Identity workforce realm | Separate workforce authentication-account ID |
| `SellerId` | Sellers | Permanent commercial Seller ID |
| `OwnerUserId` | Sellers | Reference to permanent owning public UserId |

`IdentityAccountId` and `UserId` MUST be **distinct identifiers** with a unique mapping. Changing username, password, verified mobile or future login provider MUST NOT alter UserId. SellerId remains independent of and distinct from UserId and OwnerUserId. Workforce IDs cannot be used as public UserIds.

UUID / .NET `Guid` is a practical **PROPOSED representation**, not a separately approved implementation choice. The creation/serialization strategy remains UNRESOLVED until the relevant implementation-stage review.

## Public User

- A public User may act as a customer and simultaneously own a Seller.
- Accounts owns non-credential business/profile data; it is not a subclass of `IdentityUser`.
- Username and verified login phone are Identity concerns, not alternate public User primary keys.

## Registration/provisioning — accepted design

1. Identity verifies possession of the mobile number via purpose-specific OTP.
2. Identity coordinates public-account creation, allocation/association of permanent UserId, and a **recoverable, idempotent** provisioning request to Accounts.
3. Accounts creates or returns the User record for that UserId idempotently.
4. Registration is complete and an ordinary app session may be established **only when both identity and business-user provisioning have been reconciled**.
5. If the second step is unavailable, the registration remains recoverably pending; **retry MUST NOT mint a second UserId** or create a duplicate User.
6. Do not bind both separate ownership contexts into a cross-application database transaction by default.

**Detail not yet approved:** precise ordering of ID allocation, persistence schema, retry/outbox strategy, reconciliation process and error response contracts. The invariant and coordination ownership above are approved; implementation strategy will be reviewed during Identity/Accounts stage.

## Seller relationship

- Seller is an independent commercial entity, **not a role flag** on User.
- V1: one public User owns **at most one Seller** (`User 1 -> 0..1 Seller`); a Seller has one OwnerUserId.
- Sellers MUST obtain approval before trading. Creating a Seller is not the same as granting trading permission.
- User and Seller account lifecycles are independent; User lock/suspension MUST NOT implicitly delete Seller records. Historical commercial links must be preserved.
- Ownership transfer and multiple Seller memberships/owners remain DEFERRED. Preserve a plausible path for ownership transfer without implementing it.
- Exact lifecycle enum names/transitions (e.g., Pending, Active, Suspended, Rejected, Closed), resubmission policy and the behavior of rejected historical registrations are **PROPOSED/UNRESOLVED**, not accepted schema.

## Authorization: two distinct questions

- `CanManageSeller(UserId, SellerId)` determines whether the acting public User may access Seller management. **V1 owner-only, full owner access**, subject to valid authentication and applicable Seller Panel security requirements. Expose the check as an abstraction; do not scatter `OwnerUserId == currentUserId` in handlers.
- `CanTrade(SellerId)` separately decides whether the commercial entity may trade, accounting for approval/suspension rules. Owner management rights MUST NOT imply trading eligibility.
- The owning User must not obtain workforce permissions merely by owning a Seller.
- Do not assume a suspended Seller loses all ability to view historical orders or remediate onboarding; granular operation policy remains to be designed.

## Seller data exposure

Public presentation contracts may contain approved display name, slug, logo and public profile/rating. Private owner identity, legal/verification records, business contact details, financial and operational status are restricted and exposed only by authorized contracts.

## One-Seller uniqueness implementation

Enforce the V1 rule under concurrent registration, using application validation and an appropriate reviewed persistence constraint. Exact uniqueness strategy, retries and resubmission behavior must be explicitly decided at Sellers stage rather than inferred from a proposed enum.

## Explicit non-features

No Organization, OrganizationMembership, SellerMembership, seller employees, seller roles/permissions, multiple owners or owner-transfer workflows in V1. These are not scaffolding requirements.
