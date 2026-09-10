---
type: idea
status: exploring
last_reviewed: 2026-09-10
projects:
  - "[[Kochwiki]]"
---

# Foodstuff History and Duplicate Management

## Direction

Explore stronger quality controls for the shared foodstuff catalogue after the initial authenticated, provenance-aware collaboration model is established.

Possible capabilities include:

- A version history for foodstuff changes
- Comparing and restoring earlier foodstuff values
- Detecting likely duplicates beyond exact name-and-brand uniqueness
- Merging duplicates while preserving recipe, draft, and historical-version references
- Additional review or moderation for high-impact changes

## Motivation

Shared foodstuffs avoid repeated maintenance, but an incorrect change can affect nutrition calculations and recipe presentation for multiple users. Similar products may also enter the catalogue under slightly different names or brands.

These risks are acceptable for the current product stage and do not justify delaying the simpler shared model described in [[DEC-001 Shared Foodstuffs and Owned Recipes]].

## Open questions

- Which foodstuff fields require history?
- Should restoring a version affect all referencing recipes immediately?
- How should likely duplicates be identified without merging distinct products?
- Who may merge records or restore earlier values?
- What audit information is sufficient before a complete version model exists?

## Next step

Revisit after shared foodstuffs have real multi-user usage and concrete data-quality problems can guide the design.
