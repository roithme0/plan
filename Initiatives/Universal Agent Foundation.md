---
type: initiative
status: planned
last_reviewed: 2026-09-10
projects:
  - "[[AI Service]]"
---

# Universal Agent Foundation

## Intended outcome

The AI Service provides an initial universal agent that can answer generic questions and retrieve a live weather forecast through an explicit tool.

## Motivation

Establish the conversational and tool-use foundation before granting the agent access to project data or actions.

## Scope

- Implement the universal agent within the AI Service
- Answer generic questions using the configured AI capabilities
- Retrieve live weather information through a tool or service
- Make tool failures or unavailable live data clear to the user
- Establish a tool interface that can later support project connectors
- Keep the initial conversation boundary compatible with future typed domain artifacts without requiring their renderer system now

## Out of scope

- Kochwiki or Home Assistant access
- Actions affecting project or home state
- Broad network, database, or filesystem access
- Deciding how a shared chat UI or domain renderer system is packaged and distributed

## Project roles

- [[AI Service]] owns agent orchestration, tool execution, model integration, and the initial agent interaction surface.

## Related ideas

- [[Shared Agent Chat UI and Domain Rendering]]

## Open questions

- Which source should provide weather data?
- Through which interface will users first access the agent?
- What is the smallest structured response envelope that keeps future domain artifacts possible without prematurely designing their renderers?

## Next step

Define the smallest agent, conversation, and tool boundary needed for generic questions and one live weather tool.
