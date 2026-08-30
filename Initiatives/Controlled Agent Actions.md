---
type: initiative
status: exploring
last_reviewed: 2026-08-30
projects:
  - "[[AI Service]]"
  - "[[Kochwiki]]"
  - "[[Home Assistant]]"
---

# Controlled Agent Actions

## Intended outcome

The universal agent can perform selected, user-requested actions through project APIs, such as turning on an allowed light, without receiving unrestricted access.

## Motivation

Move from an informational assistant to a useful ecosystem interface while retaining clear ownership, authorization, and accountability.

## Scope

- Add actions only after read-only integrations are established
- Allowlist actions and resources per service
- Use dedicated machine identities and scoped authorization
- Let each domain service validate and execute its own actions
- Audit requested and executed actions
- Require confirmation according to the consequence of an action

## Out of scope

- Direct database, filesystem, or unrestricted network access
- General administrative access
- Assuming every action has the same risk or confirmation policy

## Project roles

- [[AI Service]] interprets intent, requests confirmation when required, and invokes authorized tools.
- [[Kochwiki]] validates and executes allowed recipe-domain changes.
- [[Home Assistant]] validates and executes allowed home actions.

## Dependencies

- [[Read-only Project Access for the Agent]]
- Authentication, authorization, auditing, and confirmation policies
- Explicit action allowlists for each participating project

## Open questions

- Which single action should be introduced first?
- Which actions may run immediately, which require confirmation, and which should remain unavailable?

## Next step

Defer implementation until the read-only integrations are working; then define one low-risk action and its complete authorization and audit path.
