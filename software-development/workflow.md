# Software Development Workflow

Reusable software-development workflow.

## Composition

This workflow realizes reusable roles through [`agents.md`](agents.md) and coordinates them through [`team/`](team/README.md).

Initial team includes Workflow Strategist, Designer Reviewer, Coder, independent Reviewer, Manager and Command Runner.

## Lifecycle model

The Strategist provides durable workflow continuity. Execution agents are bounded/session-oriented unless explicitly configured otherwise. Designer Reviewer owns design intent and later implementation-conformance review but does not modify implementation source code.

## Flows

- [`implementation`](flows/implementation.md)
- [`testing`](flows/testing.md)
- [`debugging`](flows/debugging.md)
- [`review`](flows/review.md)
- [`release`](flows/release.md)

## Prompt routing / use cases

| Example prompt / intent | Route type | Route target | Result / notes |
| --- | --- | --- | --- |
| “Let's design this.” / “How should we build this?” | agent | Designer Reviewer | Human-facing design discussion; Designer Reviewer may read source directly. |
| “Let's code this.” / “Implement this change.” | flow | [`implementation`](flows/implementation.md) | Designer Reviewer may delegate implementation to Coder; active strategy determines sequence. |
| “Run the tests.” / “Build this.” | agent | Command Runner | Execution is delegated to Command Runner even when requested during Designer Reviewer work. |
| “Show me the diff.” / “What changed in Git?” | agent | Command Runner | Command Runner routes through `source-control`; Designer Reviewer does not access Git operations directly. |
| “Commit these changes.” / “Push these changes.” / “Create a branch.” | agent | Command Runner | Command Runner invokes the bounded `source-control` command subject to authorization. |
| “Check the new ticket.” / “Create a ticket for this.” | agent | Manager | Ticket operations belong to Manager, which uses `ticket-tracker`; Designer Reviewer asks Manager rather than accessing tracker directly. |
| “Review whether this follows the design.” | agent | Designer Reviewer | Designer Reviewer performs design-conformance review. |
| “Get an independent review.” | agent | Command Runner | Routes/initiates independent Reviewer work without confusing it with Designer Reviewer's conformance review. |
| “Why is this failing?” / “Debug this.” | flow | [`debugging`](flows/debugging.md) | Enter debugging/investigation flow. |
| “Let's release it.” | flow | [`release`](flows/release.md) | Enter authorized release/readiness flow. |
| “What should we build first?” | strategist | Workflow Strategist | Requires workflow-level prioritization/strategy. |

These are semantic routing examples, not exact required phrases.

## Connected commands

Commands are defined in the [AI Commands repository](https://github.com/starodubtsevconsulting/ai-commands).

| Command | Reference | Used by / purpose |
| --- | --- | --- |
| `source-control` | [AI Commands / source-control](https://github.com/starodubtsevconsulting/ai-commands/tree/main/source-control) | Used through Command Runner for Git history/diffs, branches, commits, pushes and other bounded source-control operations. |
| `ticket-tracker` | [AI Commands / ticket-tracker](https://github.com/starodubtsevconsulting/ai-commands/tree/main/ticket-tracker) | Used by Manager for ticket retrieval, creation and updates. |

## Memory

Workflow/project facts, decisions and learned principles belong to durable Software Development memory owned through the strategic layer. Execution agents receive task-relevant projections rather than owning permanent conversational memory by default.

## Runtime independence

This workflow defines responsibilities, routing and boundaries rather than concrete models/providers/harnesses. Runtime/profile configuration resolves `agents.md` requirements into actual agent instances and command integrations.

## Privacy boundary

Credentials, private repository configuration, ticket-system credentials and organization-specific private data remain runtime/profile concerns.