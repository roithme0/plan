---
type: idea
status: exploring
last_reviewed: 2026-10-02
projects:
  - "[[Kochwiki]]"
  - "[[AI Service]]"
---

# Recipe Optimization Skills and Kochwiki Tools

## Agreed direction

Improve the technical foundation for future recipe optimization by enabling a richer agent workflow. Better recipe quality remains a longer-term outcome, not the immediate acceptance criterion for this iteration.

Prioritize Kochwiki capabilities exposed through MCP. Skills are interesting as a following step. The motivation is broader agent capability, not overcoming snapshot limits: the current foodstuff catalogue is below the count limit and snapshot limits are acceptable today.

The direction clarified on 2026-10-01 retains the selected recipe and its used foodstuffs in the initialization snapshot. Additional foodstuffs and optionally other recipes are discovered through semantic search and retrieved through tools. The agent can show retrieved items as artifacts. Proposals may include temporary, not-yet-persisted foodstuff candidates; saving a proposal creates its required missing foodstuffs together with the draft. Standalone foodstuff creation is available only on explicit request. The delivered ownership and contracts are summarized in the architecture note; detailed implementation belongs in the service repositories.

Interaction defaults are now agreed: resolve ambiguity through natural language, let the agent use a clear match without a confirmation step, and show artifacts selectively rather than for every retrieval. Advanced selection buttons and other artifact actions are optional later shortcuts. An explicit proposal save includes creation of its missing foodstuffs without separate confirmation.

Treat this iteration as an MVP foundation. Prioritize working conversations, semantic retrieval, proposal representation, and reliable persistence. Detailed authentication, delegated authorization, audit policy, and production security hardening are successive follow-up work, not prerequisites for developing this foundation.

## Delivery status

The agreed foundation is now implemented. The current architecture and deferred
priorities are documented in [[Recipe Agent Capability and Proposal Outline]] and
[[AI-assisted Recipe Optimization]]. Subsequent decisions simplified the original
exploration: enriched searches avoid separate get tools, temporary definitions are
inline, proposals/save mappings are in memory, and presentation uses frontend
capabilities with a generic AI Service local tool. Skills remain later work.

The user will verify the real-model workflow manually. Increased runtime budgets
provide more room for normal tool sequences. Generic tool-result retention is the
highest-priority deferred follow-up; temporary-ingredient visualization and catalogue
reconciliation are also deferred. Better recipe quality remains a later outcome.
