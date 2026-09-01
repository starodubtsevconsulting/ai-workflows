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

## Human participant

Every workflow starts from or ultimately serves a **Human** participant. The Human is not an AI agent and does not need to be implemented as a reusable AI role, but MUST be represented in the workflow team's communication/capability model whenever human interaction exists.

The Human interacts with the workflow through one or more agents whose effective `human-facing` property is `true`.

The Human's practical organizational perspective may vary by workflow/profile/session. For example, the same human may participate as a developer/designer in one Software Development profile, as a CEO/product owner in another, or as the final decision-maker above the Workflow Strategist. This does not require cloning the Human into an AI role.

A useful model is:

`Human -> human-facing Agent(s) -> non-human-facing Agents / Commands / Flows`

If multiple agents are human-facing, the Human may interact with any/all of them as allowed by the workflow/profile. Non-human-facing agents are reached through routing/delegation rather than direct conversation by default.

The Human remains the final authority over their goals and may override/cancel workflow objectives subject to runtime safety/authorization boundaries.

## Human-facing semantics

`human-facing` is a DEFAULT characteristic of the reusable role, not an immutable permission. A concrete workflow/profile MAY override this default when realizing the role as an agent.

`true` means normally directly addressable by a human in an appropriate context. `false` means normally reached through another agent, flow or routing layer.

For example, a Software Development Human may talk directly to a Designer Reviewer while the Designer Reviewer asks a non-human-facing Manager to retrieve or create a ticket.

## Interaction mode

- `reactive` — acts when invoked/routed by a human, role, flow, event, or schedule.
- `proactive` — may initiate work/communication when its mandate/runtime allow it.
- `mixed` — supports both.

Interaction mode does not grant tool/command/autonomy permission.

## Override rule

Reusable role properties are defaults. Workflow/profile specialization may override them explicitly but SHOULD NOT silently broaden authority, privacy access, command permissions, or memory scope.

`Role defaults + Workflow specialization + Profile/runtime policy -> Agent profile/instance`

## Required role sections

Every role SHOULD define Purpose/responsibility, Properties, Responsibilities, Boundaries, Memory/lifecycle behavior, Human interaction expectations, relationships to other roles where relevant, and command/tool authority where relevant.

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
- [ ] `level` is declared.
- [ ] `human-facing` default is declared.
- [ ] `interaction-mode` is declared.
- [ ] `memory-class` is declared.
- [ ] `lifecycle` is declared.
- [ ] boundaries are defined.
- [ ] human interaction expectations are defined.
- [ ] overrides are explicit.
- [ ] workflows represent Human routing in team matrices where applicable.