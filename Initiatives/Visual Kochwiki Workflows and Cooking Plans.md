---
type: initiative
status: exploring
last_reviewed: 2026-10-03
projects:
  - "[[Kochwiki]]"
  - "[[AI Service]]"
---

# Visual Kochwiki Workflows and Cooking Plans

## Intended outcome

Offer agent-generated, understandable diagrams inside Kochwiki for two selected product use cases: explaining user-facing Kochwiki workflows and planning the preparation of meals. Users can discuss a diagram with the assistant and request revisions.

These two use cases are accepted for further planning. Implementation, prioritization, and the concrete visualization integration remain exploratory; this note does not describe delivered functionality.

## Motivation

Text alone can make dependencies and parallel activities difficult to follow. A visual explanation can help a user understand the next step in Kochwiki, while a visual cooking plan can coordinate preparation so dishes are ready together.

## Scope and experience

### Make Kochwiki workflows understandable

- Explain user-facing processes with a compact diagram and accompanying plain-language text.
- Example: request a recipe improvement → inspect the proposal → save it as a draft → compare with the active recipe → publish or discard.
- Adapt the explanation to the user's question and the application's actual capabilities. Clearly distinguish available steps from planned functionality, such as the currently missing draft-versus-active diff.
- Describe the assistant's observable actions and the user's review/save choices when relevant. This complements the live activity entries planned in [[Shared Agent Chat UI and Domain Rendering]]; it does not replace them or expose private model reasoning.
- Let users ask for a simpler explanation, more detail, or an updated diagram.

### Plan cooking workflows

- Turn one recipe or a selection of recipes into a visual preparation plan showing ordered steps, dependencies, and tasks that can run in parallel.
- Show preparation, cooking, resting, and waiting periods where information is available.
- Help coordinate several dishes toward a shared serving time, for example starting oven vegetables while a sauce simmers and preparing the salad during the waiting period.
- Revise the plan conversationally, for example "serve at 19:00" or "I only have one oven".
- Base the plan on the selected recipes and user-provided constraints. Identify missing durations or equipment information and make estimates explicit rather than presenting them as recipe facts.
- A cooking plan is an assistant suggestion; producing or adjusting a diagram does not automatically edit recipes.

## Visualization direction

Explore an Excalidraw MCP integration as a way for the agent to create and revise editable diagrams. The selected server's capabilities and suitability for our own frontend must be evaluated rather than assuming all Excalidraw MCP implementations provide the same behavior.

The product needs a usable diagram surface in Kochwiki, including on mobile, alongside its text explanation. MCP tool access alone does not establish that display or editing experience. Use [[Shared Agent Chat UI and Domain Rendering]] as the related artifact/presentation direction without prescribing a concrete UI or tool contract here.

## Project roles

- [[Kochwiki]] owns recipe context, product workflow accuracy, and the user-facing diagram or cooking-plan experience.
- [[AI Service]] orchestrates the conversation and bounded visualization tools and supports revising the result from follow-up requests.
- An Excalidraw integration is a visualization capability, not the authority for recipe data or Kochwiki operations.

## Scope boundary

This initiative covers the two Kochwiki product use cases above. Architecture diagrams, developer workflows, UI mockups, Plan-repo diagrams, Home Assistant documentation, and general-purpose knowledge illustrations are outside this scope.

It adds no recipe publication behavior, automatic appliance control, or requirement for manual canvas editing in the first release.

## Open questions

- Which Excalidraw MCP implementation supports the required creation, revision, and frontend presentation?
- Should the first release allow only conversational revisions, or also direct canvas editing? If direct editing is offered, how does the agent read the latest state?
- Should diagrams and cooking plans be transient conversation artifacts or saved for later reuse, and how should recipe versions be referenced?
- Which timing and equipment information is already available in recipes, and which inputs should users supply?
- Which mobile presentation works well for both process explanations and multi-dish cooking plans?

## Next step

Evaluate one user-facing Kochwiki workflow and one multi-dish cooking scenario. Check diagram readability, recipe fidelity, conversational revision, and mobile presentation before selecting the integration and defining implementation work in the service repositories.
