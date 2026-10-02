---
type: initiative
status: active
last_reviewed: 2026-10-02
projects:
  - "[[Kochwiki]]"
  - "[[AI Service]]"
---

# AI-assisted Recipe Optimization

## Intended outcome

A user can explore and iteratively improve an existing recipe in a focused chat,
review complete alternatives and explicitly save a chosen proposal as a draft.
Exploration creates no catalogue entries or drafts. Kochwiki owns all domain
validation and persistence; the AI cannot publish, accept, discard or delete recipes.

The current iteration improves the technical/workflow foundation. Demonstrably
better culinary or nutritional suggestions are a later outcome. Evidence-informed
IBS/gut-health methods and skill provenance remain separate future work; no medical
quality claim follows from this foundation.

## Delivered foundation

The local checkouts implement the architecture in
[[Recipe Agent Capability and Proposal Outline]]:

- A selected recipe/version and its used foodstuffs form the initialization snapshot.
- Kochwiki supplies semantic name/alias foodstuff search and name recipe search,
  including enriched results and active/draft recipe versions.
- Kochwiki's MCP provides domain tools and workflow instructions. AI Service is a
  generic conversation, MCP and artifact runtime without Kochwiki model dependencies.
- Complete proposals are validated and stored in Kochwiki memory, with source/base
  references and inline temporary foodstuff definitions. Refinement registers a
  new complete proposal. Preview does not create foodstuffs or recipes.
- Explicit saving through chat or the proposal artifact button uses the same
  atomic draft/dependency service. Repeated saves return the created version.
- Dedicated foodstuff creation and updates are available on explicit request.
- Frontend-advertised recipe and foodstuff artifacts provide selective complete
  presentations. JSON is an explicit presentation option. Artifacts are not proposals.

This supersedes the original catalogue-only snapshot, AI Service-owned proposals,
click-only saving and domain-coupled lifecycle. Detailed contracts, implementation
limits, setup and test evidence belong in the service repositories.

## Experience and ownership

The assistant may answer questions or clarify without producing a proposal.
Ambiguous matches and references are resolved through natural language; clear
matches need no extra confirmation. Consulting other recipes and presenting
retrieved results are optional, not mandatory workflow steps.

Saving a proposal includes creation of its required missing foodstuffs without
separate confirmation. The user reviews and publishes drafts through Kochwiki's
normal recipe versioning flow. Existing live foodstuff/source references are
accepted; missing dependencies may make preview or saving fail.

Kochwiki owns proposals, domain instructions, future skills, calculation and writes.
AI Service owns generic execution and ephemeral conversations. The frontend owns
renderers, presentation schemas and domain actions. See the architecture note for
the division between workflow instructions, tool contracts and artifact descriptions.

## Status and next work

The agreed MVP foundation is implemented; manual end-to-end verification remains
with the user. Automated tests use scripted models and do not prove real-model
workflow behavior or recipe quality. Execution and conversation budgets have been
raised so ordinary workflows have more room.

Tool-result retention across turns is the highest-priority technical follow-up,
but is deliberately deferred now. Later turns currently receive final replies
rather than tool transcripts. Other deferred improvements are temporary-ingredient
visualization and reconciliation of temporary definitions with later catalogue writes.

Skills and recipe-quality evaluation, durable conversation/outcome storage,
streaming/cancellation and advanced UI shortcuts can be scoped separately.
Authentication, delegated authorization and auditing remain required for wider
production exposure; they are not blockers for developing the current private
MVP foundation. [[Browser Authentication Foundation]] and
[[Controlled Agent Actions]] describe that longer-term direction.

No additional capability slice is selected now. Manual usage should inform the
next implementation priority. [[Recipe Optimization Skills and Kochwiki Tools]]
records the scope expansion that led to this architecture.
