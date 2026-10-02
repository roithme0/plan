---
type: initiative
status: planned
last_reviewed: 2026-10-02
projects:
  - "[[AI Service]]"
  - "[[Kochwiki]]"
---

# Web Information Retrieval

## Intended outcome

Recipe conversations in Kochwiki and the universal agent can obtain external information from the web when needed. This is committed planned work, not an exploratory idea. OpenAI's built-in Web Search in the Responses API is the preferred initial implementation; final provider selection follows a compatibility check.

## Motivation

Let the agent research information beyond saved project data and model memory, including recipe inspiration, ingredient alternatives and current information. Reuse a hosted search capability rather than maintaining a search engine or browser automation stack.

## Current state

Web information retrieval is not implemented in the planning baseline. AI Service already has a generic provider adapter and conversation/tool runtime; implementation details remain authoritative in its service repository.

## Scope

- Add optional web information retrieval in AI Service, reusable by Kochwiki recipe conversations and the universal agent.
- Prefer the built-in Responses API `web_search` tool with a supported model. OpenAI executes this hosted tool; it does not require a separate search MCP server or a locally implemented search execution loop.
- Define agent guidance for when to search: explicit research requests, current or changing information, and questions requiring external evidence. Let the model choose whether to search for ordinary turns.
- Preserve source URLs and citation annotations through the conversation boundary and show clearly visible, clickable citations in the chat UI.
- Account for additional search usage and costs; make unsupported provider/model combinations and search failures clear.
- Treat retrieved content as external evidence, not agent instructions. Kochwiki's existing validation and explicit proposal/draft saving continue to govern any resulting domain changes.

## Out of scope

- Browser UI interaction, clicks, form submission or website actions
- Playwright MCP, Computer Use or a self-hosted browser for this outcome
- Building a custom search engine or introducing a separate search MCP server by default

## Project roles

- [[AI Service]] owns provider capability checks, hosted-tool configuration, general search guidance, result/citation delivery and usage handling.
- [[Kochwiki]] owns recipe-specific guidance, presentation in its recipe chat and validation/persistence of domain proposals.
- [[Universal Agent Foundation]] reuses the same optional retrieval capability for generic questions.
- [[Shared Agent Chat UI and Domain Rendering]] is the relevant shared UI boundary for displaying source links.

## Acceptance criteria

- An explicitly requested web research task produces an answer grounded in retrieved information with clickable source citations.
- Both recipe conversations and the universal agent can enable the shared capability without duplicating provider integration in Kochwiki.
- Ordinary questions can complete without a search; current-information instructions trigger retrieval when supported.
- Unavailable search capability or failed retrieval is reported without presenting model memory as verified web evidence.
- Search usage is included in the applicable cost/usage handling.

## Open implementation questions

- Which configured models support the hosted tool, and how should capability availability be exposed?
- Does the provider mode introduced by [[ChatGPT Plan Integration]] support hosted Web Search? Ordinary API support does not establish subscription-path availability or billing coverage. Verify this independently; keep API-key mode explicit and avoid silent paid fallback.
- If the subscription path cannot use the hosted tool, should web retrieval initially be limited to API-key mode or use an independently configured search provider through a custom tool/MCP?
- Which citation and tool-progress fields must pass through the existing conversation/UI contract?

## Next step

Inspect AI Service's Responses adapter and chat response contract. Verify model/provider support, enable hosted Web Search for one supported configuration, and validate a research turn with source citations in the recipe chat. Reuse the integration for the universal agent as its generic configuration becomes available.

## Sources

- [OpenAI Web Search guide](https://developers.openai.com/api/docs/guides/tools-web-search) — reviewed on 2026-10-02
- [[ChatGPT Plan Integration]] — separate provider compatibility constraints
