---
type: initiative
status: planned
last_reviewed: 2026-09-10
projects:
  - "[[Kochwiki]]"
  - "[[AI Service]]"
---

# AI-assisted Recipe Optimization

## Intended outcome

A user can iteratively improve a recipe in a focused AI editing chat, understand the proposed changes, and save a chosen proposal as a reviewable draft without giving the AI permission to publish or delete recipe data.

## Motivation

Use shared AI capabilities to improve recipes while keeping exploration conversational, changes explainable, and control over persistence and publication with the user and Kochwiki.

## Experience direction

- A user starts AI editing from the recipe view.
- The user may open an empty chat and write the first request, or start from a predefined action such as optimizing for macronutrients or gut health.
- A predefined action sends an automatically generated first message and produces an initial proposal.
- The chat presents each proposed recipe with a visual treatment consistent with the recipe view and explains the changes.
- The user may request further changes, ask questions, or return to an earlier proposal.
- A proposal remains part of the chat and is not automatically stored as a draft or active recipe version.
- When satisfied, the user may ask to save the current or an earlier proposal as a new draft.

The exact chat and proposal UI remains open. The first implementation should be deliberately simple and mobile-first.

## Scope

- Start recipe-scoped AI editing through free-form chat or a small set of predefined optimization actions
- Provide the active recipe and relevant constraints as context for the editing conversation
- Return complete, structured recipe proposals with explanations
- Support iterative refinement and selection of a current or earlier proposal
- Visualize proposals in the chat without persisting them automatically
- Create a Kochwiki draft only after an explicit user request
- Reuse [[Recipe Versioning and Drafts]] for validation, diff review, acceptance, and discard
- Preserve core recipe management when the AI Service is unavailable

## Safety boundary

- The AI Service has no direct access to the Kochwiki database.
- The AI may request creation of a new draft through a bounded Kochwiki capability.
- The AI may not overwrite or publish the active recipe version.
- The AI may not accept or discard drafts and may not delete recipes, versions, drafts, or chat proposals.
- Kochwiki validates every proposal before persisting it as a draft.

## Out of scope

- Autonomous acceptance or publication of recipe changes
- Autonomous deletion of recipe data
- Treating every intermediate chat proposal as a persisted recipe draft
- Choosing a specific model or provider at the ecosystem-planning level
- Finalizing the detailed chat interface in this initiative

## Project roles

- [[Kochwiki]] owns recipe data, draft and version history, proposal validation, diff presentation, acceptance, discard, and persistence.
- [[AI Service]] manages the editing conversation and produces explainable, structured recipe proposals through a stable service interface.

## Dependencies

- [[Recipe Versioning and Drafts]]
- An authenticated service-to-service interface
- A bounded draft-creation capability in Kochwiki
- A recipe proposal format that Kochwiki can validate and render
- A way to associate chat proposals with the recipe version used as their starting point

## Open questions

- Which predefined optimization actions should the first version support?
- Should editing chats and intermediate proposals persist across sessions?
- How should the UI identify and restore an earlier proposal in a conversation?
- What provenance should be retained with the chat, proposal, and resulting draft?
- How should the AI handle an active recipe that changes during an editing conversation?

## Next step

Define the first recipe-editing chat flow and its proposal contract after the version and draft lifecycle exists in Kochwiki.
