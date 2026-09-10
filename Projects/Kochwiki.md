---
type: project
status: active
last_reviewed: 2026-09-10
---

# Kochwiki

## Purpose

Provide a private, mobile-first application for managing recipes and their ingredients.

## Current role

Kochwiki currently supports recipe and foodstuff management, including ordered ingredients, preparation steps, and calculated recipe nutrition. Users can create recipes and edit an existing recipe from its detail page through a full-screen recipe edit dialog.

The edit dialog currently loads the active recipe and submits changes through the backend recipe update endpoint. Updates replace the stored recipe state directly; recipe versions and drafts do not yet exist in the persistence model.

The current frontend contains a cookie-based attempt to restore the selected user, but it is not authentication and may not yet satisfy the intended remembered-user behavior. The backend API is not protected and must not be exposed to agents or other untrusted clients in its current form.

## Near-term focus

- Complete the current review, refactoring, and code-quality cleanup
- Address relevant performance issues
- Decide and implement the intended remembered-user behavior
- Introduce recipe versioning and drafts before AI-assisted editing can persist proposals
- Treat actual authentication and machine identities as separate future security work

## Direction

Remain independently useful for core recipe management while adding recipe history, drafts, general image support, and optional AI-assisted features. Manual and AI-generated changes should converge on a shared, user-controlled draft and publication lifecycle. Participate in the wider ecosystem through purposefully designed and authorized APIs.

## Ecosystem relationships

See [[System Overview]].

## Related initiatives

- [[Recipe Versioning and Drafts]] — planned
- [[AI-assisted Recipe Optimization]] — planned
- [[Recipe and Ingredient Images]] — planned
- [[Read-only Project Access for the Agent]] — planned
- [[Controlled Agent Actions]] — exploring

## Open questions

- Should future cooking recommendations use only saved recipes, or also an inventory of ingredients actually available at home?
- What form of human authentication should eventually replace or complement the current user selection?
