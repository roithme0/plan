---
type: architecture
status: proposed
last_reviewed: 2026-10-01
projects:
  - "[[Kochwiki]]"
  - "[[AI Service]]"
---

# Recipe Agent Capability and Proposal Outline

## Direction and status

Translate [[Recipe Optimization Skills and Kochwiki Tools]] into minimum capabilities and responsibilities. The workflow is agreed; names and integration conventions below remain proposed. Exact schemas, limits, storage choices, and deployment settings belong in the service repositories.

Kochwiki owns recipe-domain tools, instructions, future skill content, and stored proposals, exposed through MCP. AI Service becomes a generic conversation and MCP runtime. This supersedes the earlier outline that retained proposal registration and a domain-specific save bridge inside AI Service.

The initial context remains the selected recipe and its used foodstuffs. Additional ingredients and optional reference recipes use semantic discovery. Clarification is conversational, artifacts are selective, and clear matches require no confirmation. Temporary foodstuffs are persisted only with an explicit proposal save or standalone creation request. Skills, advanced UI shortcuts, and detailed security hardening follow later.

## Domain independence of AI Service

AI Service can carry recipe JSON, tool schemas, instructions, and opaque domain references without having compiled recipe/foodstuff/proposal models or interpreting their business meaning. Its responsibilities are generic:

- Model/provider integration and instruction/context assembly.
- Conversation messages, turns, execution limits, failures, and conversation expiry.
- MCP discovery, tool invocation, result handling, and connection lifecycle.
- Generic artifact envelopes, ordering, identifiers, and delivery to the host UI.
- Retention of tool results and opaque domain-context references needed across turns.

Kochwiki owns recipe validation, calculation, candidate definitions, proposal bases/lineage, proposal IDs and limits, persistence, and draft/dependency creation. Domain instructions come from Kochwiki; AI Service retains instructions about its own generic runtime behavior.

A configured agent may still be named `kochwiki` and identify an MCP connection and startup workflow. This is configuration, not a dedicated domain implementation. Remove the `kochwiki-contract` dependency once no domain imports remain.

## Minimum Kochwiki MCP capabilities

Names describe intended operations rather than finalized protocol contracts.

| Proposed capability | Conceptual input | Conceptual result | Persistence effect |
| --- | --- | --- | --- |
| `search_foodstuffs` | Natural-language query and bounded limit | Ranked summaries with canonical IDs, name/brand/unit, and truncation information | None |
| `get_foodstuff` | Canonical ID | Supported identity, unit, and nutrition fields; unknown values explicit | None |
| `search_recipes` | Natural-language query and bounded limit | Ranked summaries with lineage/version/state references | None |
| `get_recipe` | Exact lineage/version reference | Complete recipe and used-foodstuff presentation | None |
| `prepare_recipe_proposal` | Editing context/source, explicit base, complete candidate recipe, temporary foodstuff definitions | Validated stored proposal reference and complete preview | Proposal storage only; no recipe/catalogue write |
| `get_recipe_proposal` | Opaque proposal reference | Stored proposal content and presentation | None |
| `save_recipe_proposal` | Opaque proposal reference and write-operation identity | Actual draft references and created/reused foodstuff mappings | Draft and required missing foodstuffs atomically |
| `create_foodstuff` | Definition or reference to an exact retained candidate, plus write-operation identity | Persisted foodstuff or structured conflict/validation outcome | Explicit standalone catalogue write |

Proposed initial recipe-search coverage is active recipes, while the selected draft remains available through source context. Broader draft/history discovery can follow a concrete need.

Search supplies plausible matches, not identity guarantees. Return enough distinguishing information to support natural-language clarification. Zero results do not prove absence. Search relevance examples should cover paraphrases, branded/generic products, close alternatives, ambiguity, and no suitable match. Index technology and limits remain implementation choices. Newly created data must become searchable; do not make external indexing part of the atomic recipe/catalogue transaction.

Proposal retrieval allows tools and the host UI to refer to Kochwiki-owned content without treating the chat's rendered copy as the source of truth. Natural-language references such as "the second proposal" must resolve to an actual proposal reference; retrieval artifacts must not alter proposal numbering.

## Conversation lifecycle and domain lifecycle

Keep the existing shared conversation lifecycle in AI Service. Do not move chat history or model-turn orchestration to Kochwiki merely because proposals move.

| AI Service conversation state | Kochwiki domain state |
| --- | --- |
| Conversation ID, messages, active turn, turn result | Source recipe/version and any editing-context reference |
| Generic initial context and opaque connector references | Stored recipe proposals and their explicit bases |
| Generic artifact records and display order | Temporary foodstuff definitions and canonical mappings |
| Conversation lifetime and runtime budgets | Proposal retention/expiry and domain proposal limits |
| MCP client/connection lifetime | Atomic draft/dependency operations and write outcomes |

The current `recipe_improvement/session_lifecycle.py` is a thin recipe-typed wrapper over `ConversationSessionStore`, with a 90-minute lifetime, recipe artifact type, and proposal limit. Retain the shared store; replace the wrapper with generic configuration. Proposal-specific limits belong to Kochwiki and generic artifact/context limits stay in AI Service.

Kochwiki does not necessarily need a second full session system. An explicit editing-context handle is useful if it owns a frozen source snapshot, proposal scope, or context-level limits. It can hold those domain facts without chat messages. Alternatively, operations can carry explicit source/base references where sufficient. Choose the minimum domain context needed rather than duplicating the conversation lifecycle.

If an editing context is introduced, the host can establish it through Kochwiki and pass an opaque handle and initial snapshot to AI Service. A configured generic MCP startup operation is another option. AI Service should not hardcode recipe-specific startup behavior. The server determines the operation/content; the runtime only executes a declared integration convention.

An MCP connection is not the domain editing context. Pass explicit handles/references rather than infer proposal ownership from connection lifetime. Conversation expiry does not delete committed drafts or standalone foodstuffs. Kochwiki cleans up expired proposal-only state under its own policy; explicit close notification may be an optimization, not the sole cleanup mechanism. Surface expired/missing domain references as tool outcomes even if the conversation itself is still active.

The direction is to store proposals in Kochwiki, but storage durability and retention remain choices. That does not imply durable chat history, automatic chat reopening, or permanent proposal storage.

## Proposal-model responsibilities

| Concept | Kochwiki-owned requirement |
| --- | --- |
| Existing ingredient | Reference variant containing a canonical foodstuff ID |
| Temporary ingredient | Reference variant containing an issued local candidate ID |
| Candidate definition | Name, optional brand, unit, nullable supported nutrition; no invented database ID |
| Proposal content | Complete normalized recipe, source/base, and exact referenced candidate definitions |
| Preview | Resolved existing/candidate presentation with temporary status and missing data visible |
| Proposal identity | Domain-issued reference, immutable content, domain ordering/base rules |
| Save outcome | Draft references and candidate-to-canonical-ID mapping retained independently of chat artifacts |

Recommend defining new candidates within proposal preparation rather than requiring a general candidate-registration tool. Model-supplied request-local labels can connect definitions to ingredients; Kochwiki normalizes them to issued IDs. Refinement reuses unchanged definitions, while changed definitions receive new identities. Every stored proposal must retain enough content to save exactly what was reviewed.

Only candidates actually referenced by the recipe are materialized. Reject or normalize unused definitions, dangling references, inconsistent definitions, quantities/units, and duplicate ingredient references. Revalidate after canonical-ID mapping because two local candidates might resolve to the same existing foodstuff.

Preview and saving use the same domain rules. Extend Kochwiki's ID-only presentation/calculation boundary to resolve temporary definitions without inserting catalogue rows. Unknown nutrition staying unknown is the proposed initial policy; reuse existing calculation semantics rather than implementing nutrition in AI Service.

## Artifact delivery and selective presentation

A proposal is a Kochwiki domain object. An artifact is a generic conversation presentation record. Keep their identifiers and lifecycles distinct: displaying or redisplaying a proposal does not create another proposal.

Kochwiki defines typed payloads for reference recipes, foodstuffs, proposal previews, and persisted outcomes. The Kochwiki host supplies their renderers. AI Service understands only the agreed generic envelope and can store/forward the payload without domain deserialization.

Selective display requires an explicit integration convention. A Kochwiki MCP presentation operation can return retained domain data for display, or domain tools can support explicit display intent in their results. AI Service consumes a generic artifact marker/envelope; it must not infer display from a tool name or automatically turn every read into an artifact. MCP results alone do not establish our application-specific artifact behavior.

A generic local `show_artifact` facility is also possible if it references validated returned data, but it must not contain recipe-specific logic. The choice of a domain-provided display operation versus generic local presentation remains open; the earlier Kochwiki-specific `show_retrieved_item` implementation is not required.

Reference recipes have no proposal-save action. MVP clarification works through natural language; interactive candidate-selection controls are deferred. Generic transport instructions about artifact delivery stay in AI Service, while domain guidance about when to show a recipe or foodstuff belongs in Kochwiki.

## Atomic persistence and reliable outcomes

An explicit save request or existing save button includes creating required missing foodstuffs with no extra confirmation. Kochwiki resolves the stored proposal, validates current dependencies, creates/reuses exact matches, maps references, and creates the draft in one transaction. Failure rolls back newly inserted dependencies and the draft together.

Never reuse products on semantic similarity alone or silently modify shared foodstuffs. Exact identity/equivalence rules need service-level definition; conflicts return issues for conversational clarification.

Use runtime-generated write-operation identities, not model-invented values. Retries of the same intent recover the same outcome; a new explicitly requested save uses a new identity. Bind result recovery to the committed operation so a timeout or later model failure does not cause duplicate writes or falsely imply rollback.

If standalone creation has already materialized a candidate, Kochwiki retains the mapping and revalidates it during later saving without rewriting an immutable proposal. That intentional standalone record is not rolled back when a separate future save fails.

## Migration outline

1. Establish generic MCP tool/instruction consumption and a generic artifact delivery convention in AI Service, with semantic search/read capabilities in Kochwiki.
2. Add Kochwiki proposal storage, temporary candidate representation, pure preview, and proposal-reference tools.
3. Route conversational and existing UI saving to Kochwiki's atomic stored-proposal save. Add explicit standalone creation.
4. Switch the configured Kochwiki conversation to generic MCP execution and generic source context; remove recipe-specific input normalization, instructions, resolver, model types, artifact handler, proposal tools, and lifecycle wrapper from AI Service as their replacements become usable.
5. Keep shared session, tool-loop, model-adapter, artifact-envelope, and chat-UI infrastructure. Ensure artifacts cannot be counted or interpreted as domain proposals.
6. Verify representative workflows, rollback, unknown nutrition, ambiguous references, expired domain state, and uncertain writes. Skills, advanced UI actions, and security hardening follow separately.

Temporary coexistence during migration is acceptable. Do not preserve the domain-specific session wrapper as a permanent second architecture or remove current working behavior before the MCP replacements exist. Review tool budgets against real search/read/optional-display/proposal sequences.

## Remaining choices

- Minimal editing-context/bootstrap contract and proposal storage retention/durability.
- Generic artifact-envelope and selective-display convention across MCP.
- Semantic retrieval/index approach and MCP packaging/SDK.
- Exact foodstuff reuse/conflict rules and unknown-nutrition policy.
- Initial recipe-search states and operation-result recovery storage/lifetime.

## Compact handoff: start here for first-slice planning

The discussion established direction, not an implementation specification. Read this outline and [[Recipe Optimization Skills and Kochwiki Tools]] before defining the first slice. The existing [[AI-assisted Recipe Optimization]] initiative describes the delivered baseline and original scope; the next iteration deliberately expands it.

Agreed constraints to preserve:

- Improve the technical/workflow foundation; better recipe quality is a later outcome. MCP comes before skills and is not motivated by catalogue limits.
- Kochwiki owns domain tools, instructions, future skill content, stored proposals, and proposal lifecycle. AI Service should lose its domain models/tools/instructions and contract dependency as replacements become usable.
- Keep the shared AI Service conversation lifecycle and generic artifact transport. A connector configuration can remain named Kochwiki; it must not contain domain logic. Transitional coexistence is allowed.
- Snapshot the selected recipe and its used foodstuffs. Semantically discover additional foodstuffs and optionally reference recipes; searching and choosing an identity are distinct.
- Clarify through natural language, choose clear matches without confirmation, and show artifacts only when useful. Defer advanced click actions. A displayed reference is not a proposal; proposal IDs and artifact IDs are distinct.
- Proposals can contain unsaved candidate foodstuffs. Abandonment creates no catalogue records and needs no user cleanup.
- Explicit saving includes required missing foodstuffs without another confirmation. Kochwiki saves dependencies and draft atomically and rolls back new dependencies on failure. Standalone foodstuff creation requires an explicit request.
- Skills, sophisticated UI shortcuts, and detailed authentication/authorization/security hardening are later work, not blockers for developing the MVP foundation.

Suggested first slice, not yet selected: prove a generic MCP-backed conversation using Kochwiki-provided instructions, semantic foodstuff search, and detailed retrieval. Include one selective read-only foodstuff artifact if needed to validate the generic display convention. Keep the current recipe-improvement path working while the replacement develops. This slice is an engineering increment toward the agreed read/write workflow, not a final read-only product scope.

Before implementation, choose that slice's exact acceptance criteria, MCP transport/SDK, semantic retrieval approach, initial-context/bootstrap shape, and display convention. Proposal storage durability/retention and exact foodstuff reuse rules can be resolved when their slices are scoped; do not present the illustrative defaults above as approved decisions.

No service source code was changed during this planning discussion. Implementation evidence came from the local checkouts; recheck branch state and applicable repository instructions before editing. Detailed specs belong in the implementation repositories, while cross-project intent stays here.

## Implementation anchors

AI Service currently couples `agents/recipe.py`, `agents/wiring.py`, `sessions/http.py`, and the `recipe_improvement` package to recipe models/instructions/tools. Its `kochwiki-contract` dependency is explicit in `backend/pyproject.toml`. Shared `sessions/conversation.py` and `sessions/agent_service.py` already provide generic lifecycle foundations.

Kochwiki's recipe/foodstuff services and contract package own current domain calculation and persisted-ID schemas. Its frontend proposal-save mapping currently reconstructs writes from presentation and must switch to stored domain proposal references.
