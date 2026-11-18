# 02 — Module Boundaries and Ownership

**ID:** ARC-02 | **Status:** APPROVED | **Version:** 1.0.0 | **As of:** 2026-10-12

## Nine approved *logical* business-domain modules

| Module | Owns | Must not own |
|---|---|---|
| **Accounts** | Permanent public business User, profile, preferences, addresses/privacy when scoped | Credentials, OTP, workforce identities, Seller as User role |
| **Sellers** | Independent Seller entity, OwnerUserId, onboarding/approval, lifecycle, seller commercial eligibility, seller-context management policy | User login credentials, commercial offers, arbitrary Admin permissions |
| **Catalog** | Canonical fragrances, brands, curated fragrance knowledge, classifications and localized product information | Seller-specific prices/stock, orders |
| **Offers** | Seller offers/listings, commercial variants, prices, publication/availability; simple stock management initially | Canonical fragrance truth, order/payment authority |
| **Commerce** | Cart, checkout coordination, order records and lifecycle; historical commercial snapshots | Gateway processing, canonical catalog and seller records |
| **Payments** | Payment attempts, gateway integrations, outcomes, payment-refund processing | Order truth, seller settlement accounting by default |
| **Reviews** | User fragrance reviews, publication/moderation lifecycle | Private wardrobe/evaluations and Catalog master data |
| **Collections** | Private wardrobe, saved fragrances/lists, personal evaluations | Public review publication, canonical fragrance truth |
| **Discovery** | Fragrance recommendations, discovery sessions and results | Catalog, Offers or Collections authoritative data |

**Identity is a separate security subsystem**, not a tenth business module of the Core API. Admin is an app/security surface, not a giant cross-domain CRUD owner.

## Dependency policy

- Other modules consume **published application interfaces/contracts**; no direct references to another module's internal EF types, `DbContext`, repository, persistence tables or `IQueryable`.
- The host may reference implementations **for composition/DI/endpoint mapping only**. Implementation modules may reference another module's contracts, not its internals.
- Use module-owned identifiers across boundaries and review direct database FK/transaction coupling case by case.
- No circular implementation dependencies. Prefer operation-specific queries/commands and appropriately versioned published events.
- Internal in-process invocation is the default when both modules share a monolith; a service call is not required merely to simulate microservices.
- Enforce dependency restrictions with architecture tests when modules are implemented.

## Approved conceptual interactions

| Consumer | Provider | Intended purpose |
|---|---|---|
| Sellers | Accounts | Verify that owner User exists and is eligible as required |
| Offers | Sellers | Obtain eligibility-to-trade decisions |
| Offers | Catalog | Resolve canonical fragrance information |
| Commerce | Offers | Acquire price, offer availability and stock operations |
| Commerce / payment workflows | Payments | Payment processing through explicit contracts/events |
| Reviews | Catalog | Validate referenced fragrance |
| Collections | Catalog | Reference saved fragrance |
| Discovery | Catalog / Offers | Recommendation inputs and eligible offer context |

This table expresses logical dependencies and **does not prescribe exact methods or synchronization semantics**. The detailed checkout/stock/payment consistency protocol is UNRESOLVED.

## Special boundaries

- **Catalog vs Offers:** canonical fragrance identity/knowledge is not duplicated into separate canonical products for each seller. Commercial formats such as decants, original bottles, sample kits and exceptional product types need subsequent modeling.
- **Offers vs Inventory:** simple inventory may reside in Offers initially; future extraction possible. Reservation semantics not yet approved.
- **Commerce:** cart, checkout coordination and orders are one initial boundary; later split only if justified.
- **Reviews vs Collections:** publishing a public review never implicitly publicizes a private wardrobe, list or evaluation.
- **Discovery vs Collections:** Discovery generates recommendation results; Collections owns saving a user's private list/items.
- **Admin operations:** Admin BFF invokes domain-owned operations subject to redesigned workforce authorization, not direct arbitrary table editing.
- **Product Knowledge System:** Catalog is a plausible initial home for canonical fragrance knowledge, but the complete PKS taxonomy/data architecture remains a later detailed decision. No separate PKS microservice is authorized.

## Module packaging

For an active module, start with **one implementation assembly** organized internally by Domain/Application/Infrastructure/API concerns, and create a small `*.Contracts` assembly **only when an external consumer needs it**. Do not automatically create many empty assemblies. Publish narrow contracts, keep implementation internals internal. No large shared kernel, universal repository or global EF Core model.

## Not part of initial approved modules

Separate Inventory, Shipping/Fulfillment, Seller Settlements, Reporting, Campaigns, Notifications workflow, Organizations and Content Management modules need their own later requirements/decision. Do not create them by extrapolation.
