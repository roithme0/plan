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
- The user starts with the initial gut-health improvement direction for a person with diagnosed irritable bowel syndrome (`Reizdarm`). Its detailed optimization criteria are defined separately before implementation.
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
- The AI Service retains the active conversation, initialization snapshots, validated proposals, lineage, and capability provenance as short-lived session state.
- The service may limit retained context or tool execution for operational safety, but there is no product rule restricting an assistant turn to one proposal.
- Session expiry is explicit to the caller. Durable history, reopening, and recovery of expired proposals are not required initially.

## Foodstuff boundary

- Kochwiki supplies the complete source-recipe snapshot and a snapshot of all foodstuffs currently available for proposals when it initializes the session.
- Proposal ingredients reference foodstuffs from that snapshot by opaque external identifier.
- The AI Service rejects a proposal that references a foodstuff outside the supplied snapshot.
- The snapshot contains only the fields required for selection and explanation; Kochwiki remains authoritative for the catalogue.
- A bounded foodstuff search or lookup tool is a possible later iteration when supplying the full catalogue no longer fits the configured context budget.
- Proposing, resolving, or creating foodstuffs that are not already available in Kochwiki is not part of this initiative.

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
- The initial and only direction is gut-health recipe improvement for a person with diagnosed irritable bowel syndrome (`Reizdarm`). Dietary exclusions, calorie targets, protein targets, and other optimization directions are not initial candidates.
- The concrete nutritional signals, symptom profiles, protocols, scoring rules, and deterministic criteria are deliberately deferred to separate evidence-informed work.
- The capability may suggest recipe changes within those eventual criteria, but must not diagnose, treat, or claim to improve the underlying condition.
- A request does not have to produce a recipe proposal; the assistant may clarify the user's intent first.
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

The AI Service provides a reusable Angular chat UI library that owns generic conversation behavior, streaming, tool status, and typed artifact slots. Kochwiki consumes it at build time and retains the domain-specific recipe presentation.

- The AI Service defines a versioned artifact envelope and a renderer registry or equivalent host-extension point.
- Kochwiki registers its own recipe proposal component for the Kochwiki recipe artifact type.
- The shared chat UI does not import or own Kochwiki recipe code.
- Kochwiki's renderer provides domain actions such as "Continue from here" and "Save as draft".
- An unknown artifact type has a generic fallback representation.
- Kochwiki supplies its renderer to the chat UI through a typed host extension point; it does not supply a renderer to the backend.
- A build-time Angular library is confirmed for the first Kochwiki integration. A separately hosted iframe is not the initial direction because the host must inject its domain renderer and bounded actions.
- The library keeps a narrow public API and does not expose internal backend DTOs or private chat implementation details.
- Package registry, publication automation, release workflow, and local cross-repository development mechanics are deferred until Kochwiki consumption is imminent.
- Reusing the same renderer in a future universal-agent UI may later require a separately consumable Kochwiki renderer package, but that is not required for the first implementation.

## Scope

- Start recipe-scoped AI editing through free-form chat or a small set of predefined optimization actions
- Provide the original recipe snapshot, its stable version reference, and relevant constraints as context
- Provide all currently available Kochwiki foodstuffs as a session-initialization snapshot
- Support an ephemeral multi-turn editing session held as short-lived AI Service state without durable chat persistence
- Restrict proposals to foodstuffs in the supplied snapshot
- Initially support only the versioned IBS-focused gut-health improvement direction
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
- Foodstuff lookup tools and proposing or creating unavailable foodstuffs
- Dietary exclusions or optimization directions other than IBS-focused gut health
- Defining the detailed IBS-focused optimization criteria in this initiative
- Handling source-recipe changes during an open session

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

- What evidence-informed definition should govern the IBS-focused gut-health direction?
- Which eventual optimization criteria are deterministic, model-judged, or explanatory only?
- What minimum recipe and foodstuff fields are required for useful and valid proposals?
- At what catalogue-size or token-budget threshold should the upfront snapshot be replaced or supplemented by lookup tools?
- How should active conversation context be bounded as a session grows?
- How long should short-lived session state survive, and should activity extend its expiry?
- What exact recipe proposal schema and skill provenance must cross the service boundary?
- What generic renderer interface, fallback representation, and bounded host actions are sufficient for the first proposal artifact version?
- Which package registry and release workflow should distribute the Angular library when integration becomes imminent?
- Can Kochwiki and the universal agent later share a recipe renderer without duplicating domain UI or coupling the AI Service to Kochwiki?

## Next step

Define the evidence-informed IBS-focused optimization criteria and the minimum recipe, foodstuff-snapshot, proposal, session, and renderer contracts. The generic AI Service foundation can proceed in parallel with a provider-neutral model adapter and a streamed, short-lived, multi-turn chat protocol designed for multiple tool calls and typed artifacts.
