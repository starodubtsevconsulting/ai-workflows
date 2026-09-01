# Workflow Specification

This document defines the common contract and repository structure for every reusable workflow published here.

## Core model

A workflow represents a reusable human or business activity. It is not an agent, model, harness, product, organization or one fixed pipeline.

A **flow** is a bounded process inside a workflow. One workflow may contain many flows.

Conceptually:

`Role definition -> workflow agents.md realization -> runtime agent`

`Prompt/Event -> Workflow routing -> Flow / Agent / Strategist / Command -> Outcomes/Evidence`

`Workflow + Roles + Agent realizations + Strategy + Memory + Events + Commands + Flows -> runtime Agents -> Outcomes/Evidence`

A Workflow Strategist owns domain continuity and determines/adapts strategy and flow. The Global Governor sits above workflows and owns cross-workflow WHY/WHEN, allocation and human-aware strategy.

## Required folder structure

Every concrete workflow MUST be a top-level folder and MUST contain:

```text
<workflow-id>/
  README.md
  workflow.md
  agents.md
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

Required human-facing entry point. It MUST explain, briefly, what activity/business process the workflow represents, the intended outcome, where to read the full contract (`workflow.md`), and important workflow-specific entry points such as `strategies/` and `flows/` when present.

## agents.md

Every workflow MUST contain `agents.md`. It is the workflow's declarative agent-realization table: which reusable roles exist as agents in this workflow and the default runtime properties suggested for them.

It does NOT duplicate the role definition. The role remains authoritative for responsibility/behavior. `agents.md` specializes how that role should normally be realized in this workflow.

Required table:

| Agent | Role | Human-facing override | Intelligence | Reasoning | Context | Memory | Lifecycle | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |  |

Semantics:

- **Agent** — workflow-local agent name/identity.
- **Role** — link/reference to the reusable role under `_common/roles/`.
- **Human-facing override** — optional override of the role default; blank means inherit.
- **Intelligence** — relative capability requirement such as `low`, `medium`, `high`, `highest-available`; this is provider/model independent.
- **Reasoning** — suggested reasoning effort such as `low`, `medium`, `high`.
- **Context** — suggested context requirement such as `small`, `medium`, `large`, `largest-available`.
- **Memory** — memory requirement/class; may include optional external memory where continuity requires it.
- **Lifecycle** — persistent or ephemeral/session behavior, overriding only when explicitly intended.
- **Notes** — workflow-specific realization details.

These values are defaults/hints, not bindings to a particular model/provider. Runtime infrastructure maps them to available concrete models, context sizes, reasoning controls and memory implementations.

For example, a Coder may use medium intelligence/reasoning and be replaced frequently between bounded tasks, while a Designer Reviewer may require high intelligence/reasoning, large context and optional external continuity because it must preserve design intent through later implementation review.

If the workflow has no defined agent realizations yet, `agents.md` MUST still exist with the header and one empty row.

Conceptually:

`Reusable Role defaults + agents.md specialization + Profile/runtime policy -> concrete Agent`

A change to a reusable role SHOULD trigger review of all `agents.md` realizations referencing that role.

## workflow.md

Required authoritative workflow contract. It MUST define purpose/boundary, strategic layer, roles/composition, strategies, flows/events, prompt routing, connected commands, memory, inputs/outputs/evidence, runtime and privacy boundaries.

### Roles and composition

`workflow.md` describes which responsibilities participate and how they interact. Concrete workflow-local agent realization belongs in `agents.md`.

Roles are reusable definitions. Agents are workflow/profile/runtime realizations of roles.

### Strategies

A workflow MUST support strategy even when it publishes no reusable strategy files yet. Strategy describes HOW this kind of work should be approached for a particular objective/context. Different strategies may produce different flows.

### Flows and events

A flow is a bounded process inside a workflow. Reusable named flows SHOULD live under `flows/<flow>.md`. Strategy may alter sequence, gates, participating agents, commands and iteration loops.

**Workflow** = long-lived domain/activity.

**Strategy** = HOW that domain should currently be approached.

**Flow** = bounded process used inside the workflow.

**Role** = reusable responsibility definition.

**Agent** = workflow/runtime realization of a role.

**Command** = bounded reusable action.

**Event** = fact/state change connecting steps and flows.

### Prompt routing / use cases

Every `workflow.md` MUST contain a **Prompt routing / use cases** table.

| Example prompt / intent | Route type | Route target | Result / notes |
| --- | --- | --- | --- |
|  |  |  |  |

A prompt may route directly to a command, to an agent/role, to the Workflow Strategist, or to a named flow. Examples are semantic mappings rather than exact required phrases.

### Connected commands

Every `workflow.md` MUST contain a **Connected commands** table referencing reusable commands from the [AI Commands repository](https://github.com/starodubtsevconsulting/ai-commands).

| Command | Reference | Used by / purpose |
| --- | --- | --- |
| None yet | — | Commands will be connected as the workflow is implemented. |

The section/table remain required even when no commands exist yet. Runtime/profile authorization may further restrict any declared command.

### Memory

The workflow MUST declare memory semantics using [`_common/memory.md`](_common/memory.md). Typical model: Global Governor `GLOBAL_STRATEGIC`, Workflow Strategist `WORKFLOW_STRATEGIC`, execution agents `SESSION`, with explicitly declared external memory where required.

### Runtime boundary

Workflow and `agents.md` remain provider independent. Runtime/profile configuration maps abstract intelligence/reasoning/context/memory requirements to concrete models, providers, harnesses, credentials, tools and integrations.

### Privacy boundary

Reusable public workflow contracts MUST NOT contain client secrets, credentials, private financial/personal data, absolute private paths or confidential organization-specific configuration.

## Common roles

Reusable roles live under `_common/roles/` and follow [`role.spec.md`](role.spec.md). Global and workflow-strategic roles may use their existing subfolders. Execution roles remain reusable definitions rather than duplicated workflow-local files.

## Portability rule

A workflow definition should answer: "Could another profile/runtime use this workflow without inheriting one person's private environment?" If not, move environment-specific information into profile/runtime configuration or document it only as a sanitized example.

## Minimum acceptance checklist

- [ ] top-level workflow folder exists;
- [ ] `README.md` exists;
- [ ] `workflow.md` exists;
- [ ] `agents.md` exists;
- [ ] `agents.md` has the required agent-realization table, even if empty;
- [ ] agent rows reference reusable roles rather than duplicating them;
- [ ] purpose/outcome/boundary are defined;
- [ ] Workflow Strategist relationship is defined;
- [ ] strategy semantics are defined;
- [ ] flow/event semantics are defined;
- [ ] Prompt routing / use cases table exists;
- [ ] Connected commands table exists;
- [ ] memory boundary is defined;
- [ ] runtime independence is defined;
- [ ] privacy/publication boundary is defined.