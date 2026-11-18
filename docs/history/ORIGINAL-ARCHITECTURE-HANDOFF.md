# ELENUMÉ .NET — Comprehensive Architecture Handoff

## 1. Purpose of this handoff

This document contains the current architectural decisions, constraints, deferred decisions, and implementation-governance rules for a **new version of ELENUMÉ built with .NET**.

The new chat should treat this document as the current architectural baseline.

The next work should continue from these decisions rather than restarting the technology-selection discussion or assuming the architecture of the existing MVP should be copied.

---

# 2. Project context

ELENUMÉ currently has an MVP under development using:

- Next.js / React / TypeScript
- Supabase
- PostgreSQL
- Supabase Auth
- Supabase Data API
- PostgreSQL RLS and other PostgreSQL-specific security/data mechanisms

The new .NET version is **not intended to be a direct migration or rewrite of the current Supabase implementation**.

It should be treated as a **new, clean ELENUMÉ project**.

The existing MVP is useful as:

- a source of domain knowledge;
- behavioral evidence;
- UI/visual-design reference;
- test/reference scenarios;
- a source of lessons learned;
- evidence of previous architectural problems;
- a reference for product requirements that remain valid.

It must **not** automatically be treated as:

- the schema template;
- the domain-model template;
- the authorization template;
- the authentication template;
- the backend architecture template;
- the frontend architecture template.

Many product policies are changing, so the new project should continuously ask:

> What is the correct model for the new ELENUMÉ system?

rather than:

> How do we reproduce the existing Supabase implementation in .NET?

---

# 3. Implementation-governance rule — extremely important

Architectural agreement does **not** constitute blanket authorization to implement features.

Implementation must proceed in explicit stages.

Before implementing **every stage**, first present:

1. proposed scope;
2. what is explicitly not in scope;
3. technical approach;
4. database/schema implications;
5. API implications;
6. security implications;
7. important architectural decisions;
8. dependencies;
9. planned tests;
10. intentionally deferred capabilities;
11. any decision that still requires approval.

Then wait for the user's **explicit approval**.

Do not implement that stage until approval is given.

This requirement applies even when the general architecture has already been agreed.

The objective is:

> Keep the initial implementation simple, practical, and cost-effective while preserving clean extension paths for future capabilities.

Do not build speculative features merely because they might eventually be useful.

Prefer designing appropriate boundaries now and implementing the actual feature only when required.

---

# 4. Core technology decisions

The following are agreed:

## Backend

- **.NET 10 LTS**
- **ASP.NET Core 10**
- C#

## Persistence

- **EF Core 10**
- **PostgreSQL 18**

## Frontend

- **Next.js**

Next.js remains an appropriate frontend technology.

Whether the existing MVP frontend code itself will be reused should be decided later after reviewing its coupling to Supabase and the old backend architecture.

The primary reuse interest is the existing:

- visual design;
- UI;
- UX;
- components where appropriate;
- layouts;
- responsive behavior;
- assets;
- design language.

If the existing frontend infrastructure is too coupled to the MVP backend, use it as a visual/behavioral reference and implement a clean frontend architecture instead.

---

# 5. Fundamental product-surface separation

This is a strict requirement.

The following must be **completely separate applications**:

1. Customer-facing Storefront
2. Seller Panel
3. Admin Panel

They must not become three role-based modes inside one large frontend application.

Recommended structure:

```text
Storefront
    Next.js Storefront
    ASP.NET Core Storefront BFF

Seller
    Next.js Seller application
    ASP.NET Core Seller BFF

Admin
    Next.js Admin application
    ASP.NET Core Admin BFF
```

They may share packages such as:

- ELENUMÉ design system;
- fonts;
- visual tokens;
- icons;
- localization utilities;
- generic UI primitives;
- generated API contracts/types.

They should **not share by default**:

- route trees;
- application shells;
- browser sessions;
- security policies;
- BFFs;
- deployment units;
- runtime configuration;
- surface-specific business logic.

Each should be independently buildable and deployable.

---

# 6. BFF architecture

The project will use **Backend for Frontend (BFF)** architecture.

There should be separate BFF boundaries:

```text
Storefront → Storefront BFF

Seller Panel → Seller BFF

Admin Panel → Admin BFF
```

Do not create one universal BFF serving all three surfaces unless a future explicit architectural decision changes this.

The BFF acts as a stable facade between the frontend and internal backend architecture.

This is also important for future microservice migration.

Example:

Today:

```text
Seller Next.js
    ↓
Seller BFF
    ↓
ELENUMÉ modular monolith
```

Future:

```text
Seller Next.js
    ↓
Seller BFF
    ├─ Seller Service
    ├─ Orders Service
    ├─ Inventory Service
    └─ Catalog Service
```

The Seller frontend should not need to know that the internal backend topology changed.

---

# 7. Same-origin architecture

Use same-origin frontend+BFF architecture **per application**.

Example Storefront:

```text
https://elenume.com/*
    → Next.js Storefront

https://elenume.com/api/*
    → Storefront BFF

https://elenume.com/auth/*
    → Storefront BFF
```

Seller:

```text
https://seller.elenume.com/*
    → Seller Next.js

https://seller.elenume.com/api/*
    → Seller BFF
```

Admin:

```text
https://admin.elenume.com/*
    → Admin Next.js

https://admin.elenume.com/api/*
    → Admin BFF
```

Note:

`elenume.com`, `seller.elenume.com`, and `admin.elenume.com` are intentionally different browser origins.

The same-origin principle applies **within each frontend/BFF pair**.

A reverse proxy/ingress/load balancer may route the paths internally.

---

# 8. Browser session architecture

For browser applications, use **server-side sessions**.

OAuth/OIDC access and refresh tokens must not intentionally be exposed to browser JavaScript.

The browser should receive an opaque session reference using a cookie configured approximately as:

```text
Secure
HttpOnly
host-only
appropriate SameSite policy
Path=/
```

Each surface must have an independent session.

Conceptually:

```text
Storefront
__Host-ELENUME-STOREFRONT

Seller
__Host-ELENUME-SELLER

Admin
__Host-ELENUME-ADMIN
```

Do not use one broad `.elenume.com` authentication cookie shared between all three surfaces.

Separate sessions do not prevent SSO because multiple BFFs may authenticate through the same identity provider while creating independent local sessions.

Browser JavaScript should not need to manage OAuth tokens.

Native mobile applications and future external/partner API clients may use appropriate OAuth/OIDC token flows separately.

---

# 9. Backend architecture

Start with a **modular monolith**.

Do not begin with business microservices.

However, there is a **high probability** that ELENUMÉ will eventually move partially or fully toward microservices.

Therefore:

> The modular monolith must be designed for low-cost future extraction of modules.

The objective is **extractability**, not pretending that the application is already distributed.

Avoid premature:

- Kafka;
- RabbitMQ;
- service mesh;
- Kubernetes-specific architecture;
- distributed transactions;
- dozens of independently deployed services;
- unnecessary network calls;
- event sourcing;
- excessive CQRS infrastructure.

Introduce these later only when real requirements justify them.

---

# 10. Microservice-extractability rules

The architecture should aim for the following principles from day one:

- each domain concept has clear module ownership;
- module boundaries are explicit;
- module persistence ownership is explicit;
- prefer module-specific EF Core `DbContext`s;
- do not create cross-module EF entity navigation by default;
- do not allow arbitrary cross-module table reads;
- cross-module physical foreign keys require explicit review;
- cross-module database transactions require explicit review;
- module communication should use explicit application/contracts interfaces;
- domain events may be used internally;
- integration events should remain conceptually distinct;
- an in-process event mechanism is acceptable initially;
- a message broker can be introduced later;
- keep the shared kernel very small;
- persistence entities are not public API contracts;
- modules should not rely on another module's internal tables.

When considering database relations, distinguish:

```text
Inside one module
→ strong relational integrity is normal

Across module ownership
→ consider future service extraction cost
```

---

# 11. Initial database topology

Start with **one PostgreSQL database**.

Do not begin with separate physical databases such as:

```text
CatalogDB
SellerDB
OrdersDB
UsersDB
```

unless a future requirement clearly justifies it.

Logical/module ownership should nevertheless be explicit.

The design should allow a future module to take ownership of its own database when extracted into a service.

---

# 12. Database-portability requirement

PostgreSQL is the initial database, but the architecture must keep the possibility of a future **full or partial database migration** in mind.

Do not make PostgreSQL itself the definition of the application architecture.

The application/domain layer should own business behavior whenever reasonably practical.

PostgreSQL should primarily provide:

- persistence;
- transactions;
- referential integrity;
- constraints;
- indexing;
- durability;
- data consistency.

Avoid recreating the current Supabase architecture where substantial application authorization depends directly on PostgreSQL-specific mechanisms.

---

# 13. Database-feature review policy

**Every database design decision must be reviewed**, even if the feature is generally considered portable or allowed.

“Allowed by default” must never mean “use automatically without consideration.”

A useful classification is:

### Normally acceptable after ordinary design review

- tables;
- PK/FK;
- UNIQUE constraints;
- CHECK constraints;
- normal indexes;
- transactions;
- conventional relational data types;
- UUID identifiers.

### Use cautiously / justify portability cost

- JSON/JSONB;
- views;
- database enum types;
- arrays;
- partial indexes;
- generated columns;
- materialized views;
- database-native full-text search;
- database-specific indexing techniques.

### Avoid as default architecture

- triggers containing business logic;
- PL/pgSQL business workflows;
- PostgreSQL RLS as the primary business-authorization model;
- PostgreSQL extensions;
- stored procedures containing core application behavior.

This is not an absolute ban.

A PostgreSQL-specific feature may be used where its value clearly outweighs portability cost, but the decision should be explicit.

---

# 14. EF Core strategy

Use **EF Core 10** as the normal persistence layer.

However:

- EF entities should not define the domain architecture;
- domain entities should not merely be database records;
- do not expose `DbContext` indiscriminately throughout the system;
- keep persistence implementation inside infrastructure/module boundaries;
- use optimized SQL selectively when a measured requirement justifies it;
- isolate provider-specific SQL inside infrastructure code.

The desired model is approximately:

```text
Domain
    ↑
Application
    ↑
Infrastructure
    ↓
EF Core / Npgsql
    ↓
PostgreSQL
```

---

# 15. API architecture

Use:

- REST APIs;
- OpenAPI;
- generated TypeScript API clients for Next.js.

The frontend should consume application contracts, not database structures.

Conceptually:

```text
ASP.NET Core contracts
        ↓
OpenAPI
        ↓
generated TypeScript client
        ↓
Next.js applications
```

Do not expose EF entities directly through APIs.

Do not expose database-generated generic CRUD APIs directly to browsers.

---

# 16. Authentication and Identity stack

The selected identity technology is:

- **ASP.NET Core Identity**
- **OpenIddict**

ASP.NET Core Identity handles local account/credential capabilities.

OpenIddict provides OAuth 2.0 / OpenID Connect protocol capabilities.

Identity should have a dedicated application/trust boundary, conceptually:

```text
Elenume.Identity
```

The business backend must not become coupled to ASP.NET Core Identity's database schema.

---

# 17. ELENUMÉ-owned identity

This is a core architectural principle.

The permanent ELENUMÉ public-user identifier is:

```text
UserId
```

`UserId` must be independent from:

- Username
- Email
- Mobile phone number
- password credentials
- external identity-provider identifiers
- SellerId

Changing username, email, phone number, password, or future authentication provider must not change `UserId`.

Do not model the core ELENUMÉ domain User as:

```text
class User : IdentityUser
```

The authentication account should map to the permanent ELENUMÉ `UserId`.

---

# 18. Public vs workforce identities

There are two separate security realms/boundaries.

## Public identity

Used by:

- customers;
- sellers.

A single public `User` may be both a customer and a seller.

## Workforce identity

Used by:

- administrators;
- ELENUMÉ employees;
- internal operators.

The workforce identity must remain separate from public-user identity.

An employee who shops on ELENUMÉ may therefore conceptually have:

```text
Public User identity
+
Workforce identity
```

Do not grant Admin privileges to the person's customer identity.

---

# 19. Public-user registration

A new public user must be able to register using **only a verified mobile number**.

Username is not required during initial registration.

Flow:

```text
enter mobile number
    ↓
SMS OTP challenge
    ↓
verify mobile ownership
    ↓
create ELENUMÉ UserId
    ↓
account established
    ↓
user may choose/set username later
```

Email also must not define identity.

---

# 20. Public-user login methods

Customers and sellers must support:

### Method A

```text
Username + Password
```

### Method B

```text
Mobile Phone Number + SMS OTP
```

The mobile number must be verified before it can be used for authentication.

The same ELENUMÉ `UserId` must be resolved regardless of the authentication method used.

---

# 21. Public password policy

For customers and sellers:

- minimum length: **8 characters**;
- do not require simultaneous uppercase/lowercase/digit/symbol composition;
- support long passwords/passphrases;
- abuse/throttling controls should exist;
- common/compromised-password blocking should be considered during implementation design.

---

# 22. Seller stronger authentication

A seller may optionally enable a stricter Seller Panel authentication requirement:

```text
Username
+
Password
+
SMS OTP
```

This should be configurable by the seller.

The stronger requirement belongs to the **Seller security context**, not globally to the public `User`.

For example, the same person could potentially use:

```text
Storefront → mobile OTP
Seller Panel → password + SMS OTP
```

without changing the User identity.

Do not hard-code the architecture so that SMS can never later be supplemented/replaced by another stronger factor.

---

# 23. Admin password and MFA policy

Administrators use separate workforce identities.

Admin password requirements:

- minimum **8 characters**;
- uppercase required;
- lowercase required;
- digit required;
- special character required.

Admin MFA is mandatory.

The chosen practical initial MFA method is:

```text
SMS OTP
```

This decision is intentional despite known SMS-security trade-offs.

Admin authentication should allow stronger mechanisms to be introduced later without redesigning the identity architecture.

---

# 24. SMS authentication architecture

SMS OTP is a first-class authentication capability.

The implementation should eventually provide a provider abstraction such as conceptually:

```text
ISmsSender
```

or a broader verification-message abstraction.

Authentication/domain code must not directly depend on a specific SMS vendor.

Development and automated tests should be able to use a fake/local SMS provider.

OTP challenges should be:

- purpose-specific;
- short-lived;
- one-time use;
- rate-limited;
- retry-limited;
- audit-aware;
- resistant to brute-force and resend abuse.

Possible OTP purposes include:

```text
Login
VerifyMobile
ChangeMobile
SellerSecondFactor
AdminSecondFactor
AccountRecovery
```

An OTP for one purpose must not automatically be valid for another purpose.

---

# 25. Mobile-number management

Mobile phone ownership must be explicitly verified.

The architecture must support:

## Initial verification

```text
number submitted
    ↓
OTP sent
    ↓
OTP verified
    ↓
number becomes verified
```

## Mobile-number change

```text
authenticated user
    ↓
request new number
    ↓
new number receives OTP
    ↓
new number verified
    ↓
change finalized
```

The new number must not become the active trusted number before successful verification.

Changing mobile number does not change `UserId`.

A normal number change must remain conceptually separate from an account-recovery process where the user has lost access to their previous number.

Detailed recovery policy can be decided later.

---

# 26. Mobile-number normalization and uniqueness

Phone numbers should use a canonical normalized representation internally.

Different local representations of the same number must not create different identities.

Initial policy:

> One verified mobile number can belong to at most one active public ELENUMÉ user.

This is necessary because:

```text
mobile number + SMS OTP
```

must uniquely resolve the account being authenticated.

---

# 27. Username

Username is:

- optional at initial registration;
- settable later;
- independent from `UserId`;
- changeable according to future product rules;
- unique if it functions as a login identifier.

Username changes must never change the permanent `UserId`.

---

# 28. External identity providers

Do **not** implement external providers initially.

Examples intentionally deferred:

- Google login;
- Apple login;
- Microsoft login;
- other social/federated providers.

The architecture should not prevent adding them later.

Future external identity mapping should rely on stable provider identifiers such as issuer + subject rather than making email address the identity.

---

# 29. Passkeys

Passkeys are intentionally **deferred**.

Do not implement them in the initial identity scope.

The architecture should remain compatible with introducing them later.

---

# 30. Sessions and auditability

The architecture should support:

- session revocation;
- sign out;
- sign out other sessions where appropriate;
- account disablement;
- security-event audit trails.

Useful security events may eventually include:

```text
UserRegistered
MobileVerified
LoginSucceeded
LoginFailed
PasswordChanged
MobileChangeRequested
MobileChanged
OtpChallengeCreated
MfaEnabled
MfaDisabled
SessionRevoked
AccountLocked
```

Never log passwords, raw tokens, OTP secrets, or equivalent credentials.

---

# 31. Authorization philosophy

Authentication and authorization must remain separate.

Identity answers:

```text
Who is this user?
```

Business modules answer:

```text
What is this user allowed to do?
```

Detailed business roles and permissions should not be frozen into authentication tokens.

Avoid putting large, long-lived business-permission snapshots into OIDC tokens.

---

# 32. Admin authorization redesign

The MVP's current Admin roles and permission hierarchy are considered unsatisfactory.

Do **not** carry them over automatically.

The detailed Admin authorization model is intentionally deferred until the relevant implementation/design stage.

Possible future authorization concepts may include:

- permissions;
- capabilities;
- policies;
- workflow assignments;
- roles;
- resource ownership.

Do not assume the current MVP roles such as Catalog Reviewer or similar internal roles should remain valid.

For now preserve only:

- separate workforce/Admin identity;
- extensible policy-based authorization architecture;
- no hard-coded dependency on the MVP role model.

---

# 33. User ↔ Seller foundational architecture

The following decisions are accepted.

## A User may be both customer and seller

Do not create unrelated customer and seller login identities.

A public `User` can shop as a customer and own a Seller.

## One Seller per User initially

Initially:

```text
User 1 → 0..1 Seller
```

A user may own at most one Seller in V1.

This is an initial product rule, not an assumption that must remain forever.

## Seller is an independent entity

Seller represents the commercial marketplace entity.

It has its own permanent:

```text
SellerId
```

and is not:

- identical to User;
- represented merely as `User.Role = Seller`;
- represented merely as `User.IsSeller`.

Conceptually:

```text
User
    │ owns
    ▼
Seller
```

with:

```text
Seller.OwnerUserId
```

## Seller approval

A Seller must be approved before trading.

Seller existence and Seller authorization to trade are separate states.

## Seller lifecycle separate from User lifecycle

A User account may remain valid even if the Seller is suspended.

A Seller must not be automatically deleted merely because its owner cannot currently authenticate.

Seller status and User status are independent.

## Ownership transfer

Future Seller ownership transfer should remain architecturally possible.

It is **not** an initial feature.

`SellerId` remains permanent if ownership changes in the future.

---

# 34. Seller Panel authorization V1

V1 Seller authorization is intentionally simple.

Exactly one owner has full Seller Panel access.

Conceptually:

```text
Authenticated UserId
    ↓
CanManageSeller(UserId, SellerId)
    ↓
owner?
    ↓
allow full Seller access
```

Do not create initial:

- seller employee roles;
- seller permission hierarchies;
- seller memberships;
- multiple owners;
- delegated access levels.

However, access checks should use an abstraction such as:

```text
CanManageSeller
```

rather than scattering raw:

```text
Seller.OwnerUserId == currentUser.Id
```

checks throughout the application.

This keeps a cheap extension point for future memberships/permissions.

---

# 35. Organization model

Do **not** create an Organization model initially merely for future flexibility.

Initial model:

```text
User → owns → Seller
```

is sufficient.

Future organization/membership structures may be introduced if requirements emerge.

Possible future concepts may include:

```text
Organization
OrganizationMembership
SellerMembership
multiple seller users
multiple sellers per organization
delegated roles/permissions
```

These are deferred.

---

# 36. Seller public vs internal data

Public Seller presentation and internal Seller data should be separate contracts.

Storefront may expose things such as:

- public seller display name;
- logo;
- slug;
- rating;
- public profile.

Admin/internal systems may have access to:

- owner identity;
- verification records;
- contact details;
- legal/business information;
- financial information;
- internal status.

Do not serialize persistence entities directly into public APIs.

---

# 37. Seller lifecycle / deletion principle

Do not treat physical deletion of a Seller as the normal lifecycle action once commerce exists.

Historical records may reference the Seller through:

- orders;
- offers;
- settlements;
- reviews;
- refunds;
- disputes;
- audit records.

Use lifecycle states such as:

```text
Pending
Active
Suspended
Rejected
Closed
```

or equivalent once the Seller onboarding domain is designed.

Exact states are not frozen yet.

---

# 38. Localization

Localization should be a first-class architectural concern.

ELENUMÉ currently needs Persian and English and may need additional languages later.

Do not casually spread permanent patterns such as:

```text
NameFa
NameEn
DescriptionFa
DescriptionEn
```

across every domain object without reviewing the long-term localization model.

Where appropriate, consider translation-oriented data models.

The exact strategy can vary by domain and query requirements.

---

# 39. Search

Search should have an application abstraction.

Do not make the domain permanently dependent on PostgreSQL full-text-search semantics.

Conceptually:

```text
ISearchService
```

with potentially:

```text
PostgreSQL implementation initially

other dedicated search technology later
```

PostgreSQL can absolutely be used initially when appropriate.

---

# 40. Object storage

Use an **S3-compatible object-storage approach**.

Object storage should be abstracted from domain logic.

Database records may store:

- asset identity;
- object key;
- metadata;
- ownership/reference;
- lifecycle/status.

Do not store large binary application assets directly in PostgreSQL without a compelling reviewed reason.

---

# 41. Observability

Observability should exist from the beginning.

Use **OpenTelemetry** as the abstraction.

Initial architecture should support:

- structured logging;
- traces;
- metrics;
- health checks;
- correlation/request IDs.

Avoid coupling core application behavior to one observability vendor.

---

# 42. Frontend-reuse philosophy

The existing frontend should be evaluated later.

Primary reuse interest:

- visual design;
- UI components;
- layouts;
- styles;
- assets;
- UX;
- accessibility work;
- responsive behavior.

Do not assume existing:

- Supabase data-fetching code;
- Supabase Auth;
- Supabase SSR session logic;
- generated database types;
- server actions;
- backend-specific infrastructure

should survive.

The new frontend should consume stable APIs/BFFs rather than directly consuming database/platform SDKs.

---

# 43. “Do not assume” section

The following are especially important.

Do **not assume** that:

- the existing Supabase schema should be reproduced;
- PostgreSQL RLS should be recreated by default;
- the MVP Admin roles are valid;
- Seller is a User role;
- organizations must exist in V1;
- seller employees/memberships are approved;
- multiple sellers per user are approved for V1;
- ownership-transfer UI is approved;
- external login providers are approved;
- passkeys are approved for initial implementation;
- mobile applications are approved;
- microservices should be implemented now;
- a message broker is needed now;
- Redis is required now;
- Elasticsearch/OpenSearch is required now;
- Kafka/RabbitMQ is required now;
- Kubernetes is required now;
- current MVP product policies are automatically valid;
- architectural discussion constitutes permission to implement;
- future features discussed as possibilities are approved features.

If a capability has been described as a possible future extension, that means only:

> the architecture should avoid unnecessarily blocking it.

It does **not** mean it should be implemented.

---

# 44. Development philosophy

Optimize first for:

```text
simplicity
correctness
security
maintainability
clear domain boundaries
low operating cost
reasonable future portability
```

rather than theoretical maximum scalability or architectural fashion.

Prefer:

```text
simple solution
+
clean boundary
```

over:

```text
complex future-proof implementation
```

when both provide a reasonable future extension path.

---

# 45. Current project status

No substantive implementation of the new .NET project has yet been authorized.

The discussions so far establish the architecture baseline only.

Before any implementation begins, the relevant implementation stage must be proposed and explicitly approved.

---

# 46. Next recommended architectural task

The next work should be:

> **Define the initial backend module boundaries and the .NET solution/repository structure for the new ELENUMÉ project.**

This should still be an **architecture/design proposal only**.

Do not create code or repository files yet.

The proposal should cover at least:

- proposed domain modules;
- responsibilities of each module;
- dependencies between modules;
- which concepts belong to which module;
- proposed project/solution structure;
- where the three BFF applications live;
- where `Elenume.Identity` lives;
- where the modular-monolith API/application host lives;
- module-specific `DbContext` strategy;
- common/shared-kernel policy;
- contracts and OpenAPI organization;
- integration-event/domain-event organization;
- test-project structure;
- infrastructure boundaries;
- frontend repository/app structure if relevant;
- how the structure supports later microservice extraction;
- what should deliberately **not** be created yet.

Present alternatives where there is a meaningful architectural trade-off.

Do not implement anything until explicit approval is given.

---

# 47. Decision-making principle for the next chat

When an unresolved architectural issue appears:

1. identify it explicitly;
2. explain why it matters;
3. present the practical alternatives;
4. describe trade-offs;
5. recommend the simplest architecture that meets current requirements while preserving reasonable extension paths;
6. obtain a decision where it materially affects the architecture;
7. do not prematurely resolve unrelated future concerns.

Continue to distinguish:

```text
architecture decision
```

from:

```text
implementation authorization
```

throughout the project.