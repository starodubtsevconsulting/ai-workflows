# Workflow Specification

This document defines the common contract and repository structure for every reusable workflow published here.

## Core model

A workflow represents a reusable human or business activity. It is not an agent, model, harness, product, organization or one fixed pipeline.

A **flow** is a bounded process inside a workflow. One workflow may contain many flows.

Conceptually:

`Role definition -> workflow agents.md realization -> Team relationships -> runtime agent`

`Prompt/Event -> Workflow routing -> Flow / Agent / Strategist / Command -> Outcomes/Evidence`

`Workflow + Roles + Agent realizations + Team + Strategy + Memory + Events + Commands + Flows -> runtime Agents -> Outcomes/Evidence`

A Workflow Strategist owns domain continuity and determines/adapts strategy and flow. The Global Governor sits above workflows and owns cross-workflow WHY/WHEN, allocation and human-aware strategy.

## Required folder structure

Every concrete workflow MUST contain:

```text
<workflow-id>/
  README.md
  workflow.md
  agents.md
  team/
    README.md
    capability-matrix.csv
    communication-matrix.csv
```

A workflow MAY additionally contain:

```text
<workflow-id>/
  strategies/
    <strategy>.md
  flows/
    <flow>.md
  examples/
  tests/
  adapters/
```

Reusable role definitions belong under `_common/roles/`. Workflows SHOULD NOT duplicate reusable role definitions in local role MD files. They realize/specialize those roles through `agents.md`.

## README.md

Required human-facing entry point. It MUST briefly explain the activity/business process, intended outcome, and point to the workflow contract and relevant strategies/flows.

## agents.md

Every workflow MUST contain `agents.md`. It is the declarative runtime realization table: which reusable roles exist as agents in this workflow and their suggested runtime properties.

| Agent | Role | Human-facing override | Intelligence | Reasoning | Context | Memory | Lifecycle | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |  |

The role remains authoritative for responsibility/behavior. `agents.md` specializes realization. Values are provider-independent defaults/hints that runtime infrastructure maps to concrete models, context sizes, reasoning controls and memory implementations.

If no agents are defined yet, the file still exists with its header and one empty row.

`Reusable Role defaults + agents.md specialization + Profile/runtime policy -> concrete Agent`

A reusable role change SHOULD trigger review of all `agents.md` realizations referencing it.

## team/

Every workflow MUST contain a `team/` folder. It defines the shared coordination contract among the agents realized in `agents.md`.

### team/README.md

This is the compact **team introduction**. It SHOULD be suitable for inclusion in every participating agent's context so each agent understands who else exists and what to expect from them.

It MUST identify, when agents exist:

- team members;
- each member's brief responsibility;
- who is normally human-facing;
- major ownership/delegation expectations;
- relationships between agents;
- links to the capability and communication matrices.

Suggested table:

| Agent | Responsibility | Human-facing | Works with / delegates to |
| --- | --- | --- | --- |
|  |  |  |  |

The introduction SHOULD remain compact. Detailed role behavior belongs in reusable role definitions, and runtime characteristics belong in `agents.md`.

### team/capability-matrix.csv

Machine-readable ownership/capability boundaries for the team. It defines what each agent can own or perform at the workflow level and helps prevent agents from silently assuming another agent's responsibility.

At minimum it SHOULD be able to represent:

`agent,capability,permission/ownership,notes`

A workflow may extend the schema when needed.

### team/communication-matrix.csv

Machine-readable communication/delegation boundaries between team members. It defines which agents may communicate/delegate to which other agents and the intended relationship.

At minimum it SHOULD be able to represent:

`from_agent,to_agent,allowed,purpose/notes`

A workflow may extend the schema when needed.

### Team principle

Every participating agent SHOULD be able to answer from shared team context:

- Who am I?
- Who else is on the team?
- What do I own?
- What do I not own?
- Who should I ask/delegate to?
- Who may ask/delegate work to me?
- Which responsibilities require another agent rather than me doing the work myself?

`agents.md` answers **how agents are realized**.

`team/` answers **how those agents work together**.

`_common/roles/` answers **what each reusable role means**.

## workflow.md

Required authoritative workflow contract. It defines purpose/boundary, strategic layer, roles/composition, strategies, flows/events, prompt routing, connected commands, memory, inputs/outputs/evidence, runtime and privacy boundaries.

### Roles and composition

`workflow.md` describes which responsibilities participate and how they interact at the domain/process level. Concrete workflow-local agent realization belongs in `agents.md`; detailed coordination boundaries belong in `team/`.

### Strategies

A workflow MUST support strategy even when no reusable strategy files exist yet. Strategy describes HOW the activity should currently be approached.

### Flows and events

Reusable named flows SHOULD live under `flows/<flow>.md`. Strategy may alter sequence, gates, participating agents, commands and iteration loops.

**Workflow** = long-lived domain/activity.

**Strategy** = HOW that domain should currently be approached.

**Flow** = bounded process inside the workflow.

**Role** = reusable responsibility definition.

**Agent** = workflow/runtime realization of a role.

**Team** = shared relationships, ownership and communication contract among workflow agents.

**Command** = bounded reusable action.

**Event** = fact/state change connecting steps and flows.

### Prompt routing / use cases

Every `workflow.md` MUST contain:

| Example prompt / intent | Route type | Route target | Result / notes |
| --- | --- | --- | --- |
|  |  |  |  |

A prompt may route to a command, agent/role, Workflow Strategist, or named flow.

### Connected commands

Every `workflow.md` MUST contain a Connected commands table referencing reusable commands from the [AI Commands repository](https://github.com/starodubtsevconsulting/ai-commands), even when currently empty.

| Command | Reference | Used by / purpose |
| --- | --- | --- |
| None yet | — | Commands will be connected as the workflow is implemented. |

### Memory

The workflow MUST declare memory semantics using [`_common/memory.md`](_common/memory.md). Typical model: Global Governor `GLOBAL_STRATEGIC`, Workflow Strategist `WORKFLOW_STRATEGIC`, execution agents `SESSION`, with explicitly declared external memory where required.

### Runtime boundary

Workflow, agents and team definitions remain provider independent. Runtime/profile configuration maps abstract requirements to concrete models, providers, harnesses, credentials, tools and integrations.

### Privacy boundary

Reusable public workflow contracts MUST NOT contain secrets, private data, private absolute paths or confidential organization-specific configuration.

## Common roles

Reusable roles live under `_common/roles/` and follow [`role.spec.md`](role.spec.md).

## Portability rule

A workflow should be reusable by another profile/runtime without inheriting one person's private environment.

## Minimum acceptance checklist

- [ ] `README.md` exists;
- [ ] `workflow.md` exists;
- [ ] `agents.md` exists with required table, even if empty;
- [ ] `team/README.md` exists;
- [ ] `team/capability-matrix.csv` exists;
- [ ] `team/communication-matrix.csv` exists;
- [ ] agent rows reference reusable roles rather than duplicating them;
- [ ] team introduction identifies responsibilities/relationships when agents exist;
- [ ] capability ownership boundaries are represented;
- [ ] communication/delegation boundaries are represented;
- [ ] purpose/outcome/boundary are defined;
- [ ] strategy and flow/event semantics are defined;
- [ ] Prompt routing / use cases table exists;
- [ ] Connected commands table exists;
- [ ] memory boundary is defined;
- [ ] runtime independence is defined;
- [ ] privacy/publication boundary is defined.