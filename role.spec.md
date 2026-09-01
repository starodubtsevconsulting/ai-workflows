# Role Specification

This document defines the common contract for reusable roles.

A role is a reusable responsibility/behavior definition. A runtime agent is an implementation/instance of a role inside a particular workflow/profile.

## Human-facing `why.md`

Every reusable role MUST have a companion human-facing `why.md` explanation.

`role definition = what it is / what it must do`

`why.md = why we introduced it / what problem it solves`

`why.md` is explanatory/non-normative and may use stories, examples, analogies, diagrams/media, trade-offs and references. Keep normative authority in role/spec files and avoid duplicating full role contract.

## Required role properties

Every role definition MUST contain Properties with at least `level`, `human-facing`, `interaction-mode`, `memory-class`, and `lifecycle`.

## Role capabilities versus concrete commands

Reusable roles describe **conceptual capabilities/responsibilities**, never concrete AI Command dependencies.

Examples:

- Coder may require **source-control capability** as part of normal software implementation.
- UI Acceptance Tester may require **computer-use/vision capability** and **UI automation capability**.
- Manager may require **work/ticket-tracking capability**.

A reusable role MUST NOT bind those concepts to concrete AI Commands such as `source-control`, `computer-use`, `logs`, or `ticket-tracker`, and MUST NOT link to a concrete AI Command as the implementation of its capability.

Concrete binding belongs to workflow agent realization:

`Role conceptual capability -> agents.md implementation binding -> Team command authorization -> Command/provider/harness`

This rule is intentionally strict: a concrete AI Command reference in a reusable role's normative behavior is a governance violation unless the text is explicitly discussing the architecture/specification rule itself rather than binding role behavior.

A role may describe what a capability means in domain language. It must remain valid if the workflow later implements that capability with a different command, harness-native feature, provider or mechanism.

## Prompt / intent scenarios

Every role MUST contain a prompt/intent scenario table, even when empty.

| Example prompt / intent | Role interpretation | Workflow routing required |
| --- | --- | --- |
|  |  |  |

Reusable roles describe recognized intent, not workflow-specific peers/commands/orchestration. When routing is required, runtime agent consults active workflow/team definition.

`Role = understands responsibility/intent/capabilities`

`Workflow Agent = binds conceptual capabilities to implementation`

`Workflow Team = knows routing and authorization`

`Command = performs bounded operation`

## Inherited agent communication and trust

Every runtime agent inherits [`_common/communication.md`](_common/communication.md). Individual role files MUST NOT duplicate common protocol unless documenting role-specific exception/restriction.

## Team/runtime separation

Reusable roles do not own concrete team membership. Workflow defines static Team contract; runtime maintains dynamic roster mapping concrete IDs to team slots/instances.

## Command authority — not granted by default

Concrete commands are never granted at reusable role level. Workflow implementation binds capabilities and grants commands.

`Role capability -> Workflow agent realization -> command-matrix grant -> Runtime authorization -> Command`

Omission means not granted; explicit `forbidden` means intentional no-go.

An implementation binding in `agents.md` does not itself grant authority. `command-matrix.csv` remains authoritative for command permission.

## Human participant

Every workflow starts from or ultimately serves Human. Human is not an AI agent but MUST be represented in workflow team communication/capability modeling when interaction exists.

## Human-facing semantics

`human-facing` is a default characteristic and may be explicitly overridden by workflow/profile.

## Interaction mode

- `reactive` — acts when invoked/routed by Human, role, flow, event or schedule.
- `proactive` — may initiate work/communication when mandate/runtime allow it.
- `mixed` — supports both.

Interaction mode does not grant authority.

## Override rule

Reusable role properties are defaults. Workflow/profile specialization may override explicitly but SHOULD NOT silently broaden authority, privacy access, command permissions or memory scope.

## Required role sections

Every role SHOULD define purpose/responsibility, properties, prompt/intent scenarios, responsibilities, boundaries, memory/lifecycle behavior, Human interaction expectations and **conceptual capability needs** where relevant.

## Acceptance checklist

- [ ] Purpose/responsibility is defined.
- [ ] Required Properties are declared.
- [ ] Companion human-facing `why.md` exists.
- [ ] Prompt/intent scenario table exists even if empty.
- [ ] Role describes conceptual capabilities rather than concrete AI Commands.
- [ ] No concrete command is bound/linked as a role implementation.
- [ ] Common communication/trust protocol is inherited rather than duplicated.
- [ ] Role itself grants no concrete commands.
- [ ] Workflow agent realization binds required capabilities to implementations.
- [ ] Team command matrix remains authoritative for command permission.