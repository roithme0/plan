---
type: initiative
status: planned
last_reviewed: 2026-09-10
projects:
  - "[[Kochwiki]]"
---

# Recipe Versioning and Drafts

## Intended outcome

A user can update a recipe while retaining its previous state in history, or keep proposed changes separate as a draft until they are accepted or discarded.

## Motivation

Kochwiki already supports manual recipe editing through its recipe edit dialog, but updates currently replace the stored recipe directly. A shared versioning and draft model should make changes recoverable and reviewable while providing the foundation for both manual and AI-assisted editing.

## Current state

- The Angular frontend opens a full-screen recipe edit dialog from the recipe page.
- The editor loads the current recipe and submits its fields through the existing recipe update flow.
- The backend applies updates directly through `PATCH /recipes/{recipe_id}`.
- The current persistence model has one mutable recipe record with related ingredients and preparation steps; it has no version or draft entity yet.

## Scope

- Preserve prior active recipe states as immutable history when an update is published
- Let a manual edit either update the active recipe immediately or be saved as a draft
- Keep the current active version unchanged while a draft exists
- Let a user accept a draft as the new active version or discard it
- Show a draft as a diff against the recipe version that is currently active
- Use the same draft lifecycle for manual and AI-generated changes
- Keep the first user interface deliberately small and consistent with the existing mobile-first edit flow

## Out of scope

- Detailed service-internal data models or API contracts
- A final visual design for history, draft, or diff views
- Allowing an agent to publish an active version or delete recipe data
- Resolving all concurrent-editing behavior at the ecosystem-planning level

## Project roles

- [[Kochwiki]] owns active recipes, immutable history, drafts, validation, diff presentation, acceptance, discard, and persistence.
- [[AI Service]] may propose recipe content, but enters this lifecycle only by asking Kochwiki to create a new draft through a constrained capability.

## Dependencies

- Version and draft semantics defined in the Kochwiki service repository
- Validation that applies consistently to direct updates and draft acceptance
- A minimal way to compare structured recipe fields, ordered ingredients, and ordered preparation steps

## Open questions

- May a recipe have multiple drafts at the same time?
- How should a draft behave when the active recipe changes after the draft was created?
- Should an accepted or discarded draft remain identifiable in history?
- Should the initial diff be field-oriented, recipe-oriented, or combine both?
- What provenance should distinguish manual and AI-generated drafts?

## Next step

Define the version and draft lifecycle in Kochwiki, including publication, acceptance, discard, history, and stale-draft behavior, before choosing the persistence model and UI details.
