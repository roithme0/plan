---
type: idea
status: exploring
last_reviewed: 2026-10-01
projects:
  - "[[Kochwiki]]"
---

# Semantic Recipe and Foodstuff Search in the UI

## Direction

Explore reusing Kochwiki's planned semantic recipe and foodstuff discovery for the ordinary UI search, beyond its use as agent tools during recipe optimization.

The existing UI search compares the search text directly with recipe or foodstuff names, as reported in the product discussion. Semantic retrieval could help users find relevant existing entries without knowing their exact names.

This is an uncommitted idea, not an accepted initiative or a requirement for the current recipe-optimization work.

## Relationship to recipe optimization

[[Recipe Agent Capability and Proposal Outline]] proposes `search_foodstuffs` and `search_recipes` as Kochwiki-owned MCP capabilities. [[Recipe Optimization Skills and Kochwiki Tools]] describes their use for discovering additional ingredients and optional reference recipes.

The UI could reuse the underlying Kochwiki search capability and index through its normal application API. Sharing search logic does not require the frontend to become an MCP client or route ordinary searches through an AI Service conversation.

The agent search is planned architecture, not evidence that semantic search is already implemented. UI-specific result presentation and search behavior would be evaluated separately.

## Potential value

- Find recipes from descriptions or paraphrases rather than only their saved names.
- Find foodstuffs using alternative terms or descriptions when direct name matching misses useful candidates.
- Reuse retrieval work across agent tools and manual search rather than maintaining separate semantic-search implementations.

Results still represent existing catalogue entries or recipes. Semantic similarity does not establish that two foodstuffs are identical or interchangeable.

## Open questions

- Should semantic retrieval complement name matching, provide an optional mode, or become the default?
- How should exact name matches, partial matches, and semantically related results be ranked together?
- Which recipe and foodstuff fields provide useful search context, and which query examples establish acceptable relevance?
- Which recipe states should UI search include? The architecture's proposed initial agent search covers active recipes; UI coverage remains to be decided.
- What latency, cost, index freshness, and fallback behavior are acceptable for interactive search?

Existing visibility rules and explicit filters should continue to constrain results. A query such as "high-protein" may express search intent, but similarity ranking alone should not be treated as a guaranteed nutritional filter.

## Next step

Revisit when the semantic search capability for recipe optimization is concrete enough to evaluate reuse. Compare representative UI queries with the current name search before deciding whether to promote this idea to an initiative.
