# Workflow Specification

This document defines the common contract and repository structure for every reusable workflow published here.

## Core model

A workflow represents a reusable human or business activity. It is not an agent, model, harness, product, organization or one fixed pipeline.

A **flow** is a bounded process inside a workflow. One workflow may contain many flows.

Conceptually:

`Prompt/Event -> Workflow routing -> Flow / Role / Strategist / Command -> Outcomes/Evidence`

`Workflow + Roles + Strategy + Memory + Events + Commands + Flows -> runtime Agents -> Outcomes/Evidence`

A Workflow Strategist owns domain continuity and determines/adapts strategy and flow. The Global Governor sits above workflows and owns cross-workflow WHY/WHEN, allocation and human-aware strategy.

## Required folder structure

Every concrete workflow MUST be a top-level folder and MUST contain:

```text
<workflow-id>/
  README.md
  workflow.md
```

A workflow MAY additionally contain:

```text
<workflow-id>/
  strategies/
    <strategy>.md
  flows/
    <flow>.md
  roles/
    <workflow-specific-role>.md
  examples/
  tests/
  adapters/
```

Reusable cross-workflow definitions belong under `_common/`, not inside one workflow.

## README.md

Required human-facing entry point. It MUST explain, briefly:

- what activity/business process the workflow represents;
- the intended outcome;
- where to read the full contract (`workflow.md`);
- important workflow-specific entry points such as `strategies/` and `flows/` when present.

README is orientation, not the mechanical contract.

## workflow.md

Required authoritative workflow contract. It MUST define or explicitly address:

### Purpose and boundary
- what the workflow does;
- what outcome it is responsible for;
- what is outside its scope.

### Strategic layer
- how the Workflow Strategist participates;
- what persistent domain continuity/memory it requires;
- what decisions belong to the Strategist;
- what must be escalated to the Global Governor.

### Roles and composition
- which reusable/common roles may be selected;
- any workflow-specific roles;
- responsibilities/capability boundaries at the workflow level;
- which roles are persistent vs ephemeral.

Roles are definitions. Runtime agents are realizations of roles. A workflow composes agent profiles from roles rather than treating stored agent instances as the source abstraction.

### Strategies
A workflow MUST support the concept of strategy even when it publishes no reusable strategy files yet.

Strategy describes HOW this kind of work should be approached for a particular objective/context. Different strategies may produce very different flows for the same workflow.

When reusable strategies exist they SHOULD live in `strategies/`. The Strategist may select, combine, adapt or create strategy rather than blindly executing a named strategy file.

### Flows and events

A flow is a bounded process inside a workflow. Examples in Software Development include implementation, testing, debugging, review and release.

When a workflow has reusable named flows they SHOULD live under `flows/<flow>.md`. A flow SHOULD define its purpose/intent examples, meaningful events or states, expected outcome, and any important role/command boundaries.

A workflow MUST identify meaningful events/state changes that connect work. Strategy may alter how a flow is executed: its sequence, gates, participating roles, commands or iteration loops.

Therefore:

**Workflow** = long-lived domain/activity.

**Strategy** = HOW that domain should currently be approached.

**Flow** = bounded process used inside the workflow.

**Event** = fact/state change connecting steps and flows.

Conceptually:

`event -> flow/state/decision -> role/action/command -> result event`

A flow MAY invoke another flow. For example, Debugging may route a correction into Implementation and verification into Testing.

### Prompt routing / use cases

Every `workflow.md` MUST contain a **Prompt routing / use cases** table. This is the human-language routing contract: examples of what a person might ask and how the workflow interprets/routes that intent.

It is intentionally similar in spirit to an MCP/tool routing description, but represented as readable Markdown and owned by the workflow.

A prompt MAY route:

1. directly to a connected AI Command;
2. to a role/agent, which may then select/invoke one or more commands it is authorized to use;
3. to the Workflow Strategist when strategy, ambiguity, sequencing or cross-step reasoning is required;
4. to a named flow inside the current workflow.

Required table shape:

| Example prompt / intent | Route type | Route target | Result / notes |
| --- | --- | --- | --- |
| `<natural-language example>` | `command` / `role` / `strategist` / `flow` | command ID, role name, strategist, or flow | expected interpretation |

The table SHOULD contain several natural-language variants where useful. These are examples/semantic mappings, not exact phrases that users must type.

Example for Software Development:

| Example prompt / intent | Route type | Route target | Result / notes |
| --- | --- | --- | --- |
| “Push these changes.” | command | `source-control` | Direct bounded source-control operation when context/authorization is sufficient. |
| “Check the new ticket.” | role | `Manager` | Manager interprets the responsibility and may invoke a connected ticket-tracker command. |
| “Let's code this.” | flow | `implementation` | Enter the bounded implementation flow inside Software Development. |
| “Test this change.” | flow | `testing` | Enter the testing flow. |
| “Why is this failing?” | flow | `debugging` | Enter the debugging/investigation flow. |
| “Review this PR.” | flow | `review` | Enter independent review. |
| “Let's release it.” | flow | `release` | Enter release/readiness flow. |
| “What should we build first?” | strategist | `Workflow Strategist` | Requires prioritization/strategy rather than a direct bounded operation. |

A role route is deliberately different from a command route. The role owns reasoning/responsibility and may use commands as bounded capabilities.

`prompt -> Manager agent -> ticket-tracker command -> result -> Manager interpretation`

versus:

`prompt -> source-control command -> bounded result`

If a workflow has no implemented mappings yet, the section/table is still REQUIRED with a `TBD` row.

### Connected commands

Every `workflow.md` MUST contain a **Connected commands** table listing commands that the workflow can call directly or make available to its authorized roles/flows.

Commands are reusable bounded actions defined in the [AI Commands repository](https://github.com/starodubtsevconsulting/ai-commands). A workflow references a command rather than duplicating its specification or implementation.

Required table shape:

| Command | Reference | Used by / purpose |
| --- | --- | --- |
| None yet | — | Commands will be connected as the workflow is implemented. |

When commands are connected:

- use the canonical command identifier/path from `ai-commands`;
- link to the command definition in the AI Commands repository;
- describe why/where the workflow, role or flow may invoke it;
- do not duplicate the command's internal specification;
- runtime/profile configuration may further restrict command availability and authorization;
- declaring a command does not grant credentials or bypass runtime safety/permission boundaries.

The section and table are REQUIRED even when the workflow currently has no connected commands.

### Memory
The workflow MUST declare its memory semantics using [`_common/memory.md`](_common/memory.md).

Typical model:
- Global Governor: `GLOBAL_STRATEGIC`;
- Workflow Strategist: `WORKFLOW_STRATEGIC`;
- execution agents: `SESSION`.

Durable memory belongs to scopes, not disposable model sessions. Original source artifacts remain authoritative where applicable.

### Inputs, outputs and evidence
The workflow MUST identify its meaningful inputs and expected outputs/outcomes. Derived facts or decisions SHOULD preserve provenance/evidence when the domain requires it.

### Runtime boundary
The workflow MUST remain independent of one concrete model, provider, harness or hosting implementation unless that dependency is intrinsic to the workflow itself. Profiles/runtime configuration resolve roles into concrete agents and supply credentials, tools, paths, integrations and providers.

### Privacy boundary
Reusable public workflow contracts MUST NOT contain client secrets, credentials, private financial/personal data, absolute private paths or organization-specific confidential configuration.

## Common roles

Reusable roles live under:

```text
_common/
  roles/
    global/
    workflow/
```

`global/` contains roles above individual workflows, such as the Global Governor.

`workflow/` contains reusable roles that workflows can compose, including the Workflow Strategist and execution roles.

Workflow-specific roles may live inside the workflow only when they are not meaningfully reusable across workflows.

## Strategy versus flow

These are intentionally different concepts.

**Strategy** = philosophy/approach for achieving the workflow objective under current conditions.

**Flow** = bounded concrete process/sequence/graph of events, decisions and actions inside that workflow.

Changing strategy may change a flow. Evidence from a flow may cause the Strategist to change strategy. A workflow can have many flows simultaneously available.

## Portability rule

A workflow definition should answer: "Could another profile/runtime use this workflow without inheriting one person's private environment?"

If not, move environment-specific information into profile/runtime configuration or document it only as a sanitized example.

## Minimum acceptance checklist

A new workflow is structurally complete when:

- [ ] top-level workflow folder exists;
- [ ] `README.md` exists;
- [ ] `workflow.md` exists;
- [ ] purpose/outcome/boundary are defined;
- [ ] Workflow Strategist relationship is defined;
- [ ] roles/composition are defined;
- [ ] strategy semantics are defined;
- [ ] flow/event semantics are defined;
- [ ] reusable named flows are placed under `flows/` when present;
- [ ] Prompt routing / use cases table exists;
- [ ] Connected commands table exists and references `ai-commands`;
- [ ] memory class/boundary is defined;
- [ ] inputs/outputs/evidence are defined;
- [ ] runtime independence is defined;
- [ ] privacy/publication boundary is defined.