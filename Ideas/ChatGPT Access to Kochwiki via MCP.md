---
type: idea
status: exploring
last_reviewed: 2026-10-09
projects:
  - "[[Kochwiki]]"
---

# ChatGPT Access to Kochwiki via MCP

## Direction

Keep the option of using Kochwiki directly from ChatGPT, including the mobile app, through a personal custom MCP plugin.

This is an uncommitted possibility retained for future consideration. No implementation decision, priority, or delivery commitment has been made.

## Potential value

- Search saved recipes and foodstuffs and retrieve their details during ordinary ChatGPT conversations on a phone.
- Discuss recipes using authoritative Kochwiki data without manually copying it into the chat.
- Potentially create recipe proposals or drafts later, while preserving explicit saving and user-controlled publication.

## Documented integration route

Official OpenAI documentation reviewed on 2026-10-09 describes adding a custom MCP server on ChatGPT's web interface under Plugins, configuring its connection and authentication, and installing it as a personal plugin. The plugin documentation states that account-available plugins can be used in Chat and Work on mobile. Account availability and a real mobile end-to-end connection have not been verified for this deployment.

ChatGPT supports SSE and Streamable HTTP, OAuth authentication, and both read and write tools. Write actions require confirmation by default. A custom UI is optional; tools alone could support an initial integration. A personal connection does not require publication in the public plugin directory.

The MCP endpoint must be reachable by ChatGPT through a suitable HTTPS deployment or supported tunnel. A phone's access to Kochwiki over the private LAN or VPN does not by itself establish ChatGPT server access. Connection, authentication, and hosting details remain to be evaluated if the idea is pursued.

## Relationship to existing work

[[Recipe Agent Capability and Proposal Outline]] describes Kochwiki's existing MCP workflow foundation. Evaluate reuse of its domain tools and guidance rather than assuming the current AI Service/frontend integration is automatically compatible with ChatGPT, especially for proposal presentation and saving.

This is distinct from [[ChatGPT Plan Integration]]: here ChatGPT hosts the conversation and calls Kochwiki tools; that initiative lets the AI Service use a user's ChatGPT plan for model inference. This option does not resolve or change that initiative's authorization blocker, and does not require the AI Service as the conversation runtime.

Kochwiki continues to own domain validation, persistence, and permissions. Its current private-network boundary and temporary user selection are not adequate delegated authorization for an externally reachable ChatGPT integration. Reuse the direction in [[Browser Authentication Foundation]] and [[DEC-001 Shared Foodstuffs and Owned Recipes]] before exposing user data or mutations.

## Open questions

- Is custom MCP plugin creation available to the intended account, and does the personal plugin work in its mobile app?
- Which authenticated, narrowly scoped read tools would be useful first?
- Which hosting or tunnel route fits the private deployment, and how will Kochwiki validate the acting user's identity and permissions?
- Can existing proposal tools work independently of Kochwiki's Angular renderer and AI Service presentation tools, or is a ChatGPT-specific adapter needed?
- Is recipe and foodstuff retrieval enough, or would explicitly confirmed proposal/draft saving justify further work?

## Next step

Keep this note as a reminder. If the user decides to pursue it, first verify account availability and a small authenticated read-only mobile workflow, then decide whether to promote it to an initiative. No implementation is currently scheduled.

## Sources

- [Add custom MCP server](https://developers.openai.com/api/docs/guides/custom-mcp-server)
- [Plugins in ChatGPT and Codex, including mobile support](https://learn.chatgpt.com/docs/plugins)
- [MCP server and UI quickstart](https://developers.openai.com/plugins/build/app-quickstart)
