---
type: decision
status: accepted
date: 2026-09-10
affects:
  - "[[Kochwiki]]"
  - "[[AI Service]]"
---

# DEC-002: External OIDC Provider for Human Authentication

## Context

Kochwiki currently remembers a selected user in the browser, but this is not authentication and the backend API is not protected. Reliable user identity is required before recipe ownership, shared Foodstuff provenance, or AI interactions on behalf of a user can be enforced.

Only Kochwiki needs browser authentication initially. Future applications should nevertheless be able to rely on the same identity provider and benefit from single sign-on without making Kochwiki responsible for user credentials.

## Decision

- Human users authenticate through an external identity provider using OpenID Connect and OAuth 2.0.
- Applications rely on standards-based identity and authorization flows rather than provider-specific identity logic where practical.
- Kochwiki maintains a local user record for domain data and relates it to a stable external identity using issuer and subject identifiers.
- Email address and display name are attributes, not the durable identity key.
- Each future application or API receives its own client or resource registration and validates tokens or sessions intended for it.
- The identity provider establishes identity; each domain service remains responsible for its own authorization decisions.
- Human authentication and service-to-service authentication remain separate concerns.
- AI interactions act only on behalf of an authenticated user and do not gain independent human access rights.

## Deliberately undecided

- The initial identity-provider product
- Direct browser token handling with Authorization Code and PKCE versus a Backend-for-Frontend session
- The final cookie, token-storage, logout, session-lifetime, and account-provisioning behavior
- Service-to-service credentials and delegated-user propagation

## Consequences

- Additional applications can later share the same sign-in authority and enable SSO.
- Replacing the first provider remains feasible if application integration stays within OIDC and OAuth standards.
- Kochwiki still requires local user lifecycle and account-linking rules.
- Authentication alone does not implement recipe ownership or other domain permissions.
- Provider configuration, redirect URIs, token audience validation, and logout behavior become security-sensitive deployment concerns.

## Alternatives considered

- Application-local passwords, rejected because they duplicate sensitive identity responsibilities and provide a weaker foundation for future applications.
- Treating the current remembered-user cookie as authentication, rejected because it does not establish a trusted identity.
- Selecting Microsoft Entra ID immediately, deferred while it remains the preferred candidate rather than a final product decision.
