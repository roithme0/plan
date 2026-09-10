---
type: idea
status: exploring
last_reviewed: 2026-09-10
projects:
  - "[[AI Service]]"
  - "[[Kochwiki]]"
---

# Shared Agent Chat UI and Domain Rendering

## Direction

Explore the AI Service as a combination of two separately usable parts:

- A backend for model access, conversations, agent orchestration, structured outputs, and bounded tool calls
- A reusable chat UI that can be embedded into domain applications and can also become the interaction surface for the universal agent

This is a direction to investigate, not a decision about packaging, deployment, framework, or repository boundaries.

## Motivation

Kochwiki needs a focused AI editing conversation, while the longer-term universal agent needs a general conversation surface. Sharing the core chat experience could avoid separate implementations and make capabilities such as streaming, conversation history, tool status, and structured results consistent across applications.

At the same time, a generic chat UI must be able to present domain-specific results. A future universal agent should render a recipe as a recipe rather than reducing it to plain text, even when the conversation did not start inside Kochwiki.

## Tentative model

- The AI Service backend returns typed, structured artifacts in addition to conversational text.
- The shared chat UI understands the artifact envelope and conversation behavior without owning every domain representation.
- Domain-specific renderers may be registered or supplied separately.
- Kochwiki may embed the shared chat UI for recipe editing while retaining ownership of recipe data and lifecycle.
- The universal agent UI may use the same chat foundation and load a recipe renderer when it receives a recipe artifact.
- A generic fallback representation remains available when no specialized renderer exists.

For example, a recipe result could be identified as a typed artifact associated with the recipe version on which it is based. The exact schema is intentionally not defined here.

## Renderer ownership problem

The current Kochwiki frontend already owns the recipe presentation. Moving that rendering into the AI Service would couple a general platform to Kochwiki and risk duplicating domain UI.

A possible future approach is a domain-owned renderer package or component that can be consumed by both Kochwiki and the shared chat UI. A renderer registry could select it by artifact type. This remains only one option: distribution, version compatibility, framework interoperability, trust, and runtime discovery have not been resolved.

## Integration boundary

- Domain services remain authoritative for their data and validate all reads and writes.
- The agent receives access through a limited set of explicit domain tool calls rather than database, filesystem, or general administrative access.
- Tool capabilities should express domain intent, such as retrieving a recipe or creating a recipe draft.
- Concrete tool contracts, authentication, authorization, and audit behavior will be designed separately.
- Rendering an artifact does not itself grant permission to mutate the underlying domain object.

## Alternatives still open

- A shared Web Component embedded by domain frontends
- A framework-neutral UI package integrated at build time
- A separately hosted interface embedded through an isolated boundary
- Host-provided renderers, domain-owned renderer packages, or server-driven presentation
- A common chat core with distinct application-specific shells instead of one complete shared UI

## Open questions

- Which responsibilities belong to the reusable chat UI and which belong to each host application?
- Who owns the canonical recipe renderer?
- How are domain renderers discovered, distributed, versioned, and trusted?
- How can one renderer work both inside its domain application and in the universal agent?
- How should authentication and conversation context cross an embedding boundary?
- What generic fallback should be used for an unknown artifact type?
- Should the AI Service remain one repository and deployment, or contain separately released backend and UI packages?

## Next step

Use the first Kochwiki AI editing flow to identify the smallest reusable chat and artifact boundaries, while postponing a commitment to a renderer plugin model or deployment mechanism.
