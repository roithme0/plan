---
type: project
status: active
last_reviewed: 2026-10-09
---

# AI Service

## Purpose

Provide shared AI capabilities and an agent-based interface across projects in the network.

## Current role

The service runs a generic session-based conversation API with a configured MCP-backed Kochwiki agent and a deterministic demo agent. It owns bounded tool orchestration, generic validation, ephemeral conversation state and artifact delivery. Kochwiki owns domain tools, instructions and proposals; its frontend uses the service through restricted gateway routes. See [[Recipe Agent Capability and Proposal Outline]].

The service also publishes the reusable Angular `@roithme0/chat-ui` package. Kochwiki consumes its UI and conversation entry points and supplies a recipe artifact renderer. A universal agent for generic questions or live weather is not implemented. Streaming and ordered model/tool history are implemented in AI Service; cancellation, durable chat history, and production-grade cross-service authorization remain open.

## Direction

Develop two logically distinct capability areas within one project:

- Bounded AI capabilities requested by other services, such as recipe optimization and image generation
- A universal agent that can answer questions, use tools, and progressively interact with authorized project APIs

The backend and Angular chat package are separately consumable in the first Kochwiki integration. A future universal agent can build on the same conversation and tool boundaries. Longer-term framework-neutral embedding and cross-application renderer reuse remain open; see [[Shared Agent Chat UI and Domain Rendering]].

Keep model- and provider-specific concerns behind stable service interfaces. Domain services continue to own their data, validation, mutations, and domain-specific rules.

## ChatGPT plan access direction

[[ChatGPT Plan Integration]] remains a desired additional model-access mode, but is blocked as of 2026-10-07. The MVP requires browser-only plan connection from desktop and mobile to the LAN-hosted Kochwiki chat, with AI Service owning token exchange, persistent per-user credentials and refresh. The documented public plan-access flow requires a callback listener on the user's computer; local helpers and credential-transfer workflows do not meet the requirement. Partner integration is not viable for this personal, non-commercial use case, and website identity login alone does not establish permission to consume the ChatGPT allowance. See the initiative for evidence and the condition for revisiting implementation.

The configured API key remains the baseline for users without a connected plan. Plan failure must never trigger automatic paid fallback. Ordered history and streaming are delivered; failure reconciliation and deliberate retry remain deferred. Provider consent is separate from application identity and domain authorization.

## Foundation and remaining direction

- The backend retains caller context, messages, and generic artifacts in process-local sessions. Completed turns can produce several validated artifacts; there is no database-backed chat history.
- A provider adapter and generic MCP tool loop are implemented. Service code assigns artifact identifiers, order and timestamps; proposal IDs and base references belong to Kochwiki.
- The Angular package exposes separate `/ui` and `/conversation` entry points. Kochwiki registers recipe and foodstuff templates and advertises their schemas and metadata. JSON is an explicit presentation capability.
- Ordered model/tool history, retained execution outcomes and accepted artifacts, and end-to-end streaming are implemented in AI Service. The UI exposes complete messages, tool preparation/execution/outcomes, and artifacts; token fragments, richer action details, cancellation, observation reconciliation, and deliberate retry remain later work. These foundations do not resolve the external authorization blocker in [[ChatGPT Plan Integration]].
- Kochwiki and the AI Service currently operate within a private-network boundary. Authentication, delegated user context, and service authorization are required before exposing the integration more broadly.

## Web information retrieval direction

[[Web Information Retrieval]] has an implemented hosted OpenAI Web Search MVP on staging, enabled for the Kochwiki agent through the shared model runtime. Search is provider-executed and bounded per turn; hosted activity and continuation history are retained. Citation metadata remains internal and clickable source rendering is deferred. Universal-agent rollout, usage presentation and provider/model support, including the ChatGPT-plan path, remain open. Browser UI automation is outside this initiative.

## Home Assistant connector direction

Use the official Home Assistant MCP Server through the generic MCP runtime described in [[Recipe Agent Capability and Proposal Outline]], rather than introducing Home Assistant domain logic into the runtime. [[Home Assistant]] records the available surface and planned rollout: selected state access first, controlled actions later. This connector is planned; current implementation is not asserted here.

## Ecosystem relationships

See [[System Overview]].

Support [[Training and Nutrition Coordination]] across [[Kochwiki]] and the planned [[Training App]] through optional AI assistance. Training and nutrition data remain owned by their domain applications; the exchange and orchestration approach remains open.

## Related initiatives

- [[Training and Nutrition Coordination]] — planned; shared sporting goals, training/nutrition planning, and progress

- [[AI-assisted Recipe Optimization]] — active
- [[Recipe and Ingredient Images]] — planned
- [[Universal Agent Foundation]] — planned
- [[ChatGPT Plan Integration]] — blocked; browser-only remote-callback plan authorization is not established
- [[Web Information Retrieval]] — active; hosted OpenAI search implemented for the Kochwiki agent, clickable citations remain open
- [[Agent Access to Kochwiki and Home Assistant]] — active; Kochwiki MCP reads and selected writes implemented, universal-agent and Home Assistant rollout remain open
- [[Photo-assisted Foodstuff Nutrition Entry]] — exploring

## Open questions

- How should bounded AI calls and stateful agent operations be separated within the service?
- Through which interface or interfaces will users initially interact with the agent?
- How should domain-specific renderers later be distributed and reused outside their host applications?
- Which model providers and deployment modes should the service support first?
