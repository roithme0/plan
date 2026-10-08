---
type: project
status: active
last_reviewed: 2026-10-09
---

# Home Assistant

## Purpose

Provide home automation and access to the devices and entities in the local environment.

## Current role

A Home Assistant instance is running in the network.

## Direction

Remain authoritative for home state and automation while exposing selected data and actions to the universal agent through explicit, authorized interfaces.

## MCP integration direction

Use the official Home Assistant **Model Context Protocol Server** integration as the preferred connection for [[AI Service]], under [[Agent Access to Kochwiki and Home Assistant]], initially with selected reads. This is planned integration, not a claim that MCP is enabled on the running instance. It is the server integration; the separate Home Assistant MCP client integration serves the opposite direction.

### Available surface

The selected Home Assistant LLM API supplies MCP tools and prompts. Built-in Assist exposes selected entities for state questions and device control, such as switching lights, without administrative tasks. A context-snapshot resource is available when the API includes `GetLiveContext`. Sampling and notifications are unsupported.

The documented transport is stateless Streamable HTTP at `/api/mcp`, with OAuth or a long-lived access token. Available details depend on the installed Home Assistant version and selected API.

### Responsibilities and rollout

- Home Assistant owns entity exposure, home state, and action execution.
- AI Service uses its generic MCP client/runtime for discovery, instructions, calls, and results; avoid a duplicate custom Home Assistant tool server where the official surface suffices.
- Begin with selected state queries. Verify the actual read surface and enforce a read-tool allowlist; entity exposure alone does not establish read-only access.
- Add selected actions later through [[Agent Access to Kochwiki and Home Assistant]], preserving its authorization and audit requirements.
- Direct connectivity from AI Service within the network can suffice; public internet exposure is not a requirement of this architecture.
- Home Assistant authentication and represented AI Service user identity need an explicit mapping; a connector token does not establish per-user delegation.

### Verification before implementation

Check installed-version support, discovered tools and prompts, accessible state, and read-only enforcement. Do not assume arbitrary history, logs, configuration editing, or automation management are included. Evaluate a separate extension only for a concrete missing capability.

Official documentation reviewed on 2026-10-01:

- [MCP Server integration](https://www.home-assistant.io/integrations/mcp_server/)
- [Home Assistant LLM API](https://developers.home-assistant.io/docs/core/llm/)

## Ecosystem relationships

See [[System Overview]].

## Related initiatives

- [[Agent Access to Kochwiki and Home Assistant]] — Home Assistant connector planned; selected reads first, controlled actions later

## Open questions

- Which entities and state should the agent initially be allowed to read?
- Which actions should eventually be allowed, and which should require confirmation?
