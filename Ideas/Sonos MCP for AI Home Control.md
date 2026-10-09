---
type: idea
status: exploring
last_reviewed: 2026-10-09
projects:
  - "[[Home Assistant]]"
---

# Sonos MCP for AI Home Control

## Direction

Explore using a Sonos MCP server alongside the planned Home Assistant MCP integration to support AI-assisted home audio control.

This is an uncommitted home-control idea, separate from recipe, nutrition, and training planning. No particular Sonos MCP implementation is selected, and no integration is asserted as implemented.

## Motivation

An assistant that helps control the home could also help manage listening across Sonos speakers. A dedicated Sonos connector may complement Home Assistant if it offers useful audio capabilities beyond the selected Home Assistant MCP surface.

## Candidate use cases

Subject to verification of the chosen server's actual tools:

- Ask what is playing and on which speakers.
- Start, pause, or resume playback and adjust volume in a selected room.
- Select content or coordinate playback across rooms.
- Combine audio requests with other home actions, such as preparing lighting and music for an evening.

These are desired possibilities, not verified server capabilities.

## Relationship to Home Assistant

Evaluate this alongside [[Agent Access to Kochwiki and Home Assistant]], while keeping it a separate exploratory idea. Compare direct Sonos MCP access with Sonos control exposed through Home Assistant before adding another connector.

Clarify which interface should execute overlapping audio actions so a request does not produce duplicate or conflicting changes. Existing principles for scoped access, represented-user authorization, confirmation where appropriate, and domain validation also apply to any eventual connector.

## Open questions

- Which Sonos MCP implementation should be evaluated, and which tools does it actually expose?
- Does direct Sonos access add useful capabilities beyond Home Assistant's available audio-control tools?
- Which speakers, playback sources, and actions should be accessible?
- How should combined home/audio requests handle partial success or unavailable devices?

## Next step

Retain this as an idea. When the Home Assistant integration is taken up, compare its available Sonos control with a candidate Sonos MCP server and choose one small audio-control use case before committing to implementation.
