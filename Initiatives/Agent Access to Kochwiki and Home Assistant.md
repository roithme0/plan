---
type: initiative
status: active
last_reviewed: 2026-10-09
projects:
  - "[[AI Service]]"
  - "[[Kochwiki]]"
  - "[[Home Assistant]]"
---

# Agent Access to Kochwiki and Home Assistant

## Intended outcome

Agents in AI Service can answer questions using authorized Kochwiki and Home Assistant data and perform selected, user-requested changes through bounded service interfaces. Reading and controlled writing are stages within one integration initiative.

## Current state

Partially delivered on the service repositories' `staging` branches, reviewed on 2026-10-09.

### Kochwiki: implemented reads and selected writes

- The configured Kochwiki agent in AI Service discovers and uses Kochwiki's Streamable HTTP MCP tools and server instructions through the generic MCP runtime.
- Reads include semantic foodstuff and recipe search, complete recipe lineage retrieval, and stored proposal retrieval. Recipe search includes active versions and drafts; lineage retrieval also includes historical versions.
- Writes include foodstuff creation and updates, in-memory proposal creation/refinement, and explicit proposal saving as a draft with atomic temporary-foodstuff creation. Chat and artifact-button saves share Kochwiki's save service. Proposal generation alone does not persist a recipe or publish an active version.
- The existing integration relies on the private-network boundary and temporary selected-user identity. It does not yet establish authenticated delegated authority, enforced recipe ownership, comprehensive action auditing, or a general enforced confirmation policy.

### Home Assistant: planned, initially read-only

The official MCP Server is the preferred connector. Selected state queries come first; selected device actions may follow after authorization and confirmation behavior are established. Neither the connector nor its actions are asserted as implemented here.

### Shared access controls and universal-agent rollout: open

The domain-specific Kochwiki agent is implemented; the general [[Universal Agent Foundation]] remains planned. Reuse existing Kochwiki capabilities when that agent is configured. Dedicated scoped credentials, verified delegated user identity, domain permissions, confirmation rules, and auditability remain open requirements.

## Motivation

Support questions about saved recipes, what to cook, and home state, and selected changes such as saving a recipe draft or switching an allowed light. Plan service access and its controls together rather than maintaining overlapping read and write initiatives.

## Scope and access controls

- Access domain services through intentional APIs/MCP tools rather than databases or filesystems.
- Bound readable data, actions, and resources per service with explicit allowlists.
- Preserve and validate the represented user's identity; every call must respect both that user's permissions and the tool's narrower scope.
- Use dedicated machine credentials without treating them as independent user authority or reusing a remembered browser user as the agent identity.
- Let each domain service own validation, authorization, and persistence.
- Require confirmation according to an action's consequences and preserve explicit Kochwiki proposal saving and user-controlled publication.
- Audit the acting user, agent/machine identity, requested action, and result.
- Make unavailable or incomplete source data and connection failures visible in answers.

These are intended controls, not claims that the current trusted-network Kochwiki implementation already enforces all of them.

## Home Assistant MCP approach

Use the official MCP Server described in [[Home Assistant]], with AI Service as the client. Verify the installed server's actual tools and context and begin with a small exposed-entity set.

The standard Assist surface can include actions. The initial read-only configuration must enforce a verified read-tool allowlist and prevent writes from executing; hiding actions in model instructions or exposing selected entities alone is insufficient. Resolve credential scope and represented-user authorization before treating the connector as delegated access.

Verification should cover allowed reads, unavailable/unexposed data, connection failure, and rejected actions. Before enabling selected writes, also verify permission denial, required confirmation, domain validation, and audit records. Detailed setup and tests belong in the service repositories.

## Out of scope

- Unrestricted CRUD, administrative, database, filesystem, or network access
- Granting the agent broader authority than the represented user
- Treating temporary selected-user identity as authenticated delegation
- Assuming all writes require the same confirmation policy
- A model of household ingredient inventory unless separately introduced

## Project roles

- [[AI Service]] owns orchestration, connectors, delegated context, and confirmation interaction.
- [[Kochwiki]] owns recipe/foodstuff data, domain permissions, validation, proposals, drafts, and persistence.
- [[Home Assistant]] owns entity exposure, state, permissions, and home-action execution.

## Dependencies

- [[Universal Agent Foundation]] for the general-agent rollout, not for the already delivered Kochwiki agent
- [[Browser Authentication Foundation]] and appropriate service-to-service authentication/authorization
- Explicit read/action allowlists, verified delegation, auditability, and confirmation policies

## Open questions

- Should cooking recommendations use only saved recipes or eventually household ingredient inventory?
- Which Home Assistant entities and state form the smallest useful initial read surface?
- Which actions may run immediately, which need confirmation, and which remain unavailable?
- How should represented user identity be verified across services and recorded alongside machine identity?

## Next step

Reuse Kochwiki's delivered read/write integration. Establish authenticated delegated authority and scoped permissions for broader exposure, with confirmation and auditing for writes. Introduce a minimal Home Assistant read connector first; evaluate selected home actions afterward. Configure the future universal agent against these same service boundaries.

## Implementation evidence

Reviewed tracked code and documentation on `staging`; no live deployment or real-model end-to-end test was run for this planning update.

- [Kochwiki MCP tools and behavior](https://github.com/roithme0/kochwiki-v2/blob/0c8023952a1ed306835f947aae3a1dc25b811ec3/docs/mcp.md)
- [Kochwiki MCP server](https://github.com/roithme0/kochwiki-v2/blob/0c8023952a1ed306835f947aae3a1dc25b811ec3/backend/app/mcp_server.py)
- [AI Service Kochwiki agent wiring](https://github.com/roithme0/ai-service/blob/3cff08f9b0e0b0b528de7db6fff7b283dd22f2c0/backend/app/agents/wiring.py)
