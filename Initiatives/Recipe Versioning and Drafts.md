---
type: initiative
status: active
last_reviewed: 2026-09-29
projects:
  - "[[Kochwiki]]"
---

# Recipe Versioning and Drafts

## Intended outcome

A recipe owner can update a recipe while retaining its previous state in history, or keep proposed changes separate as a draft until they are accepted or discarded.

## Motivation

Kochwiki now supports recipe versions and drafts. This initiative tracks the remaining review experience and ownership boundary while the implemented lifecycle provides a foundation for manual and AI-assisted editing.

## Current state

- On Kochwiki v2's `staging` branch, the backend stores active, draft, and historical recipe versions under a lineage. Publishing an active edit or a draft moves the prior active version into history.
- The API supports creating and updating drafts, publishing or discarding them, and listing historical versions. Multiple drafts can coexist; publishing one leaves the others intact.
- The Angular editor offers publishing an active edit or saving a draft. Recipe pages show active and draft states and provide draft publish/discard actions. AI proposals can also be saved explicitly as separate drafts.
- Foodstuff deletion is blocked while any ingredient in a retained recipe version references it.
- Historical versions are available through the API, but the current recipe-page workflow does not expose a history browser or a draft-versus-active diff.
- User selection is still temporary; authentication, recipe ownership, and owner-only authorization are not implemented in this workflow.

## Scope

- Preserve prior active recipe states as immutable history when an update is published
- Let an owner either update the active recipe immediately or save the manual edit as a draft
- Keep the current active version unchanged while a draft exists
- Let the owner accept a draft as the new active version or discard it
- Show a draft as a diff against the recipe version that is currently active
- Use the same draft lifecycle for manual and AI-generated changes
- Retain references from active recipes, drafts, and historical versions to shared foodstuffs
- Extend foodstuff deletion checks to every retained recipe reference
- Keep the first user interface deliberately small and consistent with the existing mobile-first edit flow

## Ownership and access

- Every recipe has an explicit owner under [[DEC-001 Shared Foodstuffs and Owned Recipes]].
- All authenticated users may read active recipes.
- Only the recipe owner may directly edit or delete the recipe, accept or discard its drafts, and publish a new active version.
- Drafts and historical versions inherit authorization from their recipe rather than receiving independent ownership.
- AI-generated draft creation runs on behalf of the authenticated owner and grants the AI no independent permission.

## Historical foodstuff references

Recipe versions preserve their selected foodstuff references and ingredient quantities, but do not snapshot foodstuff fields or nutrition values. A later change to a shared foodstuff may therefore change the name, unit, or calculated nutrition shown for an historical recipe version. This is accepted for the current product stage.

## Out of scope

- Detailed service-internal data models or API contracts
- A final visual design for history, draft, or diff views
- Allowing an agent to publish an active version or delete recipe data
- Resolving all concurrent-editing behavior at the ecosystem-planning level
- Versioning foodstuffs or detecting semantic duplicates

## Project roles

- [[Kochwiki]] owns active recipes, ownership enforcement, history, drafts, validation, diff presentation, acceptance, discard, and persistence.
- [[AI Service]] may propose recipe content, but enters this lifecycle only by asking Kochwiki to create a new draft through a constrained capability on behalf of an authenticated owner.

## Dependencies

- Reliable user authentication and recipe ownership in Kochwiki
- Version and draft semantics defined in the Kochwiki service repository
- Validation that applies consistently to direct updates and draft acceptance
- A minimal way to compare structured recipe fields, ordered ingredients, and ordered preparation steps
- Referential-integrity checks covering active recipes, drafts, and historical versions

## Open questions

- May a recipe have multiple drafts at the same time?
- How should a draft behave when the active recipe changes after the draft was created?
- Should an accepted or discarded draft remain identifiable in history?
- Should the initial diff be field-oriented, recipe-oriented, or combine both?
- What provenance should distinguish manual and AI-generated drafts?
- Should collaboration, ownership transfer, or copying another user's recipe be supported later?

## Next step

Add the draft-versus-active comparison and history access to the Kochwiki UI. Revisit stale-draft behavior and apply authenticated recipe ownership and authorization before treating the owner-only rules above as enforced.
