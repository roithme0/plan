---
type: project
status: active
last_reviewed: 2026-10-02
---

# Kochwiki

## Purpose

Provide a private, mobile-first application for managing recipes and their ingredients.

## Current role

On Kochwiki v2's `staging` branch, the Angular frontend and FastAPI backend support recipe and foodstuff management, ordered ingredients and steps, and calculated nutrition. Recipes now have active, draft, and historical versions under a lineage. Manual edits can publish a new active version or create and update a draft; drafts can be published or discarded. Retained versions keep live foodstuff references, and any such reference blocks foodstuff deletion. The API lists historical versions, but the recipe UI does not yet offer a history browser or a draft-versus-active diff.

The MCP-based recipe workflow foundation is implemented: semantic discovery, in-memory proposals with temporary foodstuffs, explicit foodstuff creation/updates and atomic proposal saving through chat or the artifact button. Kochwiki owns domain guidance and frontend presentation contracts. See [[Recipe Agent Capability and Proposal Outline]] for delivered responsibilities and deferred work; real-model verification remains manual. Active recipes and drafts open a recipe-scoped chat with the selected recipe and its used foodstuffs in the snapshot. The AI Service remains optional for ordinary recipe management.

The browser still uses temporary user selection rather than authentication. The backend does not enforce recipe ownership or owner-only authorization, and foodstuff creator/change provenance is not yet represented. The current private-network deployment boundary should not be mistaken for those controls.

## Near-term focus

- Manually verify the implemented recipe-chat and proposal-to-draft foundation; deferred improvements are tracked in the architecture note
- Add history browsing and a draft-versus-active comparison to the existing version lifecycle
- Implement [[Browser Authentication Foundation]] and then enforce recipe ownership and shared-foodstuff provenance
- Keep human authentication and machine identities as separate security concerns
- Continue general image and other planned work on their own initiative timelines

## Authentication direction

Kochwiki will authenticate browser users through an external OIDC provider under [[DEC-002 External OIDC Provider for Human Authentication]]. Microsoft Entra ID Free is the current preference, while provider selection and the choice between a PKCE-based SPA and a Backend-for-Frontend session remain open.

## Data ownership direction

See [[DEC-001 Shared Foodstuffs and Owned Recipes]].

- Foodstuffs are shared among authenticated users and do not have an exclusive owner.
- Foodstuff creation and the latest change remain attributable to authenticated users.
- Recipes are readable by all authenticated users but have an explicit owner who controls their mutation, deletion, drafts, and publication.
- Recipe history retains live references to shared foodstuffs, so foodstuff changes may alter historical presentation and nutrition calculations.
- AI interactions operate only on behalf of an authenticated user and do not receive broader access.

## Direction

Remain independently useful for core recipe management while completing the recipe history and draft review experience, general image support, photo-assisted foodstuff nutrition entry, and optional AI-assisted features. Manual and AI-generated changes should converge on a shared, user-controlled draft and publication lifecycle. Participate in the wider ecosystem through purposefully designed and authorized APIs.

## Ecosystem relationships

See [[System Overview]].

## Related initiatives

- [[Browser Authentication Foundation]] — planned
- [[Recipe Versioning and Drafts]] — active
- [[AI-assisted Recipe Optimization]] — active
- [[ChatGPT Plan Integration]] — planned; recipe conversations reuse the AI Service provider connection
- [[Recipe and Ingredient Images]] — planned
- [[Read-only Project Access for the Agent]] — planned
- [[Controlled Agent Actions]] — exploring
- [[Photo-assisted Foodstuff Nutrition Entry]] — exploring

## Related ideas

- [[Foodstuff History and Duplicate Management]]
- [[Semantic Recipe and Foodstuff Search in the UI]] — exploring reuse of planned semantic retrieval for ordinary UI search

## Open questions

- Should future cooking recommendations use only saved recipes, or also an inventory of ingredients actually available at home?
- Should Kochwiki use a PKCE-based SPA or a Backend-for-Frontend session?
- Should recipe collaboration, ownership transfer, or copying another user's recipe be supported later?
