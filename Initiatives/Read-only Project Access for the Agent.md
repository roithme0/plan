---
type: initiative
status: active
last_reviewed: 2026-10-09
projects:
  - "[[AI Service]]"
  - "[[Kochwiki]]"
  - "[[Home Assistant]]"
---

# Read-only Project Access for the Agent

## Intended outcome

The universal agent can answer useful questions using authorized data from Kochwiki and Home Assistant without changing either system.

## Current state

Partially delivered on the service repositories' `staging` branches, reviewed on 2026-10-09. Kochwiki read access is implemented and already used by the configured Kochwiki agent in AI Service through its MCP connection. This is a domain-specific agent integration; a general universal agent and the Home Assistant connector remain planned.

- Kochwiki exposes semantic foodstuff and recipe search, complete recipe lineage retrieval, and stored proposal retrieval over Streamable HTTP MCP. Recipe search includes active versions and drafts; lineage retrieval also includes historical versions.
- AI Service discovers Kochwiki tools and server instructions and executes them through its generic MCP runtime. Domain data is obtained through tools, not direct database access.
- The existing connection is not read-only: it also exposes foodstuff creation/updates, in-memory proposal creation, and explicit proposal saving as a draft with atomic temporary-foodstuff creation. These delivered writes are recorded in [[Controlled Agent Actions]] without claiming that its broader authorization and audit requirements are complete.
- The integration currently relies on the private-network boundary and temporary selected-user identity. Dedicated scoped credentials, authenticated delegated authority, and domain ownership enforcement remain open.

The read-only outcome below remains the target for the future universal-agent and Home Assistant rollout, not a description of the existing Kochwiki agent's full toolset.

## Motivation

Enable requests such as asking about recipes, home state, or what to cook while establishing safe service boundaries before actions are introduced.

## Scope

- Introduce dedicated, scoped machine identities or credentials
- Expose constrained Kochwiki read capabilities through an intentional agent-facing contract
- Allow access only to selected Home Assistant entities and state
- Access both projects through their APIs rather than their databases or filesystems
- Keep the entire initiative read-only
- Make limitations in available source data visible in answers

## Home Assistant MCP approach

Prefer the official MCP Server integration described in [[Home Assistant]]. AI Service connects as the MCP client and consumes the available read tools and context where useful. Select a small set of exposed entities and verify the installed server's actual capabilities.

The standard Assist surface can also include actions. For this initiative, explicitly restrict callable tools to verified reads and test that writes cannot execute; hiding action tools in model instructions alone is insufficient. Resolve credential scope and represented-user authorization before treating the connector as delegated access.

Acceptance should cover an allowed state query, unavailable/unexposed data, connection failure, and attempted use of a disallowed action. Detailed setup and tests belong in the service repository.

## Out of scope

- Reusing a remembered browser user as the agent identity
- Treating Kochwiki's current unrestricted CRUD API as the final agent contract
- Modifying recipes, entities, devices, or automations

## Project roles

- [[AI Service]] owns agent orchestration and the project connectors.
- [[Kochwiki]] owns and authorizes access to recipe-domain data.
- [[Home Assistant]] owns and authorizes access to home state.

## Dependencies

- [[Universal Agent Foundation]]
- Authentication and authorization appropriate for service-to-service access
- An explicit initial allowlist of readable data in each project

## Open questions

- Should initial cooking recommendations use only stored recipes, or require a future model of ingredients currently available at home?
- Which Kochwiki operations and Home Assistant entities form the smallest useful read-only surface?

## Next step

Reuse the implemented Kochwiki MCP connection when introducing the universal agent. Define scoped authorization and the smallest useful Home Assistant read allowlist; verify that the future read-only configuration cannot execute writes. Do not reimplement Kochwiki access as though it were missing.

## Implementation evidence

Reviewed tracked code and documentation on `staging`; no live deployment or real-model end-to-end test was run for this planning update.

- [Kochwiki MCP tools and behavior](https://github.com/roithme0/kochwiki-v2/blob/0c8023952a1ed306835f947aae3a1dc25b811ec3/docs/mcp.md)
- [Kochwiki MCP server](https://github.com/roithme0/kochwiki-v2/blob/0c8023952a1ed306835f947aae3a1dc25b811ec3/backend/app/mcp_server.py)
- [AI Service Kochwiki agent wiring](https://github.com/roithme0/ai-service/blob/3cff08f9b0e0b0b528de7db6fff7b283dd22f2c0/backend/app/agents/wiring.py)
