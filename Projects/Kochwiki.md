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

The current backend prevents deletion of foodstuffs that are referenced by a recipe and enforces uniqueness for an exact name-and-brand combination. Authenticated multi-user ownership and change provenance are not yet represented.

## Near-term focus

- Complete the current review, refactoring, and code-quality cleanup
- Address relevant performance issues
- Define remembered-user behavior as part of a real authenticated session
- Implement [[Browser Authentication Foundation]] before enforcing multi-user ownership or exposing data to the AI Service
- Introduce recipe versioning and drafts before AI-assisted editing can persist proposals
- Treat human authentication and machine identities as separate security concerns

## Authentication direction

Kochwiki will authenticate browser users through an external OIDC provider under [[DEC-002 External OIDC Provider for Human Authentication]]. Microsoft Entra ID Free is the current preference, while provider selection and the choice between a PKCE-based SPA and a Backend-for-Frontend session remain open.

## Data ownership direction

See [[DEC-001 Shared Foodstuffs and Owned Recipes]].

- Foodstuffs are shared among authenticated users and do not have an exclusive owner.
- Foodstuff creation and the latest change remain attributable to authenticated users.
- Recipes are readable by all authenticated users but have an explicit owner who controls their mutation, deletion, drafts, and publication.
- Recipe history retains live references to shared foodstuffs, so foodstuff changes may alter historical presentation and nutrition calculations.
- AI interactions operate only on behalf of an authenticated user and do not receive broader access.

## Direction

Remain independently useful for core recipe management while adding recipe history, drafts, general image support, photo-assisted foodstuff nutrition entry, and optional AI-assisted features. Manual and AI-generated changes should converge on a shared, user-controlled draft and publication lifecycle. Participate in the wider ecosystem through purposefully designed and authorized APIs.

## Ecosystem relationships

See [[System Overview]].

## Related initiatives

- [[Browser Authentication Foundation]] — planned
- [[Recipe Versioning and Drafts]] — planned
- [[AI-assisted Recipe Optimization]] — planned
- [[Recipe and Ingredient Images]] — planned
- [[Read-only Project Access for the Agent]] — planned
- [[Controlled Agent Actions]] — exploring

- [[Photo-assisted Foodstuff Nutrition Entry]] — exploring

## Related ideas

- [[Foodstuff History and Duplicate Management]]

## Open questions

- Should future cooking recommendations use only saved recipes, or also an inventory of ingredients actually available at home?
- Should Kochwiki use a PKCE-based SPA or a Backend-for-Frontend session?
- Should recipe collaboration, ownership transfer, or copying another user's recipe be supported later?
