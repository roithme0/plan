---
type: initiative
status: active
last_reviewed: 2026-10-09
projects:
  - "[[AI Service]]"
  - "[[Kochwiki]]"
---

# Web Information Retrieval

## Intended outcome

Recipe conversations in Kochwiki and the universal agent can obtain external information from the web when needed. The initial capability is implemented using OpenAI's hosted Web Search in the Responses API. Citation presentation and rollout into a future universal agent remain open.

## Motivation

Let the agent research information beyond saved project data and model memory, including recipe inspiration, ingredient alternatives and current information. Reuse a hosted search capability rather than maintaining a search engine or browser automation stack.

## Current state

The hosted-search MVP is implemented on AI Service's `staging` branch, reviewed on 2026-10-09. The configured Kochwiki model agent enables `WebSearchConfig`; the generic model runtime advertises `web_search` to OpenAI, which executes the hosted tool. This does not require a search MCP server.

The model chooses when to search. A separate per-turn budget defaults to sixteen hosted calls, including page-open/find actions. Subsequent requests omit search once the allowance is exhausted while other tools can continue. Hosted search outcomes are retained in model history and exposed as ordinary `web_search` timeline activity.

The backend README explicitly defers citation rendering: source/citation metadata remains internal, so clickable source citations are not yet delivered through the public chat contract. A general universal agent is not yet configured; the reusable capability exists, but that rollout and the remaining acceptance criteria below are not complete. Model/account compatibility, subscription-path coverage, and search-specific usage/cost presentation remain to be verified rather than inferred from the API-key implementation.

## Scope

- Add optional web information retrieval in AI Service, reusable by Kochwiki recipe conversations and the universal agent.
- Prefer the built-in Responses API `web_search` tool with a supported model. OpenAI executes this hosted tool; it does not require a separate search MCP server or a locally implemented search execution loop.
- Define agent guidance for when to search: explicit research requests, current or changing information, and questions requiring external evidence. Let the model choose whether to search for ordinary turns.
- Preserve source URLs and citation annotations through the conversation boundary and show clearly visible, clickable citations in the chat UI.
- Surface hosted search activity while the turn runs through the shared tool-call transparency requirement in [[Shared Agent Chat UI and Domain Rendering]]. Show available provider progress and outcomes without implying visibility into every internal search step.
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
- How should retained source/citation metadata reach the public conversation contract and clickable chat citations, and what additional hosted progress should the timeline expose?

## Next step

Complete citation delivery and clickable source presentation, verify live search behavior and usage handling in the recipe chat, and enable the existing shared capability when the universal agent is configured. Keep ChatGPT-plan compatibility separate from the implemented API-key path.

## Implementation evidence

Reviewed tracked code, documentation, and existing regression coverage on `staging`; those tests and live OpenAI searches were not rerun for this documentation-only update.

- [AI Service hosted search behavior and known limits](https://github.com/roithme0/ai-service/blob/3cff08f9b0e0b0b528de7db6fff7b283dd22f2c0/backend/README.md)
- [Kochwiki agent enabling hosted search](https://github.com/roithme0/ai-service/blob/3cff08f9b0e0b0b528de7db6fff7b283dd22f2c0/backend/app/agents/wiring.py)
- [Shared hosted-search/tool loop](https://github.com/roithme0/ai-service/blob/3cff08f9b0e0b0b528de7db6fff7b283dd22f2c0/backend/app/agents/tool_turns.py)
- [Hosted-search regression coverage](https://github.com/roithme0/ai-service/blob/3cff08f9b0e0b0b528de7db6fff7b283dd22f2c0/backend/tests/test_web_search.py)

## Sources

- [OpenAI Web Search guide](https://developers.openai.com/api/docs/guides/tools-web-search) — reviewed on 2026-10-02
- [[ChatGPT Plan Integration]] — separate provider compatibility constraints
