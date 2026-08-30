---
type: initiative
status: planned
last_reviewed: 2026-08-30
projects:
  - "[[Kochwiki]]"
  - "[[AI Service]]"
---

# AI-assisted Recipe Optimization

## Intended outcome

A user can request an AI-assisted improvement to a recipe, review the proposed changes, and accept them as a new recipe version without losing the original.

## Motivation

Use shared AI capabilities to improve recipes while keeping changes explainable, reviewable, and owned by Kochwiki.

## Scope

- Introduce recipe versioning in Kochwiki before integrating optimization
- Request recipe optimization through a bounded AI Service capability
- Return a proposal rather than modifying Kochwiki directly
- Allow Kochwiki to validate and present proposed changes
- Create a new recipe version only after acceptance
- Preserve core recipe management when the AI Service is unavailable

## Out of scope

- Direct AI access to the Kochwiki database
- Autonomous acceptance of recipe changes
- Choosing a specific model or provider at the ecosystem-planning level

## Project roles

- [[Kochwiki]] owns recipe data, version history, review, validation, and persistence.
- [[AI Service]] produces an optimization proposal through a stable service interface.

## Dependencies

- A reviewable recipe-versioning model in Kochwiki
- An authenticated service-to-service interface
- A bounded recipe-optimization capability in the AI Service

## Open questions

- Which kinds of optimization should the first version support?
- What provenance should be retained with an AI-generated proposal or accepted version?

## Next step

Define and implement recipe-versioning semantics within Kochwiki independently of AI generation.
