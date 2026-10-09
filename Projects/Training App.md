---
type: project
status: planned
last_reviewed: 2026-10-09
---

# Training App

## Purpose

Provide a third application alongside [[Kochwiki]] and [[AI Service]] for planning training sessions and tracking sporting progress. "Training App" is a working name.

## Current role

New planned application. This note records product direction; no implementation or repository is asserted.

## Direction

Let users define sporting goals, plan training sessions, record completed activity, and review progress over time. Coordinate training and nutrition with Kochwiki so both applications support the same user's goals, such as muscle building.

Keep the initial direction broad. Supported sports, progress metrics, and the first delivery scope remain open.

## Ecosystem relationships

- [[Kochwiki]] supplies recipe and nutrition context and is the intended home for nutrition planning.
- [[AI Service]] can assist with planning and interpreting information across both domains, while each application owns its data and rules.
- Relevant goals, planned/completed sessions, progress, and nutrition context should be exchangeable for the same user through explicit authorized interfaces. Direct app-to-app exchange versus AI Service orchestration is undecided.
- Strava is a candidate source of completed activities, potentially through a Strava MCP server. No particular server, capabilities, or integration is selected or verified.

See [[System Overview]] and [[Training and Nutrition Coordination]].

## Related initiatives

- [[Training and Nutrition Coordination]] — planned direction

## Open questions

- What should the application be called?
- Which sports and progress indicators should the first version support?
- Which application owns the shared sporting goal, and how is the same user identified across applications?
- Which external activity source and integration approach should be evaluated first?
