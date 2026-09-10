---
type: project
status: planned
last_reviewed: 2026-09-10
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

Explore a backend and reusable chat UI as separately consumable parts of the AI Service. The backend could serve both embedded domain experiences and other applications directly, while a shared chat foundation could support Kochwiki AI editing and the longer-term universal agent. The packaging, deployment, and embedding model remain undecided; see [[Shared Agent Chat UI and Domain Rendering]].

Keep model- and provider-specific concerns behind stable service interfaces. Domain services continue to own their data, validation, mutations, and domain-specific rules.

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
- Which responsibilities belong to an AI Service backend, reusable chat UI, and host application?
- How should domain-specific artifacts and renderers be shared without coupling the AI Service to each domain?
- Which model providers and deployment modes should the service support first?
