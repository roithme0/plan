---
type: project
status: active
last_reviewed: 2026-08-30
---

# Kochwiki

## Purpose

Provide a private, mobile-first application for managing recipes and their ingredients.

## Current role

Kochwiki currently supports recipe and foodstuff management, including ordered ingredients and preparation steps. Its current implementation is being reviewed and refined before broader feature development continues.

The current frontend contains a cookie-based attempt to restore the selected user, but it is not authentication and may not yet satisfy the intended remembered-user behavior. The backend API is not protected and must not be exposed to agents or other untrusted clients in its current form.

## Near-term focus

- Complete the current review, refactoring, and code-quality cleanup
- Address relevant performance issues
- Decide and implement the intended remembered-user behavior
- Treat actual authentication and machine identities as separate future security work

## Direction

Remain independently useful for core recipe management while adding recipe history, general image support, and optional AI-assisted features. Participate in the wider ecosystem through purposefully designed and authorized APIs.

## Ecosystem relationships

See [[System Overview]].

## Related initiatives

- [[AI-assisted Recipe Optimization]] — planned
- [[Recipe and Ingredient Images]] — planned
- [[Read-only Project Access for the Agent]] — planned
- [[Controlled Agent Actions]] — exploring

## Open questions

- Should future cooking recommendations use only saved recipes, or also an inventory of ingredients actually available at home?
- What form of human authentication should eventually replace or complement the current user selection?
