---
type: idea
status: exploring
last_reviewed: 2026-09-14
projects:
  - "[[Kochwiki]]"
  - "[[AI Service]]"
---

# Profile-based Recipe Constraints and Preferences

## Direction

Allow users to store reusable recipe-related constraints and preferences in their Kochwiki user profile and apply them as context during AI-assisted recipe optimization.

Each profile entry is classified as exactly one of:

- **Constraint:** a requirement that proposals must satisfy
- **Preference:** a desired direction that should be followed when reasonable but may be traded off against the recipe, the user's current request, or other preferences

The distinction describes how strongly an entry governs a proposal, not what the entry is about. The same subject may therefore be expressed either way. For example, vegetarian eating may be a constraint for one user and a preference for another. This example should not define or limit the general model.

## Motivation

Users should not have to restate recurring requirements and preferences for every optimization session. At the same time, the AI must distinguish requirements from negotiable wishes so that it neither violates explicit constraints nor treats every preference as absolute.

## Intended behavior

- Kochwiki owns the profile data and provides UI for viewing, creating, changing, and removing entries.
- Kochwiki supplies the relevant profile entries as a snapshot when it initializes an AI-assisted recipe optimization session.
- The AI Service treats the snapshot as caller-provided context and passes it to the model or API together with the recipe and optimization context.
- The AI Service does not become the source of truth for the profile and does not independently persist profile changes.
- Constraints apply to every proposal unless the user explicitly overrides them for the current session.
- A request that clearly conflicts with a constraint may override it only when the user's intent to do so is unambiguous; otherwise the assistant asks for clarification.
- Preferences guide proposal creation but do not make an otherwise useful proposal invalid.
- The assistant may suggest adding or changing a profile entry, but profile mutations require explicit user confirmation.
- The system does not infer durable profile entries solely from repeated conversation behavior.

## Validation boundary

Prompt context alone is not sufficient for constraints that can be validated deterministically. Kochwiki should validate a proposal against enforceable constraints before it can be saved as a draft.

Not every entry will be completely machine-verifiable. The eventual model should make the distinction visible instead of implying that all natural-language constraints are guaranteed automatically.

Safety-critical information such as allergies or medical restrictions may require stricter handling than ordinary lifestyle constraints. Their exact representation and override policy remain separate design questions.

## Relationship to recipe optimization

This idea extends [[AI-assisted Recipe Optimization]] without changing its initial scope. The first IBS-focused implementation may proceed without a general user-profile model. Once adopted, the profile snapshot becomes an additional source of relevant constraints alongside the explicit request and the recipe-scoped session context.

Explicit instructions in the current session take precedence over profile preferences. A conflict with a profile constraint must be an explicit override or lead to clarification rather than being silently resolved.

## Out of scope

- Defining a fixed catalogue of supported dietary properties
- Treating vegetarianism or any other example as a special-case data model
- Adding more than the two categories constraint and preference
- Automatically learning durable profile information from conversations
- Letting the AI Service own or independently edit the user profile
- Claiming deterministic enforcement for entries that cannot be validated
- Finalizing allergy, medical-safety, or consent semantics

## Open questions

- Are entries selected from a controlled catalogue, entered as natural language, or represented through a hybrid model?
- Which constraints can Kochwiki validate deterministically against recipe and foodstuff data?
- How are conflicts between multiple constraints or preferences explained to the user?
- Should an explicit session override apply to one proposal, the complete session, or require the user to choose its duration?
- How should profile snapshot version or provenance be retained with a proposal?
- Which safety-critical entries require stronger confirmation or must not be session-overridable?
- Which profile fields are relevant enough to send to a recipe-scoped session while following data-minimization principles?

## Next step

Define the smallest profile-entry model and precedence rules, then identify which initial entries Kochwiki can validate reliably before integrating the snapshot into AI-assisted recipe optimization.
