---
type: initiative
status: planned
last_reviewed: 2026-10-02
projects:
  - "[[AI Service]]"
---

# Universal Agent Foundation

## Intended outcome

The AI Service provides an initial universal agent that can answer generic questions and retrieve a live weather forecast through an explicit tool.

## Motivation

Establish the conversational and tool-use foundation before granting the agent access to project data or actions.

## Current foundation

The AI Service `staging` branch already has a generic, session-based conversation HTTP contract, an ephemeral session store, bounded tool orchestration, typed artifacts, a reusable chat UI package, and a deterministic demo agent with example tools. The model-backed agent is configured for recipe improvement, not general questions. There is no universal agent configuration or live weather tool yet; implementation of this initiative has not begun as a distinct user-facing outcome.

## Scope

- Implement the universal agent within the AI Service
- Answer generic questions using the configured AI capabilities
- Reuse the committed [[ChatGPT Plan Integration]] provider mode for subscription-backed inference, alongside explicit API-key access; its OAuth and streaming work is a separate reusable initiative
- Retrieve live weather information through a tool or service
- Reuse [[Web Information Retrieval]] for external research as a separately committed capability; prefer OpenAI Web Search after checking provider/model support
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

Reuse the existing conversation and tool boundaries to define a generic model-backed agent configuration, select a live weather source, and implement and verify one weather tool and its user-facing interaction.
