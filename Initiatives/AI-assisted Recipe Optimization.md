---
type: initiative
status: planned
last_reviewed: 2026-09-11
projects:
  - "[[Kochwiki]]"
  - "[[AI Service]]"
---

# AI-assisted Recipe Optimization

## Intended outcome

A user can iteratively improve one existing recipe in a focused AI editing chat, understand and compare one or more proposed alternatives, and save any chosen proposal as a reviewable draft without giving the AI permission to publish or delete recipe data.

## Motivation

Use shared AI capabilities to improve recipes while keeping exploration conversational, changes explainable, and control over persistence and publication with the user and Kochwiki.

## Experience direction

- A user starts AI editing from the view of one existing recipe.
- The user may open an empty chat and write the first request, or start from a predefined action such as optimizing for macronutrients or gut health.
- The assistant may answer questions or ask for clarification without producing a recipe proposal.
- A broad request such as "improve this recipe" should normally be clarified before producing a proposal.
- When the assistant proposes a changed recipe, it returns the complete recipe rather than a patch or an incomplete set of changes.
- One assistant turn may contain multiple complete proposals, for example when the user requests alternatives or different degrees of an optimization.
- Each proposal is shown inline through a visual treatment consistent with the Kochwiki recipe view and remains available in the session's chat history.
- The user may request further changes based on the latest proposal, explicitly continue from an earlier proposal, or return to the original recipe.
- A proposal remains an ephemeral chat artifact and is not automatically stored as a draft or active recipe version.
- The user can save any proposal as a draft through an action on its recipe card.
- The user can also explicitly ask the agent to save an unambiguously identified proposal as a draft.

The first implementation should be deliberately simple and mobile-first. It may use a shared AI Service chat foundation embedded in Kochwiki, but this initiative does not move ownership of recipe presentation into the AI Service.

## Session model

- A session is scoped to exactly one original recipe and its stable source version.
- Consecutive messages, questions, refinements, and multiple proposals are supported while the session is open.
- Persisting, listing, reopening, or reading previous chats is not required initially.
- Reloading or closing the chat may discard the conversation and all proposals that have not been saved as Kochwiki drafts.
- The initial AI Service should remain stateless where practical: the client retains the active conversation and supplies the relevant history with each request.
- The service may limit retained context or tool execution for operational safety, but there is no product rule restricting an assistant turn to one proposal.

## Proposal model

- Proposals are immutable, complete, structured recipe artifacts.
- Every proposal has a service-generated identifier, deterministic order within the turn, and an explicit base reference.
- The base reference points to the original recipe version or to another proposal in the same session.
- Service-generated metadata such as identifiers, timestamps, ordering, and references is never invented by the model.
- The default basis for a refinement is the most recently selected or produced proposal.
- Kochwiki provides a deterministic action such as "Continue from here" on every proposal so that an exact base can be selected without relying on natural-language resolution.
- Natural-language references such as "the second proposal" may also be supported, but ambiguous references must result in clarification rather than an assumed selection.
- All proposals remain visible in the linear chat history even when later proposals branch from an earlier one.
- Saving a proposal creates a Kochwiki draft; it does not change or remove the chat artifact.

## Skills and proposal creation

- Optimization directions are implemented as focused, versioned skills or equivalent internal capability modules in the AI Service.
- A skill supplies instructions, constraints, evaluation criteria, and strategies for a direction such as reducing calories, increasing protein, or improving gut friendliness.
- The service selects one or more relevant skills from the user's request; a request does not have to produce a recipe proposal.
- Applied skill identifiers and versions should be retained as proposal provenance.
- The model is responsible for conversational and recipe content, while deterministic application metadata and lifecycle behavior remain in service code.
- The agent submits structured recipe candidates through a proposal tool rather than encoding them only in conversational prose.
- The proposal tool has no persistence side effect. It validates and emits one complete proposal artifact, and the model may call it multiple times in one assistant turn.

## Conversation and artifact boundary

The chat response is a stream of ordered content and lifecycle events rather than a single text value. It should be able to represent:

- Conversational text deltas
- Tool-call lifecycle and failures
- One or more typed proposal artifacts
- Completion and cancellation

Provider-specific streaming and tool-call details remain behind a stable AI Service interface. The initial implementation should support client cancellation and make model or tool failures visible without corrupting already completed content.

## Draft creation

Both the proposal card action and an explicit conversational request use the same bounded draft-creation capability.

- The user-facing UI may save a proposal directly through an action on its recipe card.
- The agent may call a draft-saving tool only after an explicit user request.
- The draft-saving tool takes the unambiguous service-generated proposal identifier, not a model-reconstructed recipe.
- The service resolves the proposal content and source reference deterministically.
- If a natural-language reference cannot be resolved unambiguously, the assistant asks the user to select or clarify the proposal.
- Kochwiki authorizes and validates the complete proposal before creating the draft.
- Draft acceptance, publication, discard, and deletion remain unavailable to the AI.

## Chat UI and recipe rendering

The AI Service may provide a reusable chat UI or chat UI package that owns generic conversation behavior, streaming, tool status, and typed artifact slots. Kochwiki retains the domain-specific recipe presentation.

- The AI Service defines a versioned artifact envelope and a renderer registry or equivalent host-extension point.
- Kochwiki registers its own recipe proposal component for the Kochwiki recipe artifact type.
- The shared chat UI does not import or own Kochwiki recipe code.
- Kochwiki's renderer provides domain actions such as "Continue from here" and "Save as draft".
- An unknown artifact type has a generic fallback representation.
- For the first Kochwiki integration, a build-time UI package or framework-compatible integration is preferred over a separately hosted iframe, because the host must inject its domain renderer.
- Reusing the same renderer in a future universal-agent UI may later require a separately consumable Kochwiki renderer package, but that is not required for the first implementation.

## Scope

- Start recipe-scoped AI editing through free-form chat or a small set of predefined optimization actions
- Provide the original recipe snapshot, its stable version reference, and relevant constraints as context
- Support an ephemeral multi-turn editing session without chat persistence
- Return complete, structured recipe proposals with explanations
- Support multiple proposals in one assistant turn
- Support iterative refinement and deterministic selection of the original recipe or any earlier proposal as the next basis
- Visualize all session proposals in the chat without persisting them automatically
- Create a Kochwiki draft only through an explicit UI action or unambiguous user request
- Reuse [[Recipe Versioning and Drafts]] for validation, diff review, acceptance, and discard
- Preserve core recipe management when the AI Service is unavailable

## Safety boundary

- Recipe editing conversations are available only to authenticated users.
- The AI acts on behalf of the authenticated user and is limited by that user's recipe permissions.
- The AI Service has no direct access to the Kochwiki database.
- A conversation grants context for only its original recipe, not access to search, compare, or retrieve other recipes.
- The AI may request creation of a new draft through a bounded Kochwiki capability.
- The AI may not overwrite or publish the active recipe version.
- The AI may not accept or discard drafts and may not delete recipes, versions, drafts, or chat proposals.
- Kochwiki validates every proposal before persisting it as a draft.
- Rendering a recipe proposal does not grant the chat UI or agent additional mutation rights.

## Out of scope

- Autonomous acceptance or publication of recipe changes
- Autonomous deletion of recipe data
- Creating an unrelated new recipe within the editing chat
- Searching, retrieving, or comparing other recipes
- Treating intermediate chat proposals as persisted recipe drafts
- Persisting, listing, or resuming editing chats
- Choosing a specific model or provider at the ecosystem-planning level
- Finalizing the detailed visual design of the chat interface
- Making the Kochwiki recipe renderer available to the universal agent in the first implementation

## Project roles

- [[Kochwiki]] owns recipe data, recipe rendering, draft and version history, proposal validation, diff presentation, acceptance, discard, and persistence.
- [[AI Service]] owns the editing conversation, model abstraction, streaming, skill application, tool orchestration, and the production of explainable structured recipe proposals through a stable service interface.
- The reusable chat UI owns generic conversation presentation and exposes a host extension point for typed domain artifacts.
- Ownership of a reusable recipe renderer outside Kochwiki remains an open architectural question.

## Dependencies

- [[Recipe Versioning and Drafts]] before an AI proposal can be persisted
- Reliable user authentication and delegated user context before integration is exposed
- An authenticated service-to-service interface
- A bounded draft-creation capability in Kochwiki
- A versioned recipe proposal format that Kochwiki can validate and render
- Stable references for the original recipe version and session proposals

The generic AI Service chat, model adapter, streaming, tool loop, and artifact envelope can be implemented before these Kochwiki integration dependencies are complete.

## Initial AI Service sequence

1. Introduce a provider-neutral model adapter and a minimal general chat.
2. Add streaming, cancellation, and explicit error events.
3. Add a generic tool-call loop that supports multiple tool calls per assistant turn.
4. Introduce typed content blocks and artifact events.
5. Add skill selection and provenance.
6. Add the recipe proposal tool.
7. Integrate the shared chat UI, Kochwiki renderer, and bounded draft creation.

## Open questions

- Which predefined optimization skills should the first version support?
- How should active conversation context be bounded as a session grows?
- What exact recipe proposal schema and skill provenance must cross the service boundary?
- How should the system react when the active Kochwiki recipe changes during an open editing session?
- Which frontend packaging mechanism best supports the initial renderer registry?
- Can Kochwiki and the universal agent later share a recipe renderer without duplicating domain UI or coupling the AI Service to Kochwiki?

## Next step

Begin the generic AI Service foundation with a provider-neutral model adapter and a streamed, ephemeral, multi-turn chat protocol designed for multiple tool calls and typed artifacts. Define the concrete recipe proposal contract and Kochwiki renderer integration afterward.
