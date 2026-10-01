---
type: project
status: active
last_reviewed: 2026-09-29
---

# AI Service

## Purpose

Provide shared AI capabilities and an agent-based interface across projects in the network.

## Current role

On the `staging` branch, the service runs a generic session-based conversation API with an OpenAI-backed recipe-improvement agent and a deterministic demo agent. It owns bounded proposal-tool orchestration, validation, short-lived conversation state, and typed artifacts. Kochwiki v2 uses this service for recipe-scoped improvement through a restricted gateway route.

The service also publishes the reusable Angular `@roithme0/chat-ui` package. Kochwiki consumes its UI and conversation entry points and supplies a recipe artifact renderer. A universal agent for generic questions or live weather is not implemented. Streaming, cancellation, durable chat history, and production-grade cross-service authorization remain open.

## Direction

Develop two logically distinct capability areas within one project:

- Bounded AI capabilities requested by other services, such as recipe optimization and image generation
- A universal agent that can answer questions, use tools, and progressively interact with authorized project APIs

The backend and Angular chat package are separately consumable in the first Kochwiki integration. A future universal agent can build on the same conversation and tool boundaries. Longer-term framework-neutral embedding and cross-application renderer reuse remain open; see [[Shared Agent Chat UI and Domain Rendering]].

Keep model- and provider-specific concerns behind stable service interfaces. Domain services continue to own their data, validation, mutations, and domain-specific rules.

## Foundation and remaining direction

- The backend retains source context, messages, and proposal artifacts in process-local sessions. Completed turns can produce several validated artifacts; there is no database-backed chat history.
- A provider adapter and bounded tool loop are implemented for recipe improvement. Service code assigns artifact identifiers, order, timestamps, and base references.
- The Angular package exposes separate `/ui` and `/conversation` entry points. Kochwiki registers its own Angular recipe template, and unknown artifact types have a generic fallback.
- The next foundation work includes streaming and cancellation, more explicit tool-status presentation, and the general model-backed agent with one live weather tool.
- Kochwiki and the AI Service currently operate within a private-network boundary. Authentication, delegated user context, and service authorization are required before exposing the integration more broadly.

## Home Assistant connector direction

Use the official Home Assistant MCP Server through the generic MCP runtime described in [[Recipe Agent Capability and Proposal Outline]], rather than introducing Home Assistant domain logic into the runtime. [[Home Assistant]] records the available surface and planned rollout: selected state access first, controlled actions later. This connector is planned; current implementation is not asserted here.

## Ecosystem relationships

See [[System Overview]].

## Related initiatives

- [[AI-assisted Recipe Optimization]] — active
- [[Recipe and Ingredient Images]] — planned
- [[Universal Agent Foundation]] — planned
- [[Read-only Project Access for the Agent]] — planned
- [[Controlled Agent Actions]] — exploring
- [[Photo-assisted Foodstuff Nutrition Entry]] — exploring

## Open questions

- How should bounded AI calls and stateful agent operations be separated within the service?
- Through which interface or interfaces will users initially interact with the agent?
- How should domain-specific renderers later be distributed and reused outside their host applications?
- Which model providers and deployment modes should the service support first?
