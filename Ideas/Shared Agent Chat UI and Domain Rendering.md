---
type: idea
status: exploring
last_reviewed: 2026-09-29
projects:
  - "[[AI Service]]"
  - "[[Kochwiki]]"
---

# Shared Agent Chat UI and Domain Rendering

## Direction

Explore the AI Service as a combination of two separately usable parts:

- A backend for model access, ephemeral or persistent conversations, agent orchestration, streaming, structured outputs, and bounded tool calls
- A reusable chat UI that can be embedded into domain applications and can also become the interaction surface for the universal agent

The first integration now uses an Angular library owned by the AI Service repository and consumed by Kochwiki at build time. It is published through GitHub Packages, with release automation and a local linking workflow. The broader framework and deployment boundaries remain exploratory.

## Motivation

Kochwiki needs a focused AI editing conversation, while the longer-term universal agent needs a general conversation surface. Sharing the core chat experience could avoid separate implementations and make capabilities such as streaming, conversation history, cancellation, tool status, and structured results consistent across applications.

At the same time, a generic chat UI must be able to present domain-specific results. A future universal agent should render a recipe as a recipe rather than reducing it to plain text, even when the conversation did not start inside Kochwiki.

## Implemented first integration

On the `staging` branches, the shared `@roithme0/chat-ui` package offers separate UI and conversation entry points. Kochwiki imports it for recipe improvement and registers a host-owned Angular template for the `kochwiki-recipe` artifact type. That template uses Kochwiki's shared recipe presentation for the original snapshot and proposals, with a Kochwiki-owned save-as-draft action on proposals. The library frames artifacts and falls back to a generic JSON view for unknown types; its conversation controller handles completed-turn HTTP requests and recovery.

The first integration does not stream responses or support cancellation. It has not established a portable domain renderer for a future universal agent.

## Content and artifact model

An assistant turn is an ordered stream of content and lifecycle events rather than a single text response. It may contain conversational text, tool status, errors, and multiple typed artifacts.

- The AI Service backend returns typed, versioned artifact envelopes in addition to conversational text.
- Service code assigns deterministic artifact identifiers, ordering, timestamps, and references.
- The model supplies organic content and structured domain candidates but does not invent application identity or lifecycle metadata.
- One assistant turn may contain several artifacts of the same type.
- The shared chat UI understands the artifact envelope and conversation behavior without owning every domain representation.
- A generic fallback representation remains available when no specialized renderer exists.

For example, a recipe proposal can be identified as a typed artifact associated with the recipe version or earlier proposal on which it is based. The exact domain schema belongs to the corresponding integration.

## Renderer registry

The shared chat UI should expose a renderer registry or equivalent host-extension point.

- A host application associates an artifact type with a domain-owned component.
- The chat UI selects the registered renderer when displaying an artifact.
- The renderer receives validated artifact data and a deliberately narrow set of host actions.
- Rendering an artifact does not grant permission to mutate the underlying domain object.
- Unknown artifacts use the generic fallback.

For the first [[AI-assisted Recipe Optimization]] integration, Kochwiki retains its recipe proposal component and registers it with the embedded chat UI. The recipe renderer provides domain-specific actions such as selecting a proposal as the basis for the next request or saving it as a draft. The AI Service does not import or own Kochwiki recipe code.

## Packaging direction for the first integration

The first Kochwiki integration uses a reusable Angular library owned by the AI Service repository and consumed at build time. Kochwiki supplies its proposal renderer to this UI library through a typed extension point; no renderer is supplied to the backend. A separately hosted iframe is not the initial direction because it would make direct reuse of a host-owned Angular component difficult and introduce cross-boundary messaging and duplicated state.

The library exposes narrow `/ui` and `/conversation` public entry points. GitHub Packages publication and release automation are established, and a local link supports cross-repository development.

This first-integration decision does not yet decide whether the long-term shared UI evolves into:

- A framework-neutral core with host adapters
- A Web Component with suitable renderer extension points
- A common chat core used by distinct application-specific shells

The fact that the chat UI belongs to the AI Service describes product and source responsibility; it does not require the UI to be a separately hosted application.

## Kochwiki first use

- The AI Service retains initialization snapshots, active conversation state, proposals, lineage, and provenance in a short-lived session; Kochwiki does not provide durable chat persistence.
- The AI Service chat package owns generic conversation presentation, artifact placement, and completed-turn conversation control. Streaming, cancellation, and tool-status presentation remain future work.
- Kochwiki registers the recipe proposal renderer and supplies bounded domain actions.
- All proposals remain visible in the linear chat history.
- Proposal lineage may reference the original recipe version or an earlier proposal without changing the linear presentation.
- A future universal agent may initially use the generic fallback or link to Kochwiki when no recipe renderer is installed.

## Renderer ownership problem

The current Kochwiki frontend already owns recipe presentation. Moving that rendering into the AI Service would couple a general platform to Kochwiki and risk duplicating domain UI.

If the universal agent later needs the same rich recipe rendering, a possible approach is a separately consumable, domain-owned Kochwiki renderer package. A renderer registry could load it by artifact type. Distribution, version compatibility, framework interoperability, trust, and runtime discovery remain unresolved and are not prerequisites for the first Kochwiki integration.

## Integration boundary

- Domain services remain authoritative for their data and validate all reads and writes.
- The agent receives access through a limited set of explicit domain tool calls rather than database, filesystem, or general administrative access.
- Tool capabilities express domain intent, such as retrieving a recipe or creating a recipe draft.
- The UI and conversational agent may invoke the same bounded domain action through different user interactions.
- Concrete tool contracts, authentication, authorization, confirmation, and audit behavior are designed with the corresponding initiative.
- A domain mutation tool should prefer deterministic artifact references over model-reconstructed domain payloads.

## Alternatives still open

- A shared Web Component embedded by domain frontends
- A framework-neutral UI package integrated at build time
- A common headless chat core with framework-specific adapters
- A separately hosted interface for hosts that do not require injected domain components
- Host-provided renderers, domain-owned renderer packages, or server-driven presentation
- A common chat core with distinct application-specific shells instead of one complete shared UI

## Open questions

- Which responsibilities belong to the reusable chat UI and which belong to each host application?
- How are independently distributed domain renderers versioned and trusted?
- How can one renderer later work both inside its domain application and in the universal agent?
- How should authentication and conversation context cross a separately hosted embedding boundary if one is introduced?
- What generic fallback should be used for an unknown artifact type?
- Should the AI Service remain one repository and deployment, or contain separately released backend and UI packages?

## Next step

Use the implemented Kochwiki flow to assess whether the host template and artifact contract are sufficient for another domain. Define how a future universal-agent UI could reuse rich recipe rendering without coupling the shared package to Kochwiki; treat streaming and cancellation as separate additions.
