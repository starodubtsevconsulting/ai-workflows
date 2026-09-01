# Role Specification

This document defines the common contract for reusable roles.

A role is a reusable responsibility/behavior definition. A runtime agent is an implementation/instance of a role inside a particular workflow/profile.

## Required role properties

Every role definition MUST contain a **Properties** section with at least:

| Property | Meaning |
| --- | --- |
| `level` | `global`, `workflow`, or `execution` scope. |
| `human-facing` | Default: whether a human is expected to interact directly with this role. |
| `interaction-mode` | Default interaction posture: `proactive`, `reactive`, or `mixed`. |
| `memory-class` | Default memory class from `_common/memory.md`. |
| `lifecycle` | Typical persistence: persistent or session/ephemeral. |

## Command authority — not granted by default

Concrete commands are **not granted at the reusable role level**.

This is a default, not a permanent prohibition. A workflow implementation may explicitly grant commands to an agent that realizes the role.

A role describes responsibilities and behavior but does not itself grant access to AI Commands, shell/tools, integrations or external systems.

When the role is realized as an agent, command access is resolved from the workflow's `team/command-matrix.csv` and then further constrained by runtime/profile authorization.

Therefore:

`command not explicitly granted to this workflow agent -> not allowed to execute`

while:

`command explicitly granted to this workflow agent -> eligible for execution, subject to runtime authorization`

The permission chain is:

`Role (no command grants) -> Workflow agent realization -> command-matrix explicit grant -> Runtime/profile authorization -> Command execution`

An explicit `forbidden` entry has a stronger/documentary meaning: the workflow intentionally declares that command as a no-go for that agent. Omission simply means **not granted**.

A role MAY describe conceptual capabilities it needs. For example, Manager coordinates ticket operations. That does not itself grant `ticket-tracker`; a concrete workflow Manager receives it only when the workflow command matrix grants it.

Every agent implementation MUST evaluate its effective command policy before invoking a command directly or through Command Runner. Command Runner evaluates the caller's policy and cannot lend its own broader access to another agent.

## Human participant

Every workflow starts from or ultimately serves a Human participant. Human is not an AI agent but MUST be represented in workflow team communication/capability modeling when human interaction exists.

Human interacts through agents whose effective `human-facing` property is true. The same human may participate from different organizational perspectives depending on workflow/profile/session.

`Human -> human-facing Agent(s) -> non-human-facing Agents / Commands / Flows`

Human remains final authority over their goals subject to runtime safety/authorization boundaries.

## Human-facing semantics

`human-facing` is a default characteristic, not immutable permission. A workflow/profile may explicitly override it.

`true` means normally directly addressable by a human in an appropriate context. `false` means normally reached through another agent, flow or routing layer.

## Interaction mode

- `reactive` — acts when invoked/routed by human, role, flow, event or schedule.
- `proactive` — may initiate work/communication when mandate/runtime allow it.
- `mixed` — supports both.

Interaction mode does not grant command/tool/autonomy permission.

## Override rule

Reusable role properties are defaults. Workflow/profile specialization may override them explicitly but SHOULD NOT silently broaden authority, privacy access, command permissions or memory scope.

`Role defaults + Workflow specialization + Profile/runtime policy -> Agent profile/instance`

## Required role sections

Every role SHOULD define Purpose/responsibility, Properties, Responsibilities, Boundaries, Memory/lifecycle behavior, Human interaction expectations, relationships to other roles where relevant, and conceptual command/tool needs where relevant. Concrete command grants belong to workflow `team/command-matrix.csv`, never the reusable role.

## Example defaults

| Role | Human-facing default | Why |
| --- | --- | --- |
| Designer Reviewer | `true` | Humans commonly discuss requirements, trade-offs, design and conformance findings directly. |
| Manager | `false` | Reactive coordination/technical-management role normally behind routing/flows. |
| Coder | `true` | A developer may directly request bounded implementation work. |
| Reviewer | `false` | Normally invoked for independent review. |
| Command Runner | `false` | Execution capability normally sits behind a role/flow. |
| Workflow Strategist | `true` | Direct surface for workflow-level strategic questions. |
| Global Governor | `true` | Primary human-aware strategic role above workflows. |

## Acceptance checklist

- [ ] Purpose/responsibility is defined.
- [ ] Properties section exists.
- [ ] `level`, `human-facing`, `interaction-mode`, `memory-class`, `lifecycle` are declared.
- [ ] boundaries are defined.
- [ ] human interaction expectations are defined.
- [ ] role itself grants no concrete commands.
- [ ] role implementations treat unlisted commands as not granted unless workflow policy explicitly grants them.
- [ ] `forbidden` is reserved for intentional explicit no-go declarations.
- [ ] workflows represent Human routing where applicable.