---
type: idea
status: exploring
last_reviewed: 2026-10-18
projects:
  - "[[Kochwiki]]"
  - "[[AI Service]]"
---

# Automatic Foodstuff Categorization

## Direction

Use Jev from TypeSafe, a decision AI, to map ingredients / foodstuffs in Kochwiki to categories such as vegetables (Gemüse), fruit (Obst), and snacks (Snacks). This is another exploratory use case for AI-based mapping alongside [[Automatic Recipe Step Images]].

## Motivation

Organize foodstuffs consistently and reduce manual categorization work, making ingredients easier to browse and filter by category.

## Intended behavior

- Let Jev select fitting categories from a defined category catalogue based on the foodstuff's available information.
- Consider categorization when foodstuffs are created or relevant information changes.
- Keep categories attached to shared foodstuffs so the classification can be reused across recipes.
- Kochwiki owns the categories, assignments, and presentation; the decision AI provides the mapping decision.

## Open questions

- Which initial categories and category definitions are useful?
- Should a foodstuff have one category or several, for example when product type and usage overlap?
- How should uncertain assignments, manual corrections, and existing foodstuffs be handled?

## Relationship to step-image mapping

[[Automatic Recipe Step Images]] describes the same general selection pattern for mapping preparation steps to a predefined image catalogue. Here the selection target is a foodstuff category. These remain separate product ideas; a shared implementation is not decided.

## Next step

Retain this as an idea. When taken up, define a small initial category catalogue and evaluate Jev's assignments on representative foodstuffs before deciding implementation details.
