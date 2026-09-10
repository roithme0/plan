---
type: decision
status: accepted
date: 2026-09-10
affects:
  - "[[Kochwiki]]"
  - "[[AI Service]]"
---

# DEC-001: Shared Foodstuffs and Owned Recipes

## Context

Kochwiki is intended for multiple authenticated users. Recipe creation should reuse a shared set of foodstuffs so that the same products and nutrition data do not need to be entered and maintained separately for each user.

Recipes have a clearer personal responsibility: users may read one another's recipes, but changes should remain under the control of the recipe owner. AI interactions must not bypass the permissions of the authenticated user on whose behalf they run.

## Decision

### Access boundary

- Kochwiki data is available only to authenticated users.
- AI access occurs only within an authenticated interaction and on behalf of that user.
- The AI Service receives no independent or broader entitlement to Kochwiki data.
- Kochwiki enforces authorization for every read and mutation requested through either its own UI or a bounded AI tool call.

### Foodstuffs

- Foodstuffs are shared canonical data and have no exclusive user owner.
- Authenticated users may read, create, and edit shared foodstuffs.
- Kochwiki records who created a foodstuff and who most recently changed it as provenance, not as an ownership restriction.
- A foodstuff may be deleted only when no active recipe, draft, or historical recipe version references it.
- Duplicate management beyond existing basic uniqueness constraints is deferred.
- Foodstuff version history is a possible later extension, not part of the initial ownership model.

### Recipes

- Every recipe has an explicit owner.
- All authenticated users may read active recipes.
- Only the owner may edit or delete a recipe and manage its drafts.
- Recipe versions and drafts remain governed by the owner of their recipe.
- The AI may create a draft only on behalf of an authenticated owner who is authorized to edit that recipe.
- The AI may not publish an active version, accept or discard a draft, transfer ownership, or delete recipe data.

### Historical recipe semantics

- Recipe versions and drafts retain references to shared foodstuffs rather than snapshotting foodstuff fields or nutrition values.
- Historical recipe structure and quantities are preserved, but their displayed foodstuff data and calculated nutrition may change when a referenced foodstuff changes.
- This changing historical representation is accepted for the current product stage.
- Referential integrity takes priority over preserving historical foodstuff values.

## Consequences

- Shared maintenance reduces repeated foodstuff records and distributes data upkeep across users.
- A foodstuff correction can affect nutrition calculations and presentation across active and historical recipes owned by other users.
- Provenance makes changes attributable without granting exclusive control to the contributor.
- Deletion checks must expand when drafts and recipe history are introduced so every retained reference is considered.
- Authorization requires reliable authenticated user identities before these rules can be enforced.
- Collaboration, ownership transfer, and copying or forking another user's recipe may be added later but are not implied by this decision.

## Alternatives considered

- User-owned foodstuffs, rejected because they encourage duplicate product and nutrition records.
- Fully immutable or versioned foodstuffs, deferred because the initial complexity is not currently justified.
- Snapshotting foodstuff data into every recipe version, rejected for now in favor of live shared references.
- Allowing every user to edit every recipe, rejected because recipe changes require explicit ownership.
