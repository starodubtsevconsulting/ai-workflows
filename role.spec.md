# Role Specification

This document defines the common contract for reusable roles.

A role is a reusable responsibility/behavior definition. A runtime agent is an implementation/instance of a role inside a particular workflow/profile.

## Required role properties

Every role definition MUST contain a **Properties** section with at least:

| Property | Meaning |
| --- | --- |
| `level` | `global`, `workflow`, or `execution` scope. |
| `human-facing` | Default: whether a human is expected to interact directly with this role. |
| `interaction-mode` | Default interaction posture, e.g. `proactive`, `reactive`, or `mixed`. |
| `memory-class` | Default memory class from `_common/memory.md`. |
| `lifecycle` | Typical persistence: persistent or session/ephemeral. |

Example:

```text
## Properties

- level: execution
- human-facing: true
- interaction-mode: reactive
- memory-class: SESSION
- lifecycle: ephemeral
```

## Human-facing semantics

`human-facing` is a DEFAULT characteristic of the reusable role, not an immutable permission.

- `true` means the role is normally suitable as a direct conversational surface for a human.
- `false` means the role normally operates behind another role/flow and is not expected to be directly addressed by the human.

A concrete workflow/profile MAY override this default when realizing the role as an agent.

Human-facing behavior is also contextual to the human's relationship to the workflow. A developer may interact directly with a Designer or Coder; a CEO-level interaction may instead route through a Strategist, CTO-like role, Manager, or Global Governor. Therefore `human-facing` means “normally directly addressable in an appropriate context,” not “always visible to every human.”

## Interaction mode

- `reactive` — normally acts when invoked/routed by a human, role, flow, event, or schedule.
- `proactive` — may initiate work/communication when its mandate and runtime allow it.
- `mixed` — supports both.

Interaction mode does not itself grant scheduling, messaging, tool, command, or autonomous execution permission.

## Override rule

Reusable role properties are defaults. A workflow/profile MAY specialize them when creating an agent profile, but the override SHOULD be explicit and SHOULD NOT silently broaden authority, privacy access, command permissions, or memory scope.

Conceptually:

`Role defaults + Workflow specialization + Profile/runtime policy -> Agent profile/instance`

## Required role sections

Every role SHOULD define:

- Purpose / responsibility;
- Properties;
- Responsibilities;
- Boundaries / what it must not own;
- Memory/lifecycle behavior;
- Human interaction expectations;
- Relationship to other roles where relevant;
- command/tool authority where relevant.

## Example defaults

These are examples, not universal organizational rules:

| Role | Human-facing default | Why |
| --- | --- | --- |
| Designer | `true` | Humans commonly discuss requirements, trade-offs and design directly. |
| Manager | `false` | Often a reactive coordination/technical-management role behind routing/flows; a workflow may override it to `true`. |
| Coder | `true` | A developer may directly request implementation work. |
| Reviewer | `false` | Commonly invoked by a flow/another role for independent review. |
| Command Runner | `false` | Execution capability normally sits behind a role/flow rather than being a conversational persona. |
| Workflow Strategist | `true` | Appropriate direct surface for workflow-level strategic questions. |
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
- [ ] overrides are treated as explicit workflow/profile specialization.