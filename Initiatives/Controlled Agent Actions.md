---
type: initiative
status: exploring
last_reviewed: 2026-10-09
projects:
  - "[[AI Service]]"
  - "[[Kochwiki]]"
  - "[[Home Assistant]]"
---

# Controlled Agent Actions

## Intended outcome

The universal agent can perform selected, user-requested actions through project APIs, such as turning on an allowed light, without receiving unrestricted access.

## Current state

Selected Kochwiki writes are already implemented through the MCP-backed Kochwiki agent: foodstuff creation and updates, in-memory recipe proposals, and explicit saving of a proposal as a draft, including atomic temporary-foodstuff creation. Saving through chat and the artifact button uses the same Kochwiki save service. See [[Read-only Project Access for the Agent]] for the reviewed implementation evidence.

This domain workflow precedes the broader universal-agent initiative. Its current private-network and selected-user boundary does not establish authenticated delegated authorization, owner-only enforcement, comprehensive action auditing, or an enforced general confirmation policy. Home Assistant actions remain unimplemented in this planning baseline. The initiative remains exploratory for that broader controlled-action outcome.

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

Build on the delivered Kochwiki writes rather than planning them as a first implementation. Establish authenticated delegated authority, scoped action permissions, auditing and confirmation behavior before extending access beyond the trusted deployment; evaluate Home Assistant actions after its read connector is established.
