---
type: project
status: planned
last_reviewed: 2026-08-30
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

Keep model- and provider-specific concerns behind stable service interfaces.

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
- Which model providers and deployment modes should the service support first?
