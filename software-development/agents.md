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
| Deployer | `Worker` |
| Manager | `Manager` |
| Command Runner | `Worker` |
| UI Acceptance Tester | `Worker` |
| Admin (when present) | `Admin` |

`Deployer` uses the reusable [`Deployer Agent definition`](../_common/roles/deployer.md) while fulfilling the common `Worker` Role.

## Agent properties

| Agent | Role | Human-facing override | Intelligence | Reasoning | Context | Memory | Lifecycle | Clone after compactions | Clone at context utilization | Scheduled | Schedule intent | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | ---: | ---: | --- | --- | --- |
| Strategist | `Strategist` |  | highest-available | high | large | `WORKFLOW_STRATEGIC` + external workflow memory | persistent | 3 | 85% | no | — | Owns Software Development HOW and durable domain continuity. |
| Judge | `Judge` | true | high | high | large | `SESSION` | ephemeral | 3 | 85% | yes | Periodically sample activity for rule compliance/authority abuse. | Scope is bound below. |
| Designer Reviewer | `Worker` |  | high | high | large | `SESSION` | ephemeral | 3 | 80% | no | — | Design/architecture and implementation-conformance Worker. |
| Coder | `Worker` |  | medium | medium | medium | `SESSION` | ephemeral | 1 | 75% | no | — | Bounded software implementation Worker. |
| Deployer | `Worker` | false | low | low | small | `SESSION` | ephemeral | 2 | 75% | no | — | Mechanical deployment/pipeline execution; concrete provider/command is bound per workflow/runtime. |
| Manager | `Manager` |  | medium | medium | medium | `SESSION` | ephemeral | 1 | 75% | yes | Periodically inspect tracked-work and Agent context/lifecycle state. | Lifecycle authority inherited from standard template. |
| Command Runner | `Worker` |  | low | low | small | `SESSION` | ephemeral | 2 | 75% | no | — | Dynamic bounded capability-routing/execution Worker. |
| UI Acceptance Tester | `Worker` |  | medium | medium | medium | `PROJECT` | persistent | 2 | 80% | no | — | Project-specific UI acceptance Worker. |

Proactive cloning uses configured context-health conditions according to [`role.spec.md`](../role.spec.md).

## Agent lifecycle authority

Common clone lifecycle is inherited from [`role.spec.md`](../role.spec.md). Role lifecycle authority is inherited from the `standard` Team Template. Software Development currently defines no lifecycle-authority override.

## Judge authoritative governance scope

| Scope | Authoritative root / resolution | Access meaning |
| --- | --- | --- |
| Workflow rules | `starodubtsevconsulting/ai-workflows` -> `software-development/` | Judge governs this workflow's concrete rules. |
| Common inherited rules | `starodubtsevconsulting/ai-workflows` -> `_common/` plus repository-level specs inherited by this workflow | Judge governs common rules insofar as they apply here. |
| Connected command rules | `starodubtsevconsulting/ai-commands` -> commands resolved from workflow bindings/policy | Judge validates commands connected to this workflow. |
| Proposed command rules | `starodubtsevconsulting/ai-commands` -> command proposed for connection | Judge may validate before authorization. |

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
| Deployer | deployment/pipeline execution | runtime/command | configured deployment provider/capability | May resolve to GitHub, Bitbucket, GitLab, cloud pipeline or another authorized implementation. |
| Deployer | deployment/pipeline observation | runtime/command | configured deployment provider/capability | Waits for and reports bounded deployment status/evidence. |
| Manager | tracked-work/ticket management | command | `ticket-tracker` | Provider resolved from project/profile. |
| Manager | Agent lifecycle/context health | runtime | harness/runtime Agent inspection + lifecycle controls | Context monitoring, cloning, roster transition and archival. |
| Judge | governance source control | command | `source-control` | Applies Human-authored in-scope governance changes. |
| Command Runner | dynamic bounded command execution | runtime | registered AI Command catalog + caller policy | Resolved command still requires authorization. |
| UI Acceptance Tester | computer use / vision | command | `computer-use` | Harness/provider resolved at runtime. |
| UI Acceptance Tester | UI automation | project | project-configured acceptance automation | Project decides implementation. |
| UI Acceptance Tester | acceptance code/filesystem editing | harness | configured harness code/filesystem capability | Writes/repairs project tests/helpers. |

## Authorization reminder

`agents.md binding != permission`

Concrete capability/command executes only when Team/runtime authorization allows it.