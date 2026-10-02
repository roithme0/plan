---
type: initiative
status: planned
last_reviewed: 2026-10-02
projects:
  - "[[AI Service]]"
  - "[[Kochwiki]]"
---

# ChatGPT Plan Integration

## Intended outcome

Users can connect their ChatGPT Plus or Pro account to AI Service and use its included ChatGPT Work/Codex allowance for eligible model requests in Kochwiki recipe conversations and the universal agent. This is committed planned work, not an exploratory idea. API-key access remains an explicit alternative.

## Motivation and planning assumption

Reuse an existing ChatGPT subscription for personal AI workflows without requiring a separate API key for eligible requests. Plan usage shares the user's existing allowance and remains subject to account, model and app limits; it is not unlimited usage.

Plan the implementation for the existing private, non-commercial deployment under the user's assumption that this use is eligible. This records the agreed planning assumption, not a confirmed OpenAI eligibility determination. The public developer flow documents open-source/local apps and self-hosted VMs; paid or remotely offered applications have a separate partner path.

## Current state

This integration is not implemented. The existing generic provider adapter and conversation/MCP runtime provide the reuse boundary. Streaming is currently deferred in the planning baseline; it becomes a prerequisite for the ChatGPT-plan provider path. The existing Responses API usage discussed for AI Service should be checked against the requirements below in the service repository before implementation.

## Scope

- Add ChatGPT plan access as an AI Service provider/authentication mode reusable by recipe conversations and the universal agent; keep domain services independent of provider credentials.
- Implement the documented Sign in with ChatGPT OAuth flow with PKCE, account consent and a persistent, opaque host identifier. Store the issued client registration and credentials securely, refresh tokens, and handle expiry, revocation, reconnection and disconnect.
- Support the selected self-hosted deployment using the documented VM credential flow where appropriate. Bind the connection to the intended application user and selected ChatGPT account/workspace.
- Request model inference through the public Responses API using the OAuth access token as the bearer credential. Discover account-available models rather than assuming the ordinary API-key model catalog applies.
- Implement streaming consumption and carry relevant progress/results through the existing conversation/UI boundary. Keep recipe artifacts and explicit proposal saving compatible with the existing workflow.
- Distinguish provider connection from application login: this grants model access and usage consent, not Kochwiki ownership permissions or access to ChatGPT conversation history/memories. Preserve [[DEC-002 External OIDC Provider for Human Authentication]].
- Surface connection, model availability and usage-limit failures clearly. Never silently switch an exhausted plan to API-key billing. Any fallback or use of additional paid credits requires an explicit user choice.

## Technical constraints for the initial provider path

The current documented flow is a preview; keep the adapter requirements in service documentation and recheck them when implementing.

- Set `stream: true` and `store: false` on HTTP inference requests.
- Send the needed conversation context in an `input` array, including tool calls and results needed for subsequent turns. HTTP `previous_response_id` and persistent provider-side conversation storage are unavailable. Coordinate with the already deferred tool-result-retention work.
- Use `instructions` or developer messages for agent guidance; explicit system message items are rejected in this flow.
- Filter unsupported fields, including `temperature`, `top_p`, `max_output_tokens`, `max_tool_calls`, `background` and `conversation`. The linked preview reference is authoritative for the complete list.
- Use the supported function/custom-tool format (namespaces or `additional_tools` input items). AI Service discovers and executes Kochwiki/Home Assistant MCP tools itself, then returns their results to the model. Hosted Responses MCP/connectors and Responses `tool_search` are unsupported in this flow.
- Do not assume image generation, file search, Code Interpreter, audio/transcription or other ordinary API features are covered. This initiative covers eligible Responses inference; embeddings for Kochwiki semantic search and image-generation capabilities keep their separate provider/billing paths unless independently supported later.

## Project roles

- [[AI Service]] owns OAuth connection lifecycle, credential isolation, provider adaptation, model discovery, streaming, conversation/tool context and error handling.
- [[Kochwiki]] supplies the user-facing entry point for its recipe workflow and retains domain tools, validation, artifacts and draft persistence. It does not receive OpenAI credentials in domain payloads.
- [[Universal Agent Foundation]] reuses the same provider connection and runtime; subscription support does not require a separate agent implementation.

## Acceptance criteria

- A connected Plus/Pro account completes a streamed recipe conversation and a generic agent turn through Responses using OAuth credentials without an API key for those requests.
- A multi-turn MCP-backed conversation preserves the tool context required for refinement and delivers the same validated domain artifacts and explicit save behavior.
- Token refresh and reconnection work for the selected deployment; credentials remain isolated from frontend/domain payloads and other users.
- Unavailable models, revoked connections and exhausted allowances produce actionable errors without automatic paid fallback.
- API-key mode remains usable as an explicit alternative, and existing application authentication/authorization boundaries are preserved.

## Open implementation questions

- Where should connection management be presented: Kochwiki settings, a shared AI Service surface, or both?
- How should the existing application session map to a provider account during the private MVP and later authenticated use?
- Which documented local/self-hosted registration and credential flow fits the actual deployment topology?
- Which current request fields and tool schemas need adaptation, and which streaming events should the chat UI expose first?

## Next step

Inspect AI Service's existing Responses adapter and conversation/UI contract. Define the ChatGPT-plan adapter, account connection lifecycle and request compatibility changes, then implement one streamed OAuth-backed turn before verifying multi-turn MCP calls and recipe artifacts. Streaming and tool-result retention are explicit prerequisites for this provider slice; a live-weather tool or new domain capability is not required to start it.

## Sources

Official OpenAI documentation reviewed on 2026-10-02:

- [Using your ChatGPT plan in other apps and sites](https://help.openai.com/de-de/articles/20001542-using-your-chatgpt-plan-in-other-apps-and-sites)
- [ChatGPT plan usage overview](https://developers.openai.com/siwc/token-sharing-open-source)
- [Registration and sign-in](https://developers.openai.com/siwc/token-sharing-open-source/sign-in)
- [Models and inference](https://developers.openai.com/siwc/token-sharing-open-source/models-and-inference)
- [Self-hosted VMs](https://developers.openai.com/siwc/token-sharing-open-source/self-hosted-vms)
- [Preview limitations](https://developers.openai.com/siwc/token-sharing-open-source/preview-limitations)
