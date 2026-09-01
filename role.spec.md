# Role Specification

This document defines the common contract for reusable roles.

A role is a reusable responsibility/behavior definition. A runtime agent is an implementation/instance of a role inside a particular workflow/profile.

## Human-facing `why.md`

Every reusable role MUST have a companion human-facing `why.md` explanation.

The normative role definition is written primarily for AI/runtime consumption: compact, explicit and rule-oriented. `why.md` exists for a person who asks **why this role exists at all**.

`role definition = what it is / what it must do`

`why.md = why we introduced it / what problem it solves`

`why.md` is explanatory and non-normative. It may be relaxed and story-like. It MAY include examples, analogies, diagrams/images/animations, historical context, trade-offs, external references and links to related roles/commands/workflows.

A good `why.md` should let someone understand the design decision without first learning the complete agent architecture. For example it may begin with an imagined situation: "Imagine a high-level agent knows what outcome it wants, but has no idea which operational command should perform it..."

Rules:

- every reusable role has a human-facing `why.md`;
- keep normative authority in the role/spec files, not `why.md`;
- do not duplicate the full role contract into `why.md`;
- explain motivation, problem and important trade-offs in ordinary human language;
- references/media are welcome when they improve understanding;
- when role behavior changes materially, check whether its explanation is still true.

Current flat role layout may use `<role>.why.md` beside `<role>.md`. A future folder-per-role layout may use `role.md` + `why.md`; either representation preserves the same conceptual contract.

## Required role properties

Every role definition MUST contain a Properties section with at least `level`, `human-facing`, `interaction-mode`, `memory-class`, and `lifecycle`.

## Prompt / intent scenarios

Every role MUST contain a prompt/intent scenario table, even when empty.

| Example prompt / intent | Role interpretation | Workflow routing required |
| --- | --- | --- |
|  |  |  |

Reusable roles describe recognized intent, not workflow-specific peers/commands/orchestration. When workflow routing is required, runtime agent consults active workflow/team definition.

`Role = understands responsibility/intent`

`Workflow Team = knows who/how to collaborate`

`Command = knows how to perform bounded operation`

## Inherited agent communication and trust

Every runtime agent implementing any role automatically inherits [`_common/communication.md`](_common/communication.md). Individual role files MUST NOT duplicate common protocol unless documenting role-specific exception/restriction.

This includes identity envelope, runtime-roster validation, unknown/stale IDs untrusted by default, receiver-side authorization, `IDENTIFY -> AUTHENTICATE -> AUTHORIZE -> COPY -> work -> REPORT BACK`, auditability and no authority broadening.

## Team/runtime separation

Reusable roles do not own concrete team membership. Workflow defines static Team contract; runtime maintains dynamic roster mapping concrete IDs to team slots/instances.

`Role spec -> workflow team policy -> runtime roster -> agent communication`

Multiple instances of one role are allowed and separately registered.

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
- [ ] Companion human-facing `why.md` exists.
- [ ] `why.md` explains motivation without becoming normative duplicate.
- [ ] Prompt/intent scenario table exists even if empty.
- [ ] Role prompt scenarios describe intent, not workflow orchestration.
- [ ] Common communication/trust protocol is inherited rather than duplicated.
- [ ] Role-specific communication exceptions are explicit when needed.
- [ ] Role itself grants no concrete commands.
- [ ] Workflow represents concrete team routing/authority.