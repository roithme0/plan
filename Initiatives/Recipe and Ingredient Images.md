---
type: initiative
status: planned
last_reviewed: 2026-08-30
projects:
  - "[[Kochwiki]]"
  - "[[AI Service]]"
---

# Recipe and Ingredient Images

## Intended outcome

Kochwiki can associate images with recipes and ingredients, including images generated through the AI Service.

## Motivation

Improve recognition and presentation of recipes and ingredients while keeping image generation reusable and provider-independent.

## Scope

- Implement general image support in Kochwiki before AI generation
- Support image ownership, metadata, access, replacement, and deletion as Kochwiki concerns
- Request generated images through a bounded AI Service capability
- Keep uploaded, imported, and generated images compatible with the same domain model

## Out of scope

- Making AI generation a prerequisite for ordinary image support
- Embedding a specific image provider into Kochwiki
- Selecting the final storage technology in this workspace

## Project roles

- [[Kochwiki]] owns image associations, metadata, access policy, and storage lifecycle.
- [[AI Service]] generates images and hides provider-specific integration details.

## Dependencies

- General recipe and ingredient image support in Kochwiki
- A defined transfer contract between the two services
- An image-generation capability in the AI Service

## Open questions

- Which metadata and provenance should be retained for generated images?
- How should generated image results be transferred to Kochwiki?

## Next step

Define the general media model and lifecycle within Kochwiki without assuming an AI source.
