# Software Development Workflow

Reusable software-development workflow.

## Composition

This workflow composes agent profiles from reusable roles under `../_common/roles/`.

Initial role set:

- Strategist — persistent workflow-level strategy and Software Development memory.
- Designer — ephemeral design/architecture session role.
- Coder — ephemeral implementation session role.
- Reviewer — ephemeral independent review role.
- Command Runner — ephemeral bounded command/tool execution role.

## Lifecycle model

The Strategist provides continuity across Software Development sessions. Designer, Coder, Reviewer and Command Runner are instantiated for bounded work and do not own permanent conversational memory.

Workflow/project facts, decisions and learned principles belong to durable Software Development memory. Each session receives a compiled task-relevant projection.

## Flows

A flow is a bounded process inside this long-lived workflow. Strategy may change the sequence/roles/commands used by a flow.

- [`implementation`](flows/implementation.md)
- [`testing`](flows/testing.md)
- [`debugging`](flows/debugging.md)
- [`review`](flows/review.md)
- [`release`](flows/release.md)

## Prompt routing / use cases

| Example prompt / intent | Route type | Route target | Result / notes |
| --- | --- | --- | --- |
| “Let's code this.” / “Implement this change.” | flow | [`implementation`](flows/implementation.md) | Enter implementation flow; active strategy determines concrete sequence. |
| “Test this change.” / “Verify this works.” | flow | [`testing`](flows/testing.md) | Run the bounded verification/testing process. |
| “Why is this failing?” / “Debug this.” | flow | [`debugging`](flows/debugging.md) | Investigate evidence, isolate cause, correct and verify. |
| “Review this PR.” | flow | [`review`](flows/review.md) | Independent review and findings/correction routing. |
| “Let's release it.” | flow | [`release`](flows/release.md) | Enter authorized release/readiness flow. |
| “Push these changes.” | command | `source-control` (planned) | Direct bounded source-control action when the command is connected and context/authorization is sufficient. |
| “Check the new ticket.” | role | `Manager` (planned) | Manager owns the responsibility and may use a future ticket-tracker command. |
| “What should we build first?” | strategist | Workflow Strategist | Requires prioritization/strategy rather than a bounded command. |

## Connected commands

Commands are defined in the [AI Commands repository](https://github.com/starodubtsevconsulting/ai-commands).

| Command | Reference | Used by / purpose |
| --- | --- | --- |
| None yet | — | `source-control` and ticket-tracker-style commands are planned examples but are not declared connected until they exist in `ai-commands`. |

## Runtime independence

This workflow defines roles and coordination, not concrete agents, models, providers or harnesses. A profile/runtime resolves the workflow's role requirements into actual agent instances.

## Next refactor

Migrate the existing agent capability, ownership, communication and routing contracts into this workflow's composition while preserving fail-closed boundaries.