---
type: idea
status: exploring
last_reviewed: 2026-10-18
projects:
  - "[[Kochwiki]]"
  - "[[AI Service]]"
---

# Automatic Recipe Categorization and Tags

## Direction

Use Jev from TypeSafe, a decision AI, to categorize recipes by preparation effort or difficulty and assign descriptive tags. Example levels are easy (leicht), medium (mittel), and hard (schwer); example tags are spicy (scharf) and vegetarian (vegetarisch). This remains an exploratory idea.

## Motivation

Make recipes easier to browse, filter, and choose for a particular occasion without requiring users to maintain every classification manually.

## Intended behavior

- Assess the recipe's ingredients and preparation steps, together with available time information, to select a fitting effort or difficulty level.
- Select applicable tags from a defined catalogue, allowing multiple tags such as spicy and vegetarian on the same recipe.
- Consider reassessing classifications when a recipe is created or relevant ingredients or steps change.
- Kochwiki owns category and tag definitions, assignments, and presentation; Jev supplies the classification decision.

## Open questions

- Should effort and difficulty be separate dimensions, and how should easy, medium, and hard be defined?
- Which initial tags and criteria are useful, including explicit ingredient-based criteria for dietary tags such as vegetarian?
- How should uncertain classifications, manual corrections, and existing recipes be handled?

## Related ideas

- [[Automatic Foodstuff Categorization]] — classifies shared ingredients rather than whole recipes
- [[Automatic Recipe Step Images]] — maps preparation steps to reusable action images

These are related Jev use cases, while their implementation and any shared classification mechanism remain open.

## Next step

Retain this as an idea. When taken up, define a small initial set of levels and tags and evaluate Jev's assignments on representative recipes.
