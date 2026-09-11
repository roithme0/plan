---
type: project
status: planned
last_reviewed: 2026-09-11
---

# AI Service

## Purpose

Provide shared AI capabilities and an agent-based interface across projects in the network.

## Current role

The service is planned and does not yet have a documented implementation role.

## Direction

Develop two logically distinct capability areas within one project:

- Bounded AI capabilities requested by other services, such as recipe optimization and image generation
- A universal agent that can answer questions, use tools, and progressively interact with authorized project APIs

Explore a backend and reusable chat UI as separately consumable parts of the AI Service. The backend could serve both embedded domain experiences and other applications directly, while a shared chat foundation could support Kochwiki AI editing and the longer-term universal agent. The packaging, deployment, and embedding model remain partly undecided; see [[Shared Agent Chat UI and Domain Rendering]].

Keep model- and provider-specific concerns behind stable service interfaces. Domain services continue to own their data, validation, mutations, and domain-specific rules.

## Initial implementation foundation

The service can begin independently of Kochwiki integration with a generic, ephemeral, multi-turn chat.

- Place the first model provider behind a provider-neutral adapter.
- Keep the initial backend stateless where practical by accepting the relevant active conversation history from the client.
- Do not require conversation persistence, listing, or resumption.
- Stream ordered text, tool lifecycle, typed artifact, error, cancellation, and completion events rather than designing the API around one final text value.
- Support multiple tool calls and artifacts within one assistant turn.
- Generate identifiers, ordering, timestamps, references, and other deterministic application metadata in service code rather than through the model.
- Validate structured model and tool output at the service boundary.
- Keep tool orchestration generic so later domain tools use the same loop.
- Keep an unauthenticated development version local or otherwise inaccessible until the authentication and authorization boundaries exist.

A practical sequence is to implement the model adapter and general chat first, then streaming and cancellation, the generic tool loop, typed artifacts, skill selection, and finally the first domain-specific recipe proposal capability.

## Chat UI direction

The AI Service may own a reusable chat UI package without requiring it to be a separately hosted application. The shared UI should own generic conversation behavior and expose a renderer registry or equivalent host-extension point for typed domain artifacts. Host applications retain and inject their domain components; for example, Kochwiki owns the recipe proposal renderer used by [[AI-assisted Recipe Optimization]].

## Ecosystem relationships

See [[System Overview]].

## Related initiatives

- [[AI-assisted Recipe Optimization]] — planned
- [[Recipe and Ingredient Images]] — planned
- [[Universal Agent Foundation]] — planned
- [[Read-only Project Access for the Agent]] — planned
- [[Controlled Agent Actions]] — exploring

## Open questions

- How should bounded AI calls and stateful agent operations be separated within the service?
- Through which interface or interfaces will users initially interact with the agent?
- Which frontend packaging mechanism should be used for the first embedded chat UI?
- How should domain-specific renderers later be distributed and reused outside their host applications?
- Which model providers and deployment modes should the service support first?
