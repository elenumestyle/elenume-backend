# ADR-0004 — Mobile-First Registration, Idempotent Account Provisioning and Authentication Policies

- **Status:** ACCEPTED
- **Decision IDs:** D-014, D-015, D-016, D-017
- **Recorded:** 2026-10-12 (original handoff plus five-item refinement review)
- **Supersedes:** none
- **Canonical detail:** `../architecture/03-IDENTITY-AND-SECURITY.md`, `04-ACCOUNTS-AND-SELLERS.md`

## Context

Public users may register with a verified phone and add a username later. Separate Identity and Accounts ownership must not produce duplicate UserIds or inconsistent completed accounts after partial failures.

## Decision

Support verified-mobile-only initial registration, later optional username, and public sign-in with either username/password or phone/SMS OTP. Identity coordinates recoverable, idempotent registration/provisioning with Accounts; incomplete provisioning cannot masquerade as a completed registration. Public password minimum eight with no composition requirement; Admin minimum eight plus uppercase/lowercase/numeric/symbol and mandatory SMS MFA. Seller may opt into username/password/SMS OTP in Seller Panel context. OTPs must be purpose-specific, one-time, short-lived and abuse controlled.

## Consequences and trade-offs

Registration involves an explicit consistency workflow rather than cross-host EF entity coupling. SMS is an accepted practical choice despite its security trade-offs. Precise SMS provider, OTP thresholds, recovery policies and reconciliation algorithm require later implementation-stage decisions.

## Not authorized by this ADR

External sign-in, passkeys, universal seller-password enforcement, exact OTP values/timeouts, or a live SMS integration stage.
