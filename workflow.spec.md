# Workflow Specification

This document defines the common contract and repository structure for every reusable workflow published here.

## Structural consistency principle

Every workflow MUST expose the same required skeleton even when parts of it are not implemented or populated yet.

**Absence of data is represented by an empty/template file, not by absence of the file or folder.**

This makes the repository itself a visible contract: a human or runtime can inspect any workflow and immediately know where agents, team relationships, commands, routing and other standard concepts belong without first discovering which pieces happen to exist for that workflow.

Required files therefore MUST NOT be omitted merely because their tables currently contain no data. Required Markdown files should contain their heading/template structure; required CSV files should contain their canonical header and may contain an empty row where useful.

Optional/extensible folders such as `strategies/` and `flows/` may be absent until the workflow actually defines a strategy or named flow, unless they are later promoted to required structure by this specification.

## Core model

A workflow represents a reusable human or business activity. A flow is a bounded process inside a workflow.

`Role definition -> workflow agents.md realization -> Team relationships -> runtime agent`

`Prompt/Event -> Workflow routing -> Flow / Agent / Strategist / Command -> Outcomes/Evidence`

## Required folder structure

Every concrete workflow MUST contain exactly this common required skeleton:

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

Reusable role definitions belong under `_common/roles/`. Workflows realize/specialize them through `agents.md` rather than duplicating role definitions.

## README.md

Required even for an early workflow. It identifies the workflow and provides its human-facing entry point.

## agents.md

Required for every workflow, populated or not.

| Agent | Role | Human-facing override | Intelligence | Reasoning | Context | Memory | Lifecycle | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |  |

It defines workflow-local realizations of reusable roles and provider-independent runtime hints. If no agents are defined yet, retain the table with an empty row.

`Reusable Role defaults + agents.md specialization + Profile/runtime policy -> concrete Agent`

## team/

Required for every workflow, populated or not. It defines shared coordination context for the agents in `agents.md`.

### team/README.md

Required compact team introduction, suitable for sharing with participating agents.

| Agent | Responsibility | Human-facing | Works with / delegates to |
| --- | --- | --- | --- |
|  |  |  |  |

When populated it identifies members, brief responsibility, human-facing behavior, ownership/delegation expectations, relationships and links to matrices.

### team/capability-matrix.csv

Required canonical minimum shape:

```csv
agent,capability,ownership,notes
,,,
```

### team/communication-matrix.csv

Required canonical minimum shape:

```csv
from_agent,to_agent,allowed,purpose
,,,
```

The matrices may be extended when needed, but their required files and canonical headers remain present across workflows.

### Team principle

Every participating agent should be able to determine who it is, who else exists, what it owns, what it does not own, whom it may delegate to, and who may delegate to it.

`_common/roles/` = what reusable roles mean.

`agents.md` = how roles are realized in this workflow.

`team/` = how those agents work together.

## workflow.md

Required authoritative workflow contract. Even an early workflow keeps all required standard sections so its shape remains predictable.

### Required standard sections

Every `workflow.md` MUST address or retain placeholders for:

- purpose/boundary;
- strategic layer;
- roles/composition;
- strategies;
- flows/events;
- Prompt routing / use cases;
- Connected commands;
- memory;
- inputs/outputs/evidence;
- runtime boundary;
- privacy boundary.

### Prompt routing / use cases

Required even when empty:

| Example prompt / intent | Route type | Route target | Result / notes |
| --- | --- | --- | --- |
|  |  |  |  |

A prompt may route to a command, agent/role, Workflow Strategist, or named flow.

### Connected commands

Required even when no commands are connected. Commands reference the [AI Commands repository](https://github.com/starodubtsevconsulting/ai-commands).

| Command | Reference | Used by / purpose |
| --- | --- | --- |
| None yet | — | Commands will be connected as the workflow is implemented. |

## Concepts

**Workflow** = long-lived domain/activity.

**Strategy** = HOW that domain should currently be approached.

**Flow** = bounded process inside the workflow.

**Role** = reusable responsibility definition.

**Agent** = workflow/runtime realization of a role.

**Team** = shared relationships, ownership and communication contract among workflow agents.

**Command** = bounded reusable action.

**Event** = fact/state change connecting steps and flows.

## Memory

Workflows declare memory semantics using [`_common/memory.md`](_common/memory.md). Typical model: Global Governor `GLOBAL_STRATEGIC`, Workflow Strategist `WORKFLOW_STRATEGIC`, execution agents `SESSION`, with explicitly declared external memory only where required.

## Runtime boundary

Workflow, agent and team definitions remain provider independent. Runtime/profile configuration maps abstract requirements to concrete models, providers, harnesses, credentials, tools and integrations.

## Privacy boundary

Reusable public workflow contracts MUST NOT contain secrets, private data, private absolute paths or confidential organization-specific configuration.

## Common roles

Reusable roles live under `_common/roles/` and follow [`role.spec.md`](role.spec.md). A reusable role change SHOULD trigger review of every `agents.md` realization referencing it.

## Portability rule

A workflow should be reusable by another profile/runtime without inheriting one person's private environment.

## Minimum acceptance checklist

- [ ] required skeleton exists even when data is empty;
- [ ] `README.md` exists;
- [ ] `workflow.md` exists with all standard sections/placeholders;
- [ ] `agents.md` exists with canonical table/header even if empty;
- [ ] `team/README.md` exists with canonical table/header even if empty;
- [ ] `team/capability-matrix.csv` exists with canonical header even if empty;
- [ ] `team/communication-matrix.csv` exists with canonical header even if empty;
- [ ] agent rows reference reusable roles rather than duplicating them;
- [ ] Prompt routing / use cases table exists even if empty;
- [ ] Connected commands table exists even if no commands are connected;
- [ ] purpose/outcome/boundary are represented;
- [ ] strategy and flow/event semantics are represented;
- [ ] memory boundary is represented;
- [ ] runtime independence is represented;
- [ ] privacy/publication boundary is represented.