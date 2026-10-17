---
type: idea
status: exploring
last_reviewed: 2026-10-14
projects:
  - "[[Kochwiki]]"
  - "[[AI Service]]"
---

# Automatic Recipe Step Images

## Direction

Give recipe preparation steps a small, automatically selected image from a predefined catalogue of generic cooking actions, such as cutting, frying, boiling, and baking.

Chef, the decision AI, selects the best-fitting catalogue image for each step when recipe steps are created or changed. If none fits well, the step has no image. This remains an exploratory idea rather than committed implementation work.

## Motivation

Make preparation steps easier to scan and recognize without adding image-editing work for users or generating a separate image for every step.

## Intended behavior

- Provide a reusable set of generic cooking-action images.
- Let Chef map each step's content to at most one existing catalogue image, or no image.
- Reassess the assignment when the step is added or its content changes, including manual and AI-assisted recipe changes.
- Show the selected image alongside the step while keeping the written instructions authoritative.
- Keep image selection automatic; users do not upload, edit, or manually select step images.
- Do not generate images individually for recipe steps.

## Relationship to existing image work

This complements [[Recipe and Ingredient Images]] with reusable illustrations for preparation steps. Kochwiki owns the catalogue, step associations, and presentation; Chef provides the selection decision. The image catalogue, integration details, and technical contracts remain open.

## Related ideas

- [[Automatic Foodstuff Categorization]] — another decision-AI mapping use case, assigning foodstuffs to categories

## Next step

Retain this as an idea. When taken up, agree on a small initial catalogue and evaluate Chef's image-or-no-image choices on representative recipe steps before defining the implementation.
