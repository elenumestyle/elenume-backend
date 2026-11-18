# 03 — Identity, Authentication and Security

**ID:** ARC-03 | **Status:** APPROVED | **Version:** 1.0.0 | **As of:** 2026-10-12

## Trust boundaries and ownership

- `Elenume.Identity` is a separately deployable ASP.NET Core Identity + OpenIddict host, **not** part of Accounts or the Core API business data layer.
- It contains **two strongly separated realms**: public (`User` customers/sellers) and workforce (administrators/employees/operators). Separate accounts, security rules, client policies and session boundaries. One host initially; separate deployable realms are DEFERRED.
- Business `UserId` is not `IdentityAccountId`; business Seller is not a login role. Workforce identity MUST NOT be inferred from or granted to a public identity.
- Identity is authoritative for **verified login mobile**, login username/credentials, OTP, account authentication/security events and identity credentials. Accounts owns business profiles.
- Detailed Admin permissions are NOT inherited from the existing MVP; they are DEFERRED until redesign.

## Public account policies

- Registration MUST be possible with **only a verified mobile number**; username/email/password are not required to establish initial account.
- Public login methods: **username + password**, or **verified mobile number + SMS OTP**. Both resolve the **same permanent UserId**.
- Optional username may be chosen later, must be unique if used for login and may change without changing UserId; account policy governing changes is not yet frozen.
- Public passwords: **minimum eight characters**; no requirement to include all character classes. Support passphrases. Throttling/abuse defense required; compromised-password filtering to be evaluated during implementation design.
- A verified mobile number maps to at most **one active public User**; normalize phone numbers canonically to prevent duplicates. Exact phone format/uniqueness migrations require a reviewed implementation decision.
- Mobile changes: verify the new number before making it trusted; never change UserId. Lost-number account recovery is a separate DEFERRED policy.

## Seller authentication

- Seller security is **contextual**, not a global change to the public User account.
- Sellers MAY opt into stricter Seller Panel login requiring **username + password + SMS OTP**. Seller security setting is independent of ordinary Storefront sign-in method.
- The Seller Panel MUST check that any required stronger authentication has actually been satisfied; a pre-existing Storefront session does not bypass it. Precise authentication assurance/reauthentication and session invalidation rules to be designed for implementation.

## Workforce / Admin

- Workforce accounts use distinct identities, credential policies and access controls.
- Admin password: **at least eight characters AND uppercase AND lowercase AND digit AND special character**.
- Admin MFA: **mandatory SMS OTP** in the initial approach, acknowledging SMS limitations. Allow future stronger factors architecturally; do not implement them now.
- A public-user token/session MUST NOT authorize workforce operations by itself.

## SMS OTP

SMS is the chosen practical authentication and MFA method. Vendor coupling is forbidden: use an SMS delivery interface and fake/local implementation in development/testing as appropriate. Challenges MUST be purpose-specific, short-lived, one-time, rate-limited, retry-limited and audit-aware. Example separate purposes: Login, VerifyMobile, ChangeMobile, SellerSecondFactor, AdminSecondFactor, AccountRecovery. An OTP for one purpose MUST NOT be reusable for another. Exact expiry, throttling thresholds, resend budget and vendor remain UNRESOLVED implementation decisions.

## Browser OIDC and sessions

- Approved BFF flow: **OIDC Authorization Code + PKCE**, with server-side token handling. BFFs are confidential server components.
- Storefront, Seller and Admin maintain **independent, persistent server-side sessions** and host-only `Secure`, `HttpOnly` cookies (`__Host-` convention with `Path=/` where compatible); select a justified `SameSite` policy.
- Browser JavaScript MUST NOT intentionally receive OAuth access/refresh tokens. BFFs must protect cookie-authenticated mutations against CSRF; account for session fixation, expiry, revocation, sign-out and compromised credentials.
- PostgreSQL-backed session-ticket persistence is one practical candidate, **not** an approved mandatory storage implementation. Redis is not required by default.
- Each BFF authenticates against its permitted realm and audience. Core API revalidates identity and **resource authorization**; user-supplied IDs, client-side route guards or 'this request came from Admin UI' are not authorization.
- Exact OpenIddict token format, encryption, JWT validation/introspection settings, session schema and OIDC client registration are subject to implementation-stage security review.

## Auditing / secrets

Support authentication event auditing and revocation. MUST NOT log OTP plaintext, passwords, access/refresh tokens, SMS secrets or sensitive credential material. Apply least privilege and data minimization. Public seller presentation must not expose private owner, financial or verification records.

## Deliberately deferred

Passkeys; Google/Apple/Microsoft/other external login providers; non-SMS MFA implementation; workforce authorization hierarchy; detailed recovery and federated-client workflows. Future support must not cause speculative code to be created now.
