---
type: initiative
status: planned
last_reviewed: 2026-09-10
projects:
  - "[[Kochwiki]]"
---

# Browser Authentication Foundation

## Intended outcome

An authenticated user can access Kochwiki through a production-capable external identity provider, and Kochwiki can map that identity to a local user before enforcing ownership and authorization.

## Motivation

Kochwiki needs trusted browser authentication before its multi-user data rules or AI interactions can be secured. Implementing the foundation through OpenID Connect and OAuth 2.0 should also allow future applications to use the same identity provider and add single sign-on without redesigning Kochwiki authentication.

## Direction

Follow [[DEC-002 External OIDC Provider for Human Authentication]].

- Use Authorization Code flow for browser sign-in.
- Keep Microsoft Entra ID Free as the currently preferred provider candidate.
- Preserve provider portability through standard OIDC discovery, issuer, subject, audience, and authorization semantics.
- Evaluate both a single-page application using PKCE and a Backend-for-Frontend using a secure browser session.
- Do not make the choice between those browser architectures in ecosystem planning yet.

## Identity-provider candidates

### Microsoft Entra ID Free

Current preference for a small, controlled user group because it is managed, production-oriented, supports MFA and SSO, and can be used without Entra ID licence cost for the expected scope. Advanced capabilities may require paid licences, and unrelated Azure resources may still have their own costs.

A Workforce tenant appears sufficient while users are explicitly managed or invited. Entra External ID may become relevant if the product later needs customer-style self-registration. Its current core pricing includes a substantial free monthly-active-user allowance, but pricing must be rechecked when a provider is selected.

### Self-hosted alternatives

- authentik provides an open-source, OIDC-certified option oriented toward self-hosting and personal or small-team use.
- Keycloak provides a mature open-source identity and access-management platform with OIDC, OAuth 2.0, SAML, MFA, and SSO, at the cost of greater operational complexity.

### Managed alternatives

- ZITADEL currently offers a managed free tier suitable for a small user population and supports service users.
- Auth0 currently offers a broad free monthly-active-user allowance, while some production, environment, and advanced security capabilities require paid tiers.

Free-tier limits and included features are evaluation inputs rather than architectural guarantees and must be verified again before adoption.

## Browser integration candidates

### Single-page application with PKCE

- Angular initiates Authorization Code flow with PKCE.
- The browser receives tokens and sends an access token to the Kochwiki API.
- FastAPI validates signature, issuer, audience, lifetime, and required claims.
- This follows the standard public-client model and can use a provider SDK such as MSAL when Entra ID is selected.
- Token exposure and browser-side session handling require particular care.

### Backend-for-Frontend

- The browser receives only a secure, HttpOnly session cookie.
- The backend performs the OIDC exchange and keeps provider tokens outside application JavaScript.
- Kochwiki applies CSRF protection and secure cookie policy.
- This reduces browser token exposure but adds backend session state and integration complexity.

Both remain valid candidates. The decision should be made from concrete deployment topology, PWA behavior, same-origin constraints, future shared UI embedding, and operational requirements.

## Scope

- Select and configure the first OIDC identity provider
- Register Kochwiki as an application and its API as needed by the chosen browser pattern
- Implement login, callback, authenticated session or access-token handling, and logout
- Validate identity at the Kochwiki backend boundary
- Create or link a local Kochwiki user by stable issuer and subject
- Reject unauthenticated access to Kochwiki application data
- Establish the authenticated user context used by later authorization rules
- Define environment-specific redirect URIs and configuration without storing secrets in source control

## Out of scope

- Final service-to-service authentication
- Delegating user context from the AI Service
- Advanced Conditional Access or identity governance
- Public self-service registration
- Selecting a provider solely from a temporary free-tier limit
- Implementing recipe ownership and Foodstuff permissions themselves

## Project roles

- The external identity provider authenticates users and supplies verifiable identity claims.
- [[Kochwiki]] validates the resulting identity, maps it to a local user, owns its session boundary, and enforces application authorization separately.

## Dependencies

- A deployment origin and callback URL for each environment
- HTTPS outside local development
- A local-user mapping based on stable external identity
- Clear frontend and backend boundaries for the selected SPA or BFF pattern

## Open questions

- Should the first provider be Microsoft Entra ID Free, authentik, Keycloak, ZITADEL, or Auth0?
- Should Kochwiki use a PKCE-based SPA or a Backend-for-Frontend session?
- How are users initially admitted: tenant membership, invitation, or an application allowlist?
- How should account linking behave if the provider or external identity changes?
- Which login and logout semantics are required for a future multi-application SSO experience?
- How should the selected pattern support an embedded shared agent chat UI?

## Next step

Compare Entra ID Free and the strongest self-hosted alternative against the expected user onboarding and deployment topology, then define a Kochwiki-local authentication specification for the selected provider and browser pattern.
