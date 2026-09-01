# Designer Role

Design and architecture role for bounded workflow work.

## Properties

- level: execution
- human-facing: true
- interaction-mode: reactive
- memory-class: SESSION
- lifecycle: ephemeral

These are defaults under [`role.spec.md`](../../role.spec.md) and may be explicitly specialized by a workflow/profile.

## Responsibilities

Explore requirements, constraints, boundaries, alternatives and trade-offs; produce design decisions/evidence appropriate to the workflow.

## Human interaction

Human-facing by default because design work often benefits from direct discussion of intent and trade-offs. Whether this role is exposed to a particular human depends on workflow/profile context; higher-level users may instead interact through a Strategist or management role.

## Lifecycle and memory

Created for a session/task, receives a compiled context projection, produces design decisions/evidence, and may then be discarded.

Persistent knowledge belongs to workflow memory/Strategist rather than to the Designer agent instance.