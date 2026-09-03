# Software Development Agents

Workflow-local Agent configuration. The selected Team Template provides reusable Roles/authority; this file names and configures concrete Agents that fulfill those Roles in Software Development. Team command matrix separately controls command authorization.

## Team template

Software Development uses [`standard`](../_common/team-templates/standard/README.md).

| Agent name | Role |
| --- | --- |
| Strategist | `Strategist` |
| Judge | `Judge` |
| Designer Reviewer | `Worker` |
| Coder | `Worker` |
| Manager | `Manager` |
| Command Runner | `Worker` |
| UI Acceptance Tester | `Worker` |
| Admin (when present) | `Admin` |

## Agent properties

| Agent | Role | Human-facing override | Intelligence | Reasoning | Context | Memory | Lifecycle | Clone after compactions | Clone at context utilization | Scheduled | Schedule intent | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | ---: | ---: | --- | --- | --- |
| Strategist | `Strategist` |  | highest-available | high | large | `WORKFLOW_STRATEGIC` + external workflow memory | persistent | 3 | 85% | no | — | Owns Software Development HOW and durable domain continuity. |
| Judge | `Judge` | true | high | high | large | `SESSION` | ephemeral | 3 | 85% | yes | Periodically sample activity for rule compliance/authority abuse. | Scope is bound below. |
| Designer Reviewer | `Worker` |  | high | high | large | `SESSION` | ephemeral | 3 | 80% | no | — | Design/architecture and implementation-conformance Worker. |
| Coder | `Worker` |  | medium | medium | medium | `SESSION` | ephemeral | 1 | 75% | no | — | Bounded software implementation Worker. |
| Manager | `Manager` |  | medium | medium | medium | `SESSION` | ephemeral | 1 | 75% | yes | Periodically inspect tracked-work and Agent context/lifecycle state. | Lifecycle authority inherited from standard template. |
| Command Runner | `Worker` |  | low | low | small | `SESSION` | ephemeral | 2 | 75% | no | — | One normal instance for routine commands; temporary copies may be created for slow/long-running bounded operations. |
| UI Acceptance Tester | `Worker` |  | medium | medium | medium | `PROJECT` | persistent | 2 | 80% | no | — | Project-specific UI acceptance Worker. |

## Command Runner concurrency

Software Development normally keeps one Command Runner available for routine bounded command execution.

When a command is expected to occupy the normal Runner for a material period, Manager/runtime may create a temporary Command Runner copy on demand, give it only the contextual knowledge and bindings required for that operation, and archive it after completion.

Deployment/pipeline execution is the primary current example:

`deployment requested -> temporary Command Runner -> source-control/deployment capability -> wait/report -> archive temporary Runner`

This preserves availability of the normal Command Runner for unrelated operations such as logs or source-control queries.

Temporary capacity is not replacement cloning. Replacement cloning increments the generation of an existing lineage; temporary copies are additional concurrent Workers created for bounded work.

## Agent lifecycle authority

Common clone lifecycle is inherited from [`role.spec.md`](../role.spec.md). Role lifecycle authority is inherited from the `standard` Team Template.

## Capability implementation bindings

Bindings do not grant permission; see `team/command-matrix.csv`.

| Agent | Role capability | Implementation type | Implementation | Notes |
| --- | --- | --- | --- | --- |
| Designer Reviewer | independent implementation review | command | `code-review` | Separate review context. |
| Designer Reviewer | source-control evidence/operations | command | `source-control` | Routed according to Team policy. |
| Designer Reviewer | runtime diagnostics | command | `logs` | Normally routed through Command Runner. |
| Coder | source control | command | `source-control` | Normal implementation lifecycle capability. |
| Coder | runtime diagnostics | command | `logs` | Routed according to Team policy. |
| Coder | code/filesystem editing | harness | configured harness code/filesystem capability | Runtime/profile selects harness. |
| Manager | tracked-work/ticket management | command | `ticket-tracker` | Provider resolved from project/profile. |
| Manager | Agent lifecycle/context health | runtime | harness/runtime Agent inspection + lifecycle controls | Context monitoring, cloning, roster transition and archival. |
| Judge | governance source control | command | `source-control` | Applies Human-authored in-scope governance changes. |
| Command Runner | dynamic bounded command execution | runtime | registered AI Command catalog + caller policy | Normal and temporary copies use same authorization model. |
| Command Runner | deployment/pipeline execution | runtime/command | configured deployment provider/capability | Prefer temporary copy when execution/wait would monopolize normal Runner. |
| UI Acceptance Tester | computer use / vision | command | `computer-use` | Harness/provider resolved at runtime. |
| UI Acceptance Tester | UI automation | project | project-configured acceptance automation | Project decides implementation. |

## Authorization reminder

`agents.md binding != permission`

Concrete capability/command executes only when Team/runtime authorization allows it.