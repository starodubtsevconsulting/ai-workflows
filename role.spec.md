# Role Specification

This document defines the common contract for reusable roles.

A role is a reusable responsibility/behavior definition. A runtime agent is an implementation/instance of a role inside a particular workflow/profile.

## Required role properties

Every role definition MUST contain a Properties section with at least `level`, `human-facing`, `interaction-mode`, `memory-class`, and `lifecycle`.

## Prompt / intent scenarios

Every role MUST contain a prompt/intent scenario table, even when empty.

For a reusable role this table describes **what kind of Human/caller intent the role recognizes**, not how a particular workflow implements the multi-agent solution.

| Example prompt / intent | Role interpretation | Workflow routing required |
| --- | --- | --- |
|  |  |  |

A role MUST NOT hard-code knowledge of workflow-specific peers, commands, ticket systems, repositories or orchestration merely to explain how an intent is fulfilled.

For example, a reusable Designer Reviewer may recognize:

`"Review this pull request: <url>" -> review an implementation/change against its intended scope -> yes`

The reusable role does **not** say "call Manager, then ticket-tracker, then code-review" because those are workflow/team implementation details.

When `Workflow routing required = yes`, the runtime agent MUST consult the active workflow/team prompt-routing definition to determine the allowed flow, roles and commands for that workflow.

This preserves:

`Role = understands responsibility/intent`

`Workflow Team = knows who/how to collaborate`

`Command = knows how to perform bounded operation`

## Agent communication protocol

Every runtime agent implementing a role MUST follow [`_common/communication.md`](_common/communication.md).

## Command authority — not granted by default

Concrete commands are not granted at reusable role level. Workflow implementation explicitly grants them.

`Role -> Workflow agent realization -> command-matrix grant -> Runtime authorization -> Command`

Omission means not granted; explicit `forbidden` means intentional no-go.

## Human participant

Every workflow starts from or ultimately serves Human. Human is not an AI agent but MUST be represented in workflow team communication/capability modeling when human interaction exists.

`Human -> human-facing Agent(s) -> internal Agents / Commands / Flows`

## Human-facing semantics

`human-facing` is a default characteristic and may be explicitly overridden by workflow/profile.

Human-facing roles SHOULD have representative Human prompt scenarios sufficient to show their expected conversational surface without embedding workflow orchestration.

## Interaction mode

- `reactive` — acts when invoked/routed by Human, role, flow, event or schedule.
- `proactive` — may initiate work/communication when mandate/runtime allow it.
- `mixed` — supports both.

Interaction mode does not grant authority.

## Override rule

Reusable role properties are defaults. Workflow/profile specialization may override explicitly but SHOULD NOT silently broaden authority, privacy access, command permissions or memory scope.

## Required role sections

Every role SHOULD define purpose/responsibility, properties, prompt/intent scenarios, responsibilities, boundaries, memory/lifecycle behavior, Human interaction expectations and conceptual command/tool needs where relevant.

Workflow-specific peer relationships and orchestration belong to the workflow/team definition, not reusable role prose.

## Acceptance checklist

- [ ] Purpose/responsibility is defined.
- [ ] Required Properties are declared.
- [ ] Prompt/intent scenario table exists even when empty.
- [ ] Human-facing role has representative Human prompt scenarios.
- [ ] Role prompt scenarios describe intent, not workflow-specific orchestration.
- [ ] Workflow-routing-required scenarios direct implementation to active workflow/team routing.
- [ ] Boundaries and Human interaction expectations are defined.
- [ ] Role implementation inherits common communication protocol.
- [ ] Role itself grants no concrete commands.
- [ ] Workflow represents Human/team routing where applicable.