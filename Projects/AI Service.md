---
type: project
status: active
last_reviewed: 2026-10-02
---

# AI Service

## Purpose

Provide shared AI capabilities and an agent-based interface across projects in the network.

## Current role

The service runs a generic session-based conversation API with a configured MCP-backed Kochwiki agent and a deterministic demo agent. It owns bounded tool orchestration, generic validation, ephemeral conversation state and artifact delivery. Kochwiki owns domain tools, instructions and proposals; its frontend uses the service through restricted gateway routes. See [[Recipe Agent Capability and Proposal Outline]].

The service also publishes the reusable Angular `@roithme0/chat-ui` package. Kochwiki consumes its UI and conversation entry points and supplies a recipe artifact renderer. A universal agent for generic questions or live weather is not implemented. Streaming, cancellation, durable chat history, and production-grade cross-service authorization remain open.

## Direction

Develop two logically distinct capability areas within one project:

- Bounded AI capabilities requested by other services, such as recipe optimization and image generation
- A universal agent that can answer questions, use tools, and progressively interact with authorized project APIs

The backend and Angular chat package are separately consumable in the first Kochwiki integration. A future universal agent can build on the same conversation and tool boundaries. Longer-term framework-neutral embedding and cross-application renderer reuse remain open; see [[Shared Agent Chat UI and Domain Rendering]].

Keep model- and provider-specific concerns behind stable service interfaces. Domain services continue to own their data, validation, mutations, and domain-specific rules.

## Foundation and remaining direction

- The backend retains caller context, messages, and generic artifacts in process-local sessions. Completed turns can produce several validated artifacts; there is no database-backed chat history.
- A provider adapter and generic MCP tool loop are implemented. Service code assigns artifact identifiers, order and timestamps; proposal IDs and base references belong to Kochwiki.
- The Angular package exposes separate `/ui` and `/conversation` entry points. Kochwiki registers recipe and foodstuff templates and advertises their schemas and metadata. JSON is an explicit presentation capability.
- Tool-result retention across turns is a high-priority deferred follow-up. Streaming, cancellation, tool-status presentation and the general model-backed agent remain later work.
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
