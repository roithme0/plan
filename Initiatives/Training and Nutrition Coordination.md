---
type: initiative
status: planned
last_reviewed: 2026-10-09
projects:
  - "[[Training App]]"
  - "[[Kochwiki]]"
  - "[[AI Service]]"
---

# Training and Nutrition Coordination

## Intended outcome

A user can plan nutrition and training toward a shared sporting goal and follow progress across Kochwiki and the new Training App, with optional assistance from AI Service.

This is an accepted product direction, with implementation scope and timing still open.

## Motivation

Training and nutrition influence the same sporting outcome. Connecting their context should help users make coherent plans and revisit them as completed activity and progress become available.

## Example journey

A user chooses muscle building as a goal, plans suitable training sessions in Training App, and uses Kochwiki to plan meals and adapt recipes to that goal. Completed sessions and progress can inform later suggestions in both applications. External activity records, for example from Strava, may supplement the user's own entries.

This example illustrates the direction without prescribing nutrition targets, a training methodology, or automatic plan changes.

## Scope

- Introduce Training App as a distinct application for training plans, sessions, and progress.
- Support nutrition planning in Kochwiki in relation to the user's sporting goal and training context.
- Exchange relevant goal, training, progress, and nutrition information for the same user.
- Explore AI Service assistance for coordinated planning, reviewing progress, and proposing adjustments.
- Consider external activity data, with Strava and a possible Strava MCP server as candidates.

## Project roles

- [[Training App]] owns training plans, sessions, and sporting progress.
- [[Kochwiki]] owns recipe/foodstuff data and the intended nutrition-planning experience. Meal planning and intake tracking are not asserted as currently implemented.
- [[AI Service]] provides optional AI assistance and orchestration through authorized domain interfaces; it does not become the source of truth for training or nutrition data.

The owner of cross-domain goals and the exchange mechanism remain open. Shared identity should build on [[Browser Authentication Foundation]]; each domain retains responsibility for authorization and validation.

## Relationship to existing work

[[Profile-based Recipe Constraints and Preferences]] may provide reusable dietary context, but sporting goals and training context are a separate cross-project concern. This direction does not expand the current MVP scope of [[AI-assisted Recipe Optimization]].

## Open questions

- Which first user journey and sports should be supported?
- Which progress indicators and nutrition-planning capabilities are needed initially?
- Where should shared goals live, and which information should each application exchange?
- Should exchange be direct, orchestrated through AI Service, or use a combination?
- Is a suitable Strava MCP server available, or would another integration better fit the required activity data?

## Next step

Keep this as high-level planned direction. When implementation is taken up, select one small end-to-end user journey and evaluate the required data and external connector before defining detailed contracts or UI flows.
