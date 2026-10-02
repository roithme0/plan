---
type: architecture
status: implemented-foundation
last_reviewed: 2026-10-02
projects:
  - "[[Kochwiki]]"
  - "[[AI Service]]"
---

# Recipe Agent Capability and Proposal Outline

## Direction and status

The agreed MCP-based recipe workflow foundation is implemented in the local
Kochwiki and AI Service checkouts. This note records the delivered architecture
and follow-up priorities; service repositories remain authoritative for schemas,
limits, deployment and tests. Manual end-to-end verification with the real model
is left to the user. Implementation is not evidence of better recipe quality or
production readiness.

This replaces the exploratory outline from 2026-10-01. Subsequent discussion
simplified candidate identities, storage, discovery, presentation and repeated
saving. Earlier illustrative contracts are not outstanding implementation tasks.

## Responsibilities

Kochwiki owns recipe and foodstuff validation, calculation, semantic search,
domain instructions, proposals and atomic draft/dependency saving. Its official
Python MCP SDK server runs inside the existing backend and exposes Streamable
HTTP at `/mcp/`. Future recipe skills also belong to Kochwiki.

AI Service owns generic model integration, conversations, turns, tool execution,
MCP connection lifecycle and generic artifact validation/delivery. Its configured
`kochwiki` agent identifies a model and MCP connection, not domain implementation.
Recipe-specific models, tools, instructions, lifecycle wrappers and the
`kochwiki-contract` dependency have been removed.

The Kochwiki frontend owns domain renderers and advertises presentation
capabilities with complete payload schemas, header guidance and optional metadata
schemas. Shared chat UI provides generic display infrastructure and a JSON renderer.

Guidance has separate owners:

- MCP instructions: domain workflow, clarification, duplicate checks, explicit
  writes, proposal refinement and selective presentation.
- Tool descriptions and schemas: individual operation inputs, outputs, side effects
  and constraints.
- Artifact capabilities: renderer data, titles, subtitles and metadata meanings.
- AI Service guidance: generic conversation, execution and presentation behavior.

## Context, discovery and tools

The frontend supplies a snapshot of the selected recipe version including its
used foodstuffs inline. It does not supply the full catalogue. Search is optional
and used for additional ingredients or reference recipes.

| Capability | Delivered behavior |
| --- | --- |
| `search_foodstuffs` | Name/alias semantic search with enriched identity, unit and nutrition summaries |
| `search_recipes` | Name semantic search with complete active versions and drafts; historical versions excluded |
| `create_foodstuff` | Explicit standalone catalogue creation |
| `update_foodstuff` | Explicit update of an unambiguously identified shared entry |
| `create_recipe_proposal` | Validate and store a complete candidate with source/base references |
| `get_recipe_proposal` | Retrieve stored input and resolve a complete current presentation |
| `save_recipe_proposal` | Save the proposal and required temporary foodstuffs atomically |

Search results are enriched deliberately so a separate get call is normally
unnecessary. Separate foodstuff/recipe get tools were deferred. Similarity scores
stay internal. Search is a prefilter, not proof of identity or absence. Clear
matches need no confirmation; ambiguity is clarified through natural language.
Search initially targets names/aliases, not broad nutritional/category queries.

AI Service discovers tools and server instructions at startup and exposes stable
connection-prefixed tool names. Restart it after server capability changes.
Discovery does not call domain tools. Multiple MCP connections are supported by
the generic runtime; Kochwiki is the first integration.

## Proposals and lifecycle

Kochwiki stores proposals and proposal-to-saved-version mappings in process memory,
assuming one deployment/worker. They survive neither backend restart nor shutdown.
There is no second domain conversation system, editing-context bootstrap, database
proposal storage or automatic proposal expiry in this foundation.

AI Service retains ephemeral conversations with a fixed 90-minute lifetime.
Conversation expiry does not delete committed recipes or foodstuffs. Proposal
storage is independent of chat artifacts and conversation lifetime.

Each proposal has a domain-issued ID, an original source recipe version and an
optional base proposal. Refinements register a new complete proposal and retain
the base's original source. Existing ingredients reference canonical foodstuff IDs;
temporary ingredients contain inline definitions with name, unit and optional
brand/nutrition. There are no separate temporary candidate IDs or registration
operations. Repeated ingredient identities and invalid references are rejected.

Source versions and existing foodstuffs are references, not frozen copies.
Presentation resolves current foodstuff values and computes nutrition without
inserting catalogue rows. Unknown nutrition remains unknown. Missing dependencies
produce errors when an operation requires them. Abandoned proposals create no
catalogue data and need no user cleanup.

## Presentation and actions

MCP returns domain data and has no artifact interface. The model chooses whether
to call AI Service's generic local `present_artifact` tool with a complete payload
matching a frontend-advertised capability. The frontend performs no enrichment
fetches. Retrieval does not automatically display an artifact.

Recipe and foodstuff capabilities reuse Kochwiki presentation components.
Presentational fields stay minimal; catalogue IDs and domain state are excluded
unless required by a separately advertised metadata contract. Foodstuff names are
artifact titles and brands are optional subtitles. Recipe names are titles.
JSON is an explicit capability, not the automatic rendering path for domain results.

Optional recipe artifact metadata `proposalId` identifies the stored proposal
represented by the display and enables its save button. Existing recipe references
have no proposal-save action. The button uses Kochwiki's HTTP save endpoint, which
calls the same service and store as the MCP save tool. Artifact IDs and proposal
IDs remain distinct; redisplaying a proposal does not register a new proposal.

The agent knows which proposal ingredients are temporary. Visually distinguishing
them is a deferred presentation improvement; it is not part of the current
minimal recipe renderer contract.

## Writes and outcomes

Dedicated foodstuff creation and updates require explicit requests. Duplicate
checks, target clarification and nutrition-unit clarification/warnings are agent
workflow policies. Validation still enforces operation schemas. Standalone writes
are distinct from proposing temporary ingredients.

Explicit proposal saving also authorizes creating required temporary foodstuffs,
without a separate confirmation. Kochwiki revalidates dependencies and creates the
foodstuffs and source-lineage draft in one transaction; failures roll back together.
Publication, acceptance, discard and deletion are not agent tools.

Repeated saving of the same proposal returns its previously created version in
its current state, even after edits or publication. A deleted saved version causes
an error rather than recreation. The mapping is in memory alongside proposals;
a separate write-operation identity or durable outcome ledger is not implemented.

Temporary definitions currently create new catalogue entries. If a matching entry
was created separately, saving reports a conflict and rolls back. Automatic reuse,
canonical mappings across proposals and conflict reconciliation are deferred
stability improvements; semantic similarity must not silently merge products.

## Verification and follow-up priorities

Automated coverage exercises MCP discovery/instruction delivery, generic tool and
artifact composition, proposal validation/preview, atomic rollback, repeat saves,
HTTP/MCP convergence and UI save behavior. Scripted models do not establish real
agent behavior. The user will perform manual end-to-end verification; no new
verification slice is scheduled now.

Runtime budgets have been raised to provide room for richer tool workflows while
remaining finite. Exact values and timeout constraints belong in AI Service docs.

Deferred work, as agreed on 2026-10-02:

1. **High-priority follow-up: retain tool results across turns.** Currently only
   final replies, not tool transcripts, reach later turns; identifiers depend on
   final replies. Generic retention/context handling needs its own slice. It is
   more urgent than the improvements below, but deliberately deferred now.
2. Visually distinguish existing and temporary proposal ingredients.
3. Reconcile temporary ingredients with catalogue entries created after proposal
   registration, including explicit standalone creation.
4. Add focused skills and evaluate recipe quality once the foundation is exercised.
5. Add durable history/outcome recovery, streaming/cancellation, advanced UI actions
   and authentication/authorization/auditing as separately scoped work when needed.

The immediate outcome is the implemented technical/workflow foundation, not
measurably better suggestions. Manual verification and deferred improvements do
not justify describing it as a production-ready feature.
