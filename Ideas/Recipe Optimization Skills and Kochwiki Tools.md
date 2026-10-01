---
type: idea
status: exploring
last_reviewed: 2026-10-01
projects:
  - "[[Kochwiki]]"
  - "[[AI Service]]"
---

# Recipe Optimization Skills and Kochwiki Tools

## Agreed direction

Improve the technical foundation for future recipe optimization by enabling a richer agent workflow. Better recipe quality remains a longer-term outcome, not the immediate acceptance criterion for this iteration.

Prioritize Kochwiki capabilities exposed through MCP. Skills are interesting as a following step. The motivation is broader agent capability, not overcoming snapshot limits: the current foodstuff catalogue is below the count limit and snapshot limits are acceptable today.

The direction clarified on 2026-10-01 retains the selected recipe and its used foodstuffs in the initialization snapshot. Additional foodstuffs and optionally other recipes are discovered through semantic search and retrieved through tools. The agent can show retrieved items as artifacts. Proposals may include temporary, not-yet-persisted foodstuff candidates; saving a proposal creates its required missing foodstuffs together with the draft. Standalone foodstuff creation is available only on explicit request. Tool schemas, packaging, and delivery order remain under discussion. This note does not authorize implementation.

Interaction defaults are now agreed: resolve ambiguity through natural language, let the agent use a clear match without a confirmation step, and show artifacts selectively rather than for every retrieval. Advanced selection buttons and other artifact actions are optional later shortcuts. An explicit proposal save includes creation of its missing foodstuffs without separate confirmation.

Treat this iteration as an MVP foundation. Prioritize working conversations, semantic retrieval, proposal representation, and reliable persistence. Detailed authentication, delegated authorization, audit policy, and production security hardening are successive follow-up work, not prerequisites for developing this foundation.

## Observed foundation

Reviewed the local AI Service and Kochwiki v2 checkouts on 2026-10-01:

- The recipe chat supports refinement, multiple complete proposals, source/prior-proposal references, and user-triggered saving as a new draft.
- AI Service has a provider-neutral tool registry, bounded orchestration, and configurable recipe instructions/tool factories. Versioned skills and skill provenance are not implemented.
- Current session input includes a source recipe and full foodstuff catalogue, limited to 100 foodstuffs and 16,000 characters of initial serialized context. These limits are not driving this work.
- Proposal validation permits only foodstuff references from the initial catalogue. An HTTP call to Kochwiki resolves presentation and calculated nutrition.
- Kochwiki already implements recipe/foodstuff listing and reading, foodstuff creation, and draft creation. These domain operations can be reused; they are not already a scoped agent contract.
- Foodstuff nutrition fields may be missing. A name/brand uniqueness conflict path exists, but is not complete semantic duplicate detection.
- No Kochwiki MCP server or AI Service MCP client is implemented in the inspected code.
- Temporary user selection and a private-network boundary do not implement authenticated delegation or owner-only authorization.

Implementation anchors: AI Service `backend/app/recipe_improvement/{instructions,turn_service,session_input,proposals,resolver}.py` and `backend/app/sessions/tools.py`; Kochwiki `backend/app/api/routes/{recipes,foodstuffs}.py`, `backend/app/services/foodstuffs.py`, `backend/app/schemas/foodstuff.py`, and its recipe conversation page.

## Workflow to enable

These are conditional steps, not a mandatory script for every conversation:

1. Understand the user's task using the selected recipe/version and its used foodstuffs from the initialization snapshot.
2. Semantically search for additional foodstuffs as needed. Optionally find and retrieve another recipe when the user refers to it or it is useful reference material.
3. Show selected retrieved foodstuffs or recipes as artifacts when useful for identification, confirmation, or discussion. Clarify ambiguous matches.
4. Represent genuinely missing ingredients as temporary foodstuff candidates embedded in the proposal, with their identity, unit, and available nutrition information. Do not persist them during exploration.
5. Validate and register a complete reviewable proposal combining existing foodstuff references and temporary candidates. Preview it without creating catalogue data.
6. Refine the proposal or, on an explicit save request or proposal-card action, persist its required missing foodstuffs and the draft together. Return the actual draft reference/link and the created or reused foodstuff references.

Separately, an explicit request to create a foodstuff may persist it without creating a recipe draft. That operation reports its own outcome and is not a side effect of ordinary proposal generation.

Reading and proposal generation are non-persistent by default. Abandoned or expired proposals leave no shared foodstuff records behind and require no user cleanup. Explicit persistence requests are distinct from confirming that a retrieved item is the intended match.

Publication, deletion, editing existing shared foodstuffs, and accepting/discarding drafts are not requested capabilities for this iteration.

## Candidate tools

Names describe intent only; exact contracts belong in the service repositories.

Kochwiki will own and store proposals and provide domain tools, instructions, and future skill content through MCP. AI Service retains generic conversation sessions, model/tool execution, and artifact delivery. [[Recipe Agent Capability and Proposal Outline]] is the current architectural outline and includes a compact handoff for first-slice planning.

The proposed minimum inputs/results, local-versus-MCP boundary, and model changes are mapped in [[Recipe Agent Capability and Proposal Outline]]. That note develops this direction without finalizing service API schemas.

| Capability | Workflow role | Result or effect |
| --- | --- | --- |
| Semantic search foodstuffs | Discover ingredients from natural-language descriptions without fetching the whole catalogue | Ranked, bounded candidates with authoritative IDs |
| Read foodstuff | Obtain selected ingredient details | Units, available nutrition, explicit missing fields |
| Create foodstuff | Fulfil an explicit standalone creation request | Persisted shared foodstuff and its ID |
| Semantic search recipes | Discover references despite differences in names or descriptions | Ranked, bounded summaries with lineage/version references |
| Read recipe | Inspect a reference recipe/version | Complete recipe with its state and role clear |
| Prepare/read proposal | Validate, store, and retrieve a Kochwiki-owned proposal | Proposal reference and preview; no recipe/catalogue write |
| Resolve recipe presentation | Validate and preview existing ingredients plus temporary foodstuff candidates | No persistence; may remain an internal runtime call |
| Create recipe draft with required foodstuffs | Save an authorized proposal in its intended lineage | Draft plus created/reused foodstuff references, committed together |

Proposal preparation, validation, storage, and retrieval belong to Kochwiki. A conversation-facing save tool accepts a Kochwiki-issued proposal reference and resolves its retained content within Kochwiki. AI Service passes references and generic payloads without reconstructing or interpreting recipe content.

Do not ask the model to reconstruct a displayed proposal for saving. The proposal-card action and conversational action should converge on the same domain creation semantics; identical transports are not required.

## Workflow choices

### Semantic discovery and retrieved-item artifacts

Search should accept ordinary descriptions of foodstuffs and recipes, tolerate wording/name differences, and return a small useful candidate set. Retrieving the entire catalogue or requiring the agent to enumerate aliases is not the intended workflow. The choice of embedding model, index, and any lexical/hybrid component is deferred to implementation design.

Semantic similarity supplies candidates, not proof that two foodstuffs or recipes are interchangeable. Preserve identity, brand, unit, and recipe/version references; clarify material ambiguity instead of silently selecting the highest-ranked result. A weak or empty result is not by itself proof that a foodstuff is absent. Establish search acceptance examples for paraphrases, close alternatives, ambiguous names, and no suitable match, and keep results consistent with catalogue/recipe changes.

Reuse recipe presentation for a retrieved-recipe artifact and add a foodstuff artifact. The UI must distinguish reference recipes, existing foodstuffs, proposed foodstuff candidates, recipe proposals, and successful creation/save results. A reference recipe must not acquire proposal-saving actions merely because it uses the same visual renderer.

The agent should be able to show selected retrieved items for confirmation or opinion, but should not automatically emit an artifact for every read or search result. Clarify through natural language in the MVP: the user can describe the intended item or refer to a shown candidate. Foodstuff-selection buttons, confirmation controls, and other sophisticated artifact interactions are deferred as optional shortcuts. Existing recipe rendering and direct save behavior can remain; new workflows must work through conversation without requiring new UI actions.

Whether explicit presentation is a separate capability or an opt-in part of retrieval remains an implementation choice. In either case, artifact emission must be selective and displayed data must come from actual retrieval rather than model reconstruction.

### Temporary foodstuffs and persistence

Persisted foodstuffs are shared canonical data under [[DEC-001 Shared Foodstuffs and Owned Recipes]]. A temporary candidate in a proposal is not yet a canonical foodstuff and must not receive an invented database ID.

The Kochwiki proposal model needs to distinguish an existing foodstuff reference from a proposal/context-local candidate reference. Store candidate definitions with the reviewable proposal, assign local identifiers in Kochwiki, and retain them through refinements and alternatives. Saving must resolve the exact definitions reviewed by the user. Sharing candidates across proposals and recording subsequent canonical-ID mappings are implementation choices; do not silently rewrite immutable proposal content.

Search before proposing a missing foodstuff, distinguish generic ingredients from branded products, and define near-duplicate handling. Establish the source/basis of nutrition values. The current schema permits unknown values; whether labelled estimates may be stored is a separate choice. Preview validation/calculation must support candidate data without inserting it into the database; the current ID-only resolver needs an extension or equivalent pure domain operation.

On an explicit request to save a proposal, Kochwiki should validate the complete materialization request, resolve existing references, create or reuse suitable candidate foodstuffs, and create the draft in one database transaction. A failure must roll back newly inserted foodstuffs and the draft together. Use a combined domain capability rather than a model-driven chain of independent foodstuff-create calls followed by draft creation.

Atomic saving and rollback of newly created dependent foodstuffs on draft-creation failure were explicitly agreed during the discussion. Exact contracts remain to be designed.

Recheck for candidates created elsewhere since the proposal was generated. Do not merge semantically similar products automatically; define when exact identity permits reuse and when a conflict requires clarification. Record candidate-to-canonical-ID mappings in the save result. Handle intentional repeated saves separately from retries using an explicit idempotency/result-recovery contract.

Standalone foodstuff creation remains a separate tool, gated by an explicit creation request. Successful standalone creation is an intended persistent outcome even if no recipe is later saved. This differs from proposal-only candidates, which expire with unsaved conversation state.

### Reference recipes

Reading other recipes intentionally expands the original recipe-only scope. It can provide preparation patterns, ingredient combinations, or examples the user references. It does not change the editing target or grant mutation rights.

DEC-001 describes the eventual authenticated access model. Detailed visibility and permission policy belong to later hardening rather than driving this MVP discussion. Switching targets requires explicit user intent; copying/forking another user's recipe is not implied by read access.

### Context and validation

Keep the source recipe/version and its used foodstuffs in the initialization snapshot. Replace the full upfront catalogue with tools for additional ingredients. Consulting other recipes is optional, not a mandatory step before every proposal.

Replace the initial-catalogue-only ingredient rule with validation of existing permitted Kochwiki references and explicit temporary candidate definitions. Snapshot foodstuffs, retrieved foodstuffs, and temporary candidates need distinct, validated references. A temporary candidate must be usable in a proposal before any persistence occurs.

Define retained explanation/provenance and validation during proposal registration and saving. DEC-001 already accepts live shared-foodstuff references and changing historical nutrition; immutable foodstuff history is not a prerequisite for this work.

### Conversational saving

Resolve references such as "save the second option" to an exact stored proposal or clarify through conversation. Derive the lineage/source deterministically. Make proposed missing foodstuffs visible in the reviewable proposal so saving it also clearly materializes those dependencies. An explicit save request includes their creation, with no separate confirmation. Return the actual saved draft and navigation action; retain click-to-save with the same combined domain semantics. Detailed user permission enforcement is later hardening work.

Distinguish intentional repeated saves from retries after uncertain outcomes. Define write identity, idempotency, and result recovery rather than relying on automatic retries.

## Representative conversations for discussion

These examples are fictional design cases, not descriptions of actual stored data. They exercise workflow behavior rather than nutritional optimization quality. Natural-language clarification, clear-match selection, selective presentation, and dependency creation on explicit save are agreed defaults; remaining proposals are identified below.

### 1. Find an existing foodstuff and propose a change

The snapshot contains a selected pasta recipe and its used foodstuffs. The user says: "Try this with the chickpea pasta I use."

1. The agent semantically searches for the described product and receives a bounded candidate set.
2. If two plausible products differ in brand or form, it asks in natural language: "Do you mean the Brand A spirals or the Brand B penne?" It may show foodstuff artifacts when those details help; no selection controls are required.
3. The user answers in natural language. The agent generates a complete recipe proposal using the intended canonical ID, without any catalogue or draft write.
4. If one product is a clear match, the agent may proceed without confirmation or a separate retrieval artifact. Explain its choice when useful; the user can correct it through the conversation.

Derived needs: semantic candidate discovery, authoritative detail retrieval, optional artifact presentation, reference selection, and proposal validation. Confirmation of a match is not consent to save a draft.

### 2. Propose a genuinely missing foodstuff

The user requests a specific product for which search does not reveal a suitable existing record, and clarifies the product identity when necessary.

1. The agent collects enough identity and unit information to form a temporary candidate. Required missing details lead to a targeted question.
2. It shows a foodstuff candidate marked "New foodstuff - not saved", either within the recipe proposal or as a linked artifact. The exact UI arrangement remains open.
3. The complete proposal refers to that local candidate. Its preview and validation work without a database ID or catalogue insert.
4. The user requests an alternative or leaves the chat. No foodstuff or draft has been persisted, and no cleanup action is needed.

Proposed initial data policy for discussion: keep unknown nutrition values unknown rather than infer authoritative catalogue values. Preview should represent incomplete nutrition explicitly according to domain calculation rules. This is not yet an agreed policy.

Derived needs: local candidate definitions/references, validation and preview without persistence, clear saved/unsaved presentation, and refinement that preserves reviewed candidate data. No search result is not definitive proof of absence; search and clarification should be adequate before declaring a candidate new.

### 3. Save a proposal with a missing foodstuff

The user says: "Save the second proposal as a draft."

1. The agent resolves the user's reference to an exact Kochwiki proposal reference. If it is ambiguous, it asks which proposal rather than guessing.
2. Kochwiki receives that proposal reference and the write-operation identity, then resolves its stored content, temporary definitions, and editing lineage.
3. In one transaction, it validates/reconciles dependencies, inserts required new foodstuffs, maps local references to canonical IDs, and inserts the draft.
4. On success, the chat reports the actual draft and which foodstuffs were created or reused, with a navigation action. On failure, no newly created dependencies from this operation remain.
5. A timeout does not prove that the transaction failed. Retry/result recovery must use the same operation identity rather than create another draft blindly.

Agreed interaction default: an explicit save request or existing proposal-card save action includes creating the proposal's required missing foodstuffs. No separate confirmation is needed. An unresolved identity/data conflict may require clarification about what to save, rather than an additional permission step.

Derived MVP needs: exact proposal resolution, combined domain save, atomicity, candidate-ID mapping, and reliable handling of uncertain/repeated writes. The user should not manage rollback or cleanup. Detailed delegated authorization follows during hardening.

### Companion cases

- **Reference recipe:** "Use the preparation method from my lentil bake." Semantic search and recipe retrieval can identify and show a reference artifact, clarifying competing matches. The selected pasta recipe remains the editing target. Reference retrieval is optional, not part of every conversation.
- **Standalone creation:** "Create this foodstuff now, even without saving the recipe." Resolve the exact candidate, execute a separate authorized creation operation, and show the persisted result. Later saves must account for that creation rather than duplicate it. Do not change an immutable proposal silently.

## Technical ownership

- Kochwiki owns its MCP domain surface, authoritative IDs, validation, and persistence, with permission enforcement added during later hardening. Reuse domain services for the chosen capabilities.
- Kochwiki also owns stored proposals, proposal references/lifecycle, domain instructions, and future domain skill content.
- AI Service owns generic MCP client integration, bounded execution, conversation state, and generic artifact delivery. It carries opaque references and returned content without Kochwiki model dependencies or business rules.
- The host UI renders proposals and selectively shown read-only reference artifacts and action results, and retains existing direct saving. Natural-language clarification needs no new selection or confirmation UI.
- Later Kochwiki-provided skills supply focused methods using these capabilities; AI Service supplies generic loading/application support.

MCP defines server tools, resources, and prompts; this domain-tool design is an application choice. See the official [MCP overview](https://modelcontextprotocol.io/specification/2026-07-28/basic/index) and [tool specification](https://modelcontextprotocol.io/specification/2026-07-28/server/tools). Verify SDK/protocol compatibility when implementation is scoped.

An endpoint in the existing backend and a small API-backed adapter are packaging candidates. A separate deployment is not assumed. A server alone will not integrate the recipe workflow; AI Service needs its client/adapter too.

The eventual identity model remains documented in DEC-001. Per the current product direction, detailed authentication, delegated authorization, and audit/security design are deferred to successive hardening work. They are not acceptance criteria or blocking prerequisites for the initial MVP foundation. Record this as an intentional staging choice rather than presenting the MVP as a completed production feature.

## Proposed delivery outline

1. Use the agreed conversation defaults to scope semantic lookup, optional reference recipes, selective artifact presentation, temporary foodstuffs, explicit saving, and explicit standalone creation. Defer advanced artifact actions.
2. Specify minimum tools, existing/local candidate references, errors, atomic save behavior, and write retry semantics. Map these to domain operations and missing search/preview behavior; keep detailed authorization design outside this slice.
3. Establish MCP server/client integration with one end-to-end read capability. This is an engineering increment within the intended read/write workflow, not a final read-only scope.
4. Add semantic foodstuff and recipe discovery, detailed retrieval, and retrieved-item artifacts. Preserve the source recipe/used-foodstuff snapshot and editing target.
5. Extend proposals and pure preview validation to support temporary foodstuff candidates. Verify that exploration and abandoned proposals cause no catalogue writes.
6. Add conversational and UI saving through atomic draft/dependency materialization, plus explicit standalone foodstuff creation. Exercise ambiguity, duplicates, rollback, missing nutrition, and uncertain/repeated save outcomes.
7. Introduce focused versioned skills later. Evidence-informed IBS criteria and proposal-quality evaluation remain separate later work.

Add authentication, authorization, auditing, and production security progressively as separate hardening work. Optional UI shortcuts can follow demonstrated conversational needs.

MVP acceptance is predictable workflow execution and domain effects through MCP, with correct references, validation, atomic saving, selective presentation, and natural-language clarification. Measurably better nutritional or culinary suggestions and production-grade security are not immediate acceptance criteria.

## Planning relationships

This direction expands [[AI-assisted Recipe Optimization]] beyond the original exclusions for other-recipe reading, foodstuff tools, and creating unavailable foodstuffs. Those exclusions describe the first delivered scope, not the intended next iteration.

[[Controlled Agent Actions]] provides delegated-write principles. Its universal-agent-first/read-only-first delivery sequence should not be assumed to block designing Kochwiki reads and writes together. [[Read-only Project Access for the Agent]] remains related universal-agent work, not the scope container for this full workflow.

The existing initiative and accepted decisions retain their longer-term authentication/security direction. The user's MVP sequencing explicitly defers that implementation while this foundation is developed; do not inherit those dependencies as blockers for the current slice.

[[Profile-based Recipe Constraints and Preferences]] and a general skill framework are not prerequisites. Detailed implementation specs belong in the service repositories after workflow agreement.

## Open discussion

- How should temporary foodstuff definitions and references be carried across proposal refinements and alternatives?
- Should retrieved-item artifacts be emitted by read tools or selected through a separate presentation capability?
- What semantic search examples and relevance thresholds establish useful retrieval without confusing near-matches with identical items?
- Are nutrition values limited to user-provided/verified data initially, or can labelled estimates be stored?
- Which recipe states are useful to include in initial reference discovery? Detailed access policy follows during hardening.
- Which exact-identity rules allow reusing a foodstuff created since proposal generation, and which conflicts need clarification?
- Should the first MCP surface serve only AI Service or also an external client?
