---
type: workspace
---

# Workspace Structure

This workspace uses a shallow hybrid structure: folders distinguish broad kinds of notes, while links describe relationships between them. The structure should remain small and may evolve when repeated use reveals a genuine need.

## Structure

```text
Home.md                  Workspace entry point
Projects/                Durable applications and services
Initiatives/             Time-bounded outcomes spanning one or more projects
Architecture/            Ecosystem-level relationships and principles
Decisions/               Significant cross-project decisions
Ideas/                   Possibilities not yet accepted as planned work
Templates/               Starting structures for recurring note types
Archive/                 Material retained but no longer active
```

`README.md` explains the repository to someone encountering it outside Obsidian. `Home.md` is the navigation entry point from within the workspace.

## Placement rules

### Projects

A project is a durable application or service with its own identity and lifecycle. Project notes capture purpose, current role, direction, and ecosystem relationships without duplicating implementation documentation.

### Initiatives

An initiative is a desired, time-bounded outcome. It may involve several projects and should describe the intended result, scope, and the role of each participating project.

### Architecture

Architecture notes describe the ecosystem as a whole: system relationships, shared principles, and intended data flows. Service-internal architecture belongs in the relevant service repository.

### Decisions

Decision notes record significant choices that shape more than one project or the overall direction. Implementation decisions local to one service belong in that service repository.

### Ideas

Ideas are uncommitted possibilities. When an idea becomes an accepted outcome with active planning, it should become an initiative.

### Archive

Archived notes remain available for historical context but are no longer part of active planning.

## Sources of truth

- This workspace is authoritative for ecosystem intent, priorities, and planned relationships.
- Service repositories are authoritative for current behavior, technical constraints, and implementation.
- A mismatch between the two is made explicit rather than silently resolved; it may indicate an unimplemented plan or outdated planning context.

## Conventions

- Use human-readable note names and shallow folders.
- Link canonical concepts by name, such as `[[Kochwiki]]`.
- Separate current state from intended direction.
- Mark speculative material clearly rather than presenting it as committed work.
- Do not store secrets, source code, complete API contracts, or detailed deployment instructions here.
- Add a new folder or note type only after a recurring organizational need emerges.
