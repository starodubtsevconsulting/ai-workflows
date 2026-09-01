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

## Command authority — deny by default

Every reusable role has **zero command authority by default**.

A role definition describes responsibilities and behavior but MUST NOT implicitly grant access to AI Commands, shell/tools, integrations or external systems.

When a role is realized as an agent inside a workflow, its command access MUST be resolved from that workflow's `team/command-matrix.csv` (and then further constrained by runtime/profile authorization).

Therefore an implementation of any role MUST assume:

`command not explicitly allowed for this workflow agent -> forbidden`

The permission chain is:

`Role (zero grants) -> Workflow agent realization -> team/command-matrix.csv explicit allow -> Runtime/profile authorization -> Command execution`

All gates must pass.

A role MAY describe the kinds of capabilities it conceptually needs (for example, Manager coordinates ticket operations), but that statement is not permission to invoke `ticket-tracker`. The concrete Software Development Manager receives that permission only because its workflow command matrix explicitly grants it.

Every agent implementation MUST be connected to/evaluate its effective command policy before invoking a command directly or through Command Runner. Command Runner MUST evaluate the caller's policy and cannot lend its own broader command access to another agent.

Explicit `forbidden` entries are useful for important boundaries, but omission remains denial.

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
- [ ] role implementations assume commands are forbidden unless explicitly allowed by workflow policy.
- [ ] workflows represent Human routing where applicable.