# Role Specification

This document defines the common contract for reusable roles.

A role is a reusable responsibility/behavior definition. A runtime agent is an implementation/instance of a role inside a particular workflow/profile.

## Required role properties

Every role definition MUST contain a Properties section with at least `level`, `human-facing`, `interaction-mode`, `memory-class`, and `lifecycle`.

## Agent communication protocol

Every runtime agent implementing a role MUST follow the common [`_common/communication.md`](_common/communication.md) protocol for agent-to-agent communication.

In particular:

- every outgoing agent message identifies sender `agent_id`, `agent_name`, `profile`, `workflow`, and `project` when applicable, plus contextual identifiers such as branch/task/session when relevant;
- delegated work is accepted with `COPY` only after the receiver understands and accepts responsibility;
- `COPY` creates an obligation to report back;
- the receiver reports `DONE`, `BLOCKED`, `REFUSED`, `FAILED`, or a material `UPDATE` as appropriate;
- after `COPY`, the delegator may rely on report-back rather than continuously polling/watching the receiver;
- communication never broadens authority.

This is a runtime behavior requirement inherited by every role implementation, not something each reusable role should redefine independently.

## Command authority — not granted by default

Concrete commands are not granted at the reusable role level. This is a default, not permanent prohibition. A workflow implementation may explicitly grant commands to an agent realizing the role.

`Role (no command grants) -> Workflow agent realization -> command-matrix explicit grant -> Runtime/profile authorization -> Command execution`

Omission means not granted; explicit `forbidden` means intentional no-go. Every implementation evaluates effective command policy before direct or Command Runner invocation.

## Human participant

Every workflow starts from or ultimately serves a Human participant. Human is not an AI agent but MUST be represented in workflow team communication/capability modeling when human interaction exists.

`Human -> human-facing Agent(s) -> non-human-facing Agents / Commands / Flows`

Human remains final authority over goals subject to runtime safety/authorization boundaries.

## Human-facing semantics

`human-facing` is a default characteristic, not immutable permission. A workflow/profile may explicitly override it.

## Interaction mode

- `reactive` — acts when invoked/routed by human, role, flow, event or schedule.
- `proactive` — may initiate work/communication when mandate/runtime allow it.
- `mixed` — supports both.

Interaction mode does not grant command/tool/autonomy permission.

## Override rule

Reusable role properties are defaults. Workflow/profile specialization may override them explicitly but SHOULD NOT silently broaden authority, privacy access, command permissions or memory scope.

`Role defaults + Workflow specialization + Profile/runtime policy -> Agent profile/instance`

## Required role sections

Every role SHOULD define purpose/responsibility, properties, responsibilities, boundaries, memory/lifecycle behavior, Human interaction expectations, relationships to other roles where relevant, and conceptual command/tool needs where relevant. Concrete command grants belong to workflow `team/command-matrix.csv`.

## Acceptance checklist

- [ ] Purpose/responsibility is defined.
- [ ] Required Properties are declared.
- [ ] boundaries and human interaction expectations are defined.
- [ ] role implementation inherits the common communication protocol.
- [ ] role itself grants no concrete commands.
- [ ] unlisted commands remain not granted unless workflow policy grants them.
- [ ] workflows represent Human routing where applicable.