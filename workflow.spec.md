# Workflow Specification

This document defines the common contract and repository structure for every reusable workflow published here.

## Structural consistency principle

Every workflow MUST expose the same required skeleton even when parts are not populated yet. Absence of data is represented by an empty/template file, not by absence of a required file/folder.

## Core model

A workflow represents a reusable human or business activity. A flow is a bounded process inside a workflow.

`Role definition -> agents.md realization -> Team relationships -> command policy -> runtime agent`

`Caller agent -> Command Runner/direct command routing -> command-policy check -> AI Command -> bounded result`

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

Reusable role definitions belong under `_common/roles/`; workflows specialize them through `agents.md`.

## agents.md

Required for every workflow:

| Agent | Role | Human-facing override | Intelligence | Reasoning | Context | Memory | Lifecycle | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |  |

It defines workflow-local agent realizations and provider-independent runtime hints.

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

Every workflow MUST define per-agent AI Command access. This is the authoritative workflow-level command authorization matrix used by Command Runner/direct command routing.

Canonical shape:

```csv
agent,command,access,notes
,,,
```

`access` is normally one of:

- `allowed` — the agent may request/invoke this command subject to runtime/command-level authorization;
- `forbidden` — explicit no-go even if the command exists globally;
- absence of a command row — **not allowed by default**.

The policy is therefore **allowlist-first / fail-closed**. A command existing in the AI Commands repository does not make it available to every agent.

Explicit `forbidden` rows are encouraged for important boundaries because they document intentional no-go capabilities rather than relying only on omission.

A workflow may expose many globally available AI Commands while granting each agent only the subset appropriate to its responsibility.

Conceptually:

`AI Commands catalog` = everything the runtime could potentially know how to execute.

`workflow Connected commands` = commands relevant/connected to this workflow.

`team/command-matrix.csv` = which workflow agents are actually allowed or explicitly forbidden to request each connected command.

`runtime/command authorization` = final environmental/credential/safety gate.

All gates must pass.

### Command Runner authorization behavior

Command Runner may understand/route to any command defined in the configured AI Commands catalog, but MUST execute on behalf of a caller only after checking the caller's effective command policy.

Example:

`Designer Reviewer -> Command Runner: retrieve logs`

Command Runner resolves `logs`, checks whether Designer Reviewer has `logs=allowed`, then executes only if the workflow and runtime also permit it.

If a command is absent or forbidden for the caller, Command Runner refuses rather than finding a workaround.

Command Runner does not grant its own broad command capability to callers. It is an enforcement/execution boundary, not a privilege-escalation mechanism.

Direct command invocation by an agent (for example Coder -> `source-control`) MUST apply the same command matrix and runtime authorization checks.

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

Connected Commands declares workflow relevance, while `team/command-matrix.csv` declares per-agent authorization.

## Concepts

**Workflow** = long-lived domain/activity.

**Strategy** = HOW the domain is approached.

**Flow** = bounded process inside workflow.

**Role** = reusable responsibility definition.

**Agent** = workflow/runtime realization of a role.

**Team** = relationships/ownership/communication/command authority among agents.

**Command** = bounded reusable action.

**Command Runner** = bounded command resolver/executor that enforces caller command policy; not an authority-escalation mechanism.

## Memory

Workflows declare memory semantics using [`_common/memory.md`](_common/memory.md). Strategic roles may own persistent memory; execution roles normally use session context unless explicitly specified.

## Runtime boundary

Workflow definitions remain provider independent. Runtime/profile maps abstract requirements to concrete models, commands, credentials and integrations and provides the final authorization gate.

## Minimum acceptance checklist

- [ ] required skeleton exists even when empty;
- [ ] `README.md`, `workflow.md`, `agents.md` exist;
- [ ] `team/README.md` exists;
- [ ] `team/capability-matrix.csv` exists;
- [ ] `team/communication-matrix.csv` exists;
- [ ] `team/command-matrix.csv` exists with canonical header even if empty;
- [ ] command access is fail-closed: missing means not allowed;
- [ ] important no-go commands are explicitly marked `forbidden` where useful;
- [ ] Prompt routing/use cases table exists;
- [ ] Connected commands table exists;
- [ ] memory/runtime/privacy boundaries are represented.