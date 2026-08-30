---
type: initiative
status: planned
last_reviewed: 2026-08-30
projects:
  - "[[AI Service]]"
  - "[[Kochwiki]]"
  - "[[Home Assistant]]"
---

# Read-only Project Access for the Agent

## Intended outcome

The universal agent can answer useful questions using authorized data from Kochwiki and Home Assistant without changing either system.

## Motivation

Enable requests such as asking about recipes, home state, or what to cook while establishing safe service boundaries before actions are introduced.

## Scope

- Introduce dedicated, scoped machine identities or credentials
- Expose constrained Kochwiki read capabilities through an intentional agent-facing contract
- Allow access only to selected Home Assistant entities and state
- Access both projects through their APIs rather than their databases or filesystems
- Keep the entire initiative read-only
- Make limitations in available source data visible in answers

## Out of scope

- Reusing a remembered browser user as the agent identity
- Treating Kochwiki's current unrestricted CRUD API as the final agent contract
- Modifying recipes, entities, devices, or automations

## Project roles

- [[AI Service]] owns agent orchestration and the project connectors.
- [[Kochwiki]] owns and authorizes access to recipe-domain data.
- [[Home Assistant]] owns and authorizes access to home state.

## Dependencies

- [[Universal Agent Foundation]]
- Authentication and authorization appropriate for service-to-service access
- An explicit initial allowlist of readable data in each project

## Open questions

- Should initial cooking recommendations use only stored recipes, or require a future model of ingredients currently available at home?
- Which Kochwiki operations and Home Assistant entities form the smallest useful read-only surface?

## Next step

After the agent foundation exists, define one minimal read-only use case and the exact data each participating service must expose for it.
