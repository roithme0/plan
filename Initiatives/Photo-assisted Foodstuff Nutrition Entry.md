---
type: initiative
status: exploring
last_reviewed: 2026-09-25
projects:
  - "[[Kochwiki]]"
  - "[[AI Service]]"
---

# Photo-assisted Foodstuff Nutrition Entry

## Intended outcome

When creating a foodstuff, a user enters its name and brand manually, photographs or uploads its nutrition label, and receives proposed values for kilocalories, carbohydrates, protein, and fat in the existing Kochwiki form. The user checks and may correct the values and their reference quantity before saving the foodstuff.

## Motivation

Entering four nutrition values by hand is a major source of effort in foodstuff creation. A label photo can shorten the process without changing who controls the shared foodstuff catalogue or how recipe nutrition is calculated.

## Scope and experience

- Add optional camera capture or image upload to the existing foodstuff creation flow; manual entry remains available.
- Extract only kcal, carbohydrates, protein, and fat, along with the label's reference quantity and unit. The name and brand remain user-entered.
- Prefer the label's per-100-g or per-100-ml column when several columns are present (for example, per portion and per 100 g). Read kcal rather than mistaking kJ for kcal.
- Apply values to the corresponding form fields only when the reference unit matches the foodstuff unit: 100 g for `g`, 100 ml for `ml`. These existing Kochwiki fields represent nutrition per 100 g or 100 ml respectively.
- Show the detected reference quantity, proposed values, and any missing or uncertain fields for review. The user may edit every field before submitting through the normal Kochwiki create flow.
- A mismatch, ambiguous column, or unreadable value must not silently populate the wrong basis or invent a number. Explain the issue and let the user retry or enter values manually. Never equate 100 g with 100 ml without an explicit conversion basis.
- The photo is input for extraction; permanent storage or association with the foodstuff is not required for this first outcome. If image retention is later desired, coordinate with [[Recipe and Ingredient Images]].
- The AI-assisted extraction produces a suggestion only. Kochwiki owns validation, creation, access control, and provenance under [[DEC-001 Shared Foodstuffs and Owned Recipes]].

## Project roles

- [[Kochwiki]] owns the creation form, unit choice, review and correction step, validation, and final persistence of the shared foodstuff.
- [[AI Service]] offers a bounded image-to-structured-nutrition capability, returning the four values and their observed reference basis without creating or editing foodstuffs.

## Initial boundary

The first supported path is an unambiguous label with values per 100 g for a foodstuff stored in `g`, or per 100 ml for one stored in `ml`. A label may also show other columns, provided the correct per-100 column can be identified reliably. Missing values remain empty for manual completion. There is no automatic conversion between mass and volume.

Foodstuffs stored as `Stk.` use nutrition per piece in today's Kochwiki model. A per-100-g or per-100-ml label cannot be mapped to that basis without an explicit piece weight or volume. The desired first-step behavior for such products remains open; do not silently apply per-100 values as per-piece values.

## Open questions

- Should the first release support `Stk.` when the label itself states nutrition per piece, or require manual entry for every piece-based foodstuff?
- Should a later release ask for a reliable weight or volume per piece to convert from a per-100 label, and where would that conversion input live?
- Should the extracted photo be discarded after processing or saved as evidence once general foodstuff image support exists?
- Should the same assisted entry later be offered when editing an existing foodstuff?

## Next step

Define the bounded extraction response and a mobile form review interaction in the service repositories. Decide piece-based behavior before implementing that path.
