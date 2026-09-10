---
type: initiative
status: exploring
last_reviewed: 2026-09-10
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
- Run agent interactions only on behalf of an authenticated user
- Authorize every tool call against the represented user's permissions and the tool's own narrower scope
- Allowlist actions and resources per service
- Use dedicated machine identities without treating them as independent user authority
- Let each domain service validate and execute its own actions
- Audit the authenticated user, requested action, agent or machine identity, and execution result
- Require confirmation according to the consequence of an action

## Out of scope

- Anonymous access to project data or actions
- Giving the AI Service permissions that no authenticated user supplied
- Direct database, filesystem, or unrestricted network access
- General administrative access
- Assuming every action has the same risk or confirmation policy

## Project roles

- [[AI Service]] interprets intent, preserves the authenticated user context, requests confirmation when required, and invokes authorized tools.
- [[Kochwiki]] validates both user and tool permissions and executes allowed recipe-domain changes.
- [[Home Assistant]] validates both user and tool permissions and executes allowed home actions.

## Dependencies

- [[Browser Authentication Foundation]]
- [[Read-only Project Access for the Agent]]
- Delegated user context across service boundaries
- Authentication, authorization, auditing, and confirmation policies
- Explicit action allowlists for each participating project

## Open questions

- Which single action should be introduced first?
- Which actions may run immediately, which require confirmation, and which should remain unavailable?
- How should authenticated user context be delegated to and verified across service boundaries?
- How should machine identity and represented user identity appear together in the audit trail?

## Next step

Defer implementation until the read-only integrations and reliable human authentication are working; then define one low-risk action and its complete authorization and audit path.
