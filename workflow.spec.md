# Workflow Specification

This document defines the common contract and repository structure for every reusable workflow published here.

## Structural consistency principle

Every workflow MUST expose the same required skeleton even when parts are not populated yet. Absence of data is represented by an empty/template file, not by absence of a required file/folder.

## Core model

A workflow represents a reusable human or business activity. A flow is a bounded process inside a workflow.

`Role definition -> agents.md realization -> Team relationships -> command policy -> runtime agent`

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
    command-matrix.csv
```

Optional folders may include `strategies/`, `flows/`, `examples/`, `tests/`, and `adapters/`.

## agents.md

Required for every workflow. It defines workflow-local agent realizations and provider-independent runtime hints.

| Agent | Role | Human-facing override | Intelligence | Reasoning | Context | Memory | Lifecycle | Scheduled | Schedule intent | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |  |  |  |

### Scheduled agent semantics

Every agent realization MUST explicitly declare whether it is expected to run periodically/proactively without a direct conversational invocation.

- `Scheduled = yes` — runtime should support periodic/event-triggered activation for this agent.
- `Scheduled = no` — agent normally exists only when invoked/routed by a session, Human, agent, flow or event explicitly requiring it.
- `Schedule intent` describes WHY it wakes, not a hard-coded cron expression. Concrete cadence/timing belongs to runtime/profile configuration unless the workflow itself requires a specific cadence.

Scheduling does **not** mean continuous surveillance. A scheduled Judge, for example, may periodically sample/review activity for governance violations rather than observing every interaction in real time.

Scheduling also grants no extra commands, memory or authority. Every scheduled run uses the same team capability/communication/command boundaries as any other invocation.

Examples:

- Manager: `Scheduled=yes`, intent: periodically inspect work/ticket state and surface/perform allowed coordination actions.
- Judge: `Scheduled=yes`, intent: periodically audit agent/workflow activity for rule compliance and abuse of authority.
- Coder: normally `Scheduled=no`; implementation is invoked for bounded work.

## team/

Required shared coordination and authority contract.

### team/README.md

| Agent | Responsibility | Human-facing | Works with / delegates to |
| --- | --- | --- | --- |
|  |  |  |  |

### team/capability-matrix.csv

```csv
agent,capability,ownership,notes
,,,
```

### team/communication-matrix.csv

```csv
from_agent,to_agent,allowed,purpose
,,,
```

### team/command-matrix.csv

Every workflow MUST define per-agent AI Command access:

```csv
agent,command,access,notes
,,,
```

Commands are not granted by default. `allowed` explicitly grants eligibility subject to runtime authorization; `forbidden` documents an explicit no-go; omission means not granted.

Command Runner may route/execute configured AI Commands only after checking the caller's effective command policy and cannot lend its own broader access to callers. Direct command invocation uses the same policy.

## workflow.md

Required authoritative workflow contract with standard sections/placeholders for purpose/boundary, strategic layer, roles/composition, strategies, flows/events, Prompt routing/use cases, Connected commands, memory, inputs/outputs/evidence, runtime and privacy boundaries.

### Prompt routing / use cases

| Example prompt / intent | Route type | Route target | Result / notes |
| --- | --- | --- | --- |
|  |  |  |  |

### Connected commands

Required even when empty; references reusable commands from the [AI Commands repository](https://github.com/starodubtsevconsulting/ai-commands).

| Command | Reference | Used by / purpose |
| --- | --- | --- |
| None yet | — | Commands will be connected as the workflow is implemented. |

## Concepts

**Workflow** = long-lived domain/activity.

**Strategy** = HOW the domain is approached.

**Flow** = bounded process inside workflow.

**Role** = reusable responsibility definition.

**Agent** = workflow/runtime realization of a role.

**Scheduled agent** = agent realization that runtime periodically/event-triggeredly activates for a declared purpose; not a continuously running observer by implication.

**Team** = relationships/ownership/communication/command authority among agents.

**Command** = bounded reusable action.

## Memory

Workflows declare memory semantics using [`_common/memory.md`](_common/memory.md). Strategic roles may own persistent memory; execution roles normally use session context unless explicitly specified.

## Runtime boundary

Workflow definitions remain provider independent. Runtime/profile maps abstract requirements to concrete models, schedules/triggers, commands, credentials and integrations and provides final authorization.

## Minimum acceptance checklist

- [ ] required skeleton exists even when empty;
- [ ] `agents.md` includes `Scheduled` and `Schedule intent` columns;
- [ ] every defined agent explicitly declares scheduled yes/no;
- [ ] scheduled agents state why they wake;
- [ ] scheduling does not broaden authority;
- [ ] team matrices exist;
- [ ] command access is not granted unless explicit;
- [ ] Prompt routing/use cases and Connected commands exist;
- [ ] memory/runtime/privacy boundaries are represented.