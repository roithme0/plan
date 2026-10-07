---
type: initiative
status: blocked
last_reviewed: 2026-10-07
projects:
  - "[[AI Service]]"
  - "[[Kochwiki]]"
---

# ChatGPT Plan Integration

## Intended outcome

Users can connect their ChatGPT Plus or Pro account to AI Service and use its included ChatGPT Work/Codex allowance for eligible model requests in Kochwiki recipe conversations and the universal agent. This remains a committed desired outcome, but implementation is blocked by the browser-only authorization limitation below. The first MVP targets existing Kochwiki chat with browser-only connection on desktop and mobile; the universal agent reuses the provider later. The configured API key remains the baseline for users without a connected plan.

## Motivation and planning assumption

Reuse an existing ChatGPT subscription for personal AI workflows without requiring a separate API key for eligible requests. Plan usage shares the user's existing allowance and remains subject to account, model and app limits; it is not unlimited usage.

Plan the implementation for the existing private, non-commercial deployment under the user's assumption that this use is eligible. This records the agreed planning assumption, not a confirmed OpenAI eligibility determination. The public developer flow documents open-source/local apps and self-hosted VMs; paid or remotely offered applications have a separate partner path.

## Current state

Blocked as of 2026-10-07; subscription integration is not implemented. AI Service has delivered ordered model/tool history, end-to-end streaming, application identity carriage, and conversation ownership. Selected-user Kochwiki integration is implemented and locally verified; compatible chat-library release and dependency adoption remain a rollout dependency. Failure reconciliation and deliberate retry are intentionally deferred, and mid-chat recovery does not gate the intended MVP. Provider connection ownership, persistent credentials, refresh, and subscription inference remain intended MVP work after authorization is unblocked.

Initial ownership uses the selected Kochwiki user's trusted `kochwiki:<stable-user-id>` assertion within the private LAN. Provider consent stays separate from application identity; verified shared SSO remains later work. Each user may connect their own plan. Unconnected users may use the deployment's API-key baseline; plan failure must not trigger automatic API-key fallback.

See [AI Service's concept](../../ai-service/docs/concepts/2026-10-03-chatgpt-plan-integration.md) for service-level decisions.

## Blocker: Browser-only plan authorization

The MVP requires browser-only connection from Kochwiki on desktop and mobile, with no callback listener, setup command, or helper installed on the user's device. The deployment is a private, non-commercial LAN server. Local authorization followed by credential transfer does not meet this requirement, and partner integration is not a viable path for this use case.

The documented public ChatGPT-plan OAuth flow requires an HTTP loopback callback on `127.0.0.1` on the computer running the browser. It cannot be replaced with Kochwiki's or AI Service's LAN-server callback. The documented self-hosted procedure still authorizes locally before transferring credentials. This prevents the intended browser-only desktop/mobile connection flow. [Registration and sign-in](https://developers.openai.com/siwc/token-sharing-open-source/sign-in#2-start-authorization), [self-hosted VMs](https://developers.openai.com/siwc/token-sharing-open-source/self-hosted-vms).

The separate website flow supports registered remote callbacks but is documented for selected partners and identity scopes. Website identity login alone does not grant permission to use the user's ChatGPT allowance for inference. No supported browser-only remote-callback plan-authorization path has been established for this deployment. This is a limitation of the currently documented integration route, not a claim that OAuth cannot support the architecture in principle. [Website sign-in](https://developers.openai.com/siwc/website), [client registration](https://developers.openai.com/siwc/request-client-id).

Revisit implementation only when a documented and available route supports both the remote callback and explicit ChatGPT-plan-use authorization for this personal deployment without local software. The intended flow remains Kochwiki browser → OpenAI login/consent → remote callback with an authorization code → AI Service exchanges and validates the code, stores credentials per application user, and refreshes them server-side. Provider tokens should not pass through Kochwiki frontend JavaScript. API-key chat remains available; this blocker neither removes that path nor authorizes automatic paid fallback.

## Scope

- Add ChatGPT plan access as an AI Service provider/authentication mode reusable by recipe conversations and the universal agent; keep domain services independent of provider credentials.
- Once a supported browser-only remote-callback route is available, implement ChatGPT-plan OAuth with PKCE, explicit plan-use consent and a persistent, opaque host identifier. Store the issued client registration and credentials securely, refresh tokens, and handle expiry, revocation, reconnection and disconnect.
- Support the LAN deployment with browser-only authorization from desktop and mobile. Local listeners, helper installation, and local-to-VM credential transfer do not meet this MVP requirement. Bind each connection to its application owner and validated ChatGPT account/workspace.
- Request model inference through the public Responses API using the OAuth access token as the bearer credential. Discover account-available models rather than assuming the ordinary API-key model catalog applies.
- Implement streaming consumption and carry text plus live tool-call activity through the existing conversation/UI boundary. Follow the committed tool-call transparency requirement in [[Shared Agent Chat UI and Domain Rendering]]: understandable action/input summaries, preparation versus execution, and completion/error/cancellation outcomes. Keep recipe artifacts and explicit proposal saving compatible with the existing workflow.
- Distinguish provider connection from application login: this grants model access and usage consent, not Kochwiki ownership permissions or access to ChatGPT conversation history/memories. Preserve [[DEC-002 External OIDC Provider for Human Authentication]].
- Surface connection, model availability and usage-limit failures clearly. Never silently switch an exhausted plan to API-key billing. Any fallback or use of additional paid credits requires an explicit user choice.

## Technical constraints for the initial provider path

The current documented flow is a preview; keep the adapter requirements in service documentation and recheck them when implementing.

- Set `stream: true` and `store: false` on HTTP inference requests.
- Send the needed conversation context in an `input` array, including tool calls and results needed for subsequent turns. HTTP `previous_response_id` and persistent provider-side conversation storage are unavailable. Reuse AI Service's delivered ordered model/tool history.
- Use `instructions` or developer messages for agent guidance; explicit system message items are rejected in this flow.
- Filter unsupported fields, including `temperature`, `top_p`, `max_output_tokens`, `max_tool_calls`, `background` and `conversation`. The linked preview reference is authoritative for the complete list.
- Use the supported function/custom-tool format (namespaces or `additional_tools` input items). AI Service discovers and executes Kochwiki/Home Assistant MCP tools itself, then returns their results to the model. Hosted Responses MCP/connectors and Responses `tool_search` are unsupported in this flow.
- Verify hosted Web Search availability and usage coverage independently for [[Web Information Retrieval]]. Ordinary Responses API support does not establish support through this subscription path; do not silently fall back to paid API-key access.
- Do not assume image generation, file search, Code Interpreter, audio/transcription or other ordinary API features are covered. This initiative covers eligible Responses inference; embeddings for Kochwiki semantic search and image-generation capabilities keep their separate provider/billing paths unless independently supported later.

## Project roles

- [[AI Service]] owns OAuth connection lifecycle, credential isolation, provider adaptation, model discovery, streaming, conversation/tool context and error handling.
- [[Kochwiki]] supplies the user-facing entry point for its recipe workflow and retains domain tools, validation, artifacts and draft persistence. It does not receive OpenAI credentials in domain payloads.
- [[Universal Agent Foundation]] reuses the same provider connection and runtime; subscription support does not require a separate agent implementation.

## Acceptance criteria

- A desktop or mobile browser user connects from Kochwiki through OpenAI login/consent and returns to a remote callback without installing local software. AI Service exchanges the authorization code and stores/refreshes provider credentials server-side. This criterion is currently blocked by the documented public flow.
- A connected Plus/Pro account completes a streamed recipe conversation and a generic agent turn through Responses using OAuth credentials without an API key for those requests.
- A multi-turn MCP-backed conversation preserves the tool context required for refinement and delivers the same validated domain artifacts and explicit save behavior.
- Tool activity appears while a turn runs, before its final answer; repeated/parallel calls stay distinct and execution outcomes and failures are visible. Provider HTTP streaming alone does not satisfy this UI requirement.
- Token refresh and reconnection work for the selected deployment; credentials remain isolated from frontend/domain payloads and other users.
- Unavailable models, revoked connections and exhausted allowances produce actionable errors without automatic paid fallback.
- API-key mode remains usable as an explicit alternative, and existing application authentication/authorization boundaries are preserved.

## Open implementation questions

- When will a documented, available route support remote-callback ChatGPT-plan authorization for this personal deployment without local software? Both callback support and explicit permission to consume the user's allowance are required.
- After that blocker is resolved, where should Kochwiki expose connection/status controls, and what protected persistent backend credential store and account-model default fit the deployment?
- How should deferred mid-chat recovery and deliberate retry be delivered later without repeating completed or uncertain domain actions?

## Next step

Keep subscription implementation blocked until a documented and available route meets browser-only desktop/mobile authorization with a remote callback and explicit ChatGPT-plan-use permission for this personal deployment. Do not pursue local-helper implementation or partner registration as substitutes. Recheck official capability documentation when support changes; then resume the connection lifecycle and adapter work against the existing conversation foundation. API-key chat remains available independently.

## Sources

Authorization blocker reviewed against official OpenAI documentation on 2026-10-07:

- [Using your ChatGPT plan in other apps and sites](https://help.openai.com/de-de/articles/20001542-using-your-chatgpt-plan-in-other-apps-and-sites)
- [ChatGPT plan usage overview](https://developers.openai.com/siwc/token-sharing-open-source)
- [Registration and sign-in](https://developers.openai.com/siwc/token-sharing-open-source/sign-in)
- [Models and inference](https://developers.openai.com/siwc/token-sharing-open-source/models-and-inference)
- [Website identity sign-in](https://developers.openai.com/siwc/website)
- [Client registration](https://developers.openai.com/siwc/request-client-id)
- [Self-hosted VMs](https://developers.openai.com/siwc/token-sharing-open-source/self-hosted-vms)
- [Preview limitations](https://developers.openai.com/siwc/token-sharing-open-source/preview-limitations)
