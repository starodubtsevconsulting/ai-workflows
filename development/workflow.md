# Development Workflow

Reusable software-development workflow.

## Composition

This workflow composes agent profiles from reusable roles under `../_common/roles/`.

Initial role set:

- Strategist — persistent workflow-level strategy and Development memory.
- Designer — ephemeral design/architecture session role.
- Coder — ephemeral implementation session role.
- Reviewer — ephemeral independent review role.
- Command Runner — ephemeral bounded command/tool execution role.

## Lifecycle model

The Strategist provides continuity across Development sessions. Designer, Coder, Reviewer and Command Runner are instantiated for bounded work and do not own permanent conversational memory.

Workflow/project facts, decisions and learned principles belong to durable Development memory. Each session receives a compiled task-relevant projection.

## Runtime independence

This workflow defines roles and coordination, not concrete agents, models, providers or harnesses. A profile/runtime resolves the workflow's role requirements into actual agent instances.

## Next refactor

Migrate the existing agent capability, ownership, communication and routing contracts into this workflow's composition while preserving fail-closed boundaries.
