# Role Specification

This document defines the common contract for reusable roles.

A role is a reusable responsibility/behavior definition. A runtime agent is an implementation/instance of a role inside a particular workflow/profile.

## Required role properties

Every role definition MUST contain a Properties section with at least `level`, `human-facing`, `interaction-mode`, `memory-class`, and `lifecycle`.

## Prompt / intent scenarios

Every role MUST contain a prompt/intent scenario table, even when empty.

| Example prompt / intent | Role interpretation | Workflow routing required |
| --- | --- | --- |
|  |  |  |

Reusable roles describe recognized intent, not workflow-specific peers/commands/orchestration. When workflow routing is required, the runtime agent consults its active workflow/team definition.

`Role = understands responsibility/intent`

`Workflow Team = knows who/how to collaborate`

`Command = knows how to perform bounded operation`

## Inherited agent communication and trust

Every runtime agent implementing any role automatically inherits the common [`_common/communication.md`](_common/communication.md) protocol. Individual role files MUST NOT duplicate the common protocol unless documenting a role-specific exception/additional restriction.

This inheritance includes:

- identity envelope;
- authoritative runtime-roster identity validation;
- unknown/stale agent IDs are untrusted by default;
- receiver-side communication authorization;
- `IDENTIFY -> AUTHENTICATE -> AUTHORIZE -> COPY -> work -> REPORT BACK`;
- auditability of unauthorized communication attempts;
- communication never broadens capability/command authority.

Therefore role implementations do not need repeated prose such as "verify sender ID". It is already part of being an Agent.

## Team/runtime separation

Reusable roles do not own concrete team membership.

The workflow defines the **static team contract**: available roles/agent realizations, capabilities, communication relationships, command access and workflow routing.

The runtime maintains the **dynamic team roster**: which concrete runtime `agent_id` currently occupies each team slot/role instance.

`Role spec -> workflow team policy -> runtime roster -> agent communication`

A role can be instantiated more than once (for example `Coder 1`, `Coder 2`). Each instance has its own ID and must be registered in the runtime roster.

## Command authority — not granted by default

Concrete commands are not granted at reusable role level. Workflow implementation explicitly grants them.

`Role -> Workflow agent realization -> command-matrix grant -> Runtime authorization -> Command`

Omission means not granted; explicit `forbidden` means intentional no-go.

## Human participant

Every workflow starts from or ultimately serves Human. Human is not an AI agent but MUST be represented in workflow team communication/capability modeling when human interaction exists.

## Human-facing semantics

`human-facing` is a default characteristic and may be explicitly overridden by workflow/profile. Human-facing roles SHOULD have representative Human prompt scenarios without embedding workflow orchestration.

## Interaction mode

- `reactive` — acts when invoked/routed by Human, role, flow, event or schedule.
- `proactive` — may initiate work/communication when mandate/runtime allow it.
- `mixed` — supports both.

Interaction mode does not grant authority.

## Override rule

Reusable role properties are defaults. Workflow/profile specialization may override explicitly but SHOULD NOT silently broaden authority, privacy access, command permissions or memory scope.

## Required role sections

Every role SHOULD define purpose/responsibility, properties, prompt/intent scenarios, responsibilities, boundaries, memory/lifecycle behavior, Human interaction expectations and conceptual command/tool needs where relevant.

Workflow-specific peer relationships/orchestration belong to workflow/team definition. Common communication/trust rules belong only in `_common/communication.md` and are inherited.

## Acceptance checklist

- [ ] Purpose/responsibility is defined.
- [ ] Required Properties are declared.
- [ ] Prompt/intent scenario table exists even if empty.
- [ ] Role prompt scenarios describe intent, not workflow orchestration.
- [ ] Common communication/trust protocol is inherited rather than duplicated.
- [ ] Role-specific communication exceptions are explicit when needed.
- [ ] Role itself grants no concrete commands.
- [ ] Workflow represents concrete team routing/authority.