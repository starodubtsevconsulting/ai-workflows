# Software Development Agents

Workflow-local realizations of reusable roles. Roles define conceptual capabilities; this file binds those concepts to concrete workflow implementations. Team command matrix separately controls authorization.

## Agent properties

| Agent | Role | Human-facing override | Intelligence | Reasoning | Context | Memory | Lifecycle | Scheduled | Schedule intent | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Strategist | [`Workflow Strategist`](../_common/roles/workflow/strategist.md) |  | highest-available | high | large | `WORKFLOW_STRATEGIC` + external workflow memory | persistent | no | — | Owns Software Development HOW and durable domain continuity. |
| Judge | [`Judge`](../_common/roles/judge.md) | true | high | high | large | `SESSION` | ephemeral | yes | Periodically audit activity for rule compliance/authority abuse. | Human-only direct governance surface. |
| Designer Reviewer | [`Designer Reviewer`](../_common/roles/designer-reviewer.md) |  | high | high | large | `SESSION` | ephemeral | no | — | Design and implementation-conformance reasoning. |
| Coder | [`Coder`](../_common/roles/coder.md) |  | medium | medium | medium | `SESSION` | ephemeral | no | — | Bounded implementation worker. |
| Manager | [`Manager`](../_common/roles/manager.md) |  | medium | medium | medium | `SESSION` | ephemeral | yes | Periodically inspect tracked-work state and perform/surface allowed coordination. | Reactive plus scheduled operational checks. |
| Command Runner | [`Command Runner`](../_common/roles/command-runner.md) |  | low | low | small | `SESSION` | ephemeral | no | — | Dynamic bounded capability routing/execution for authorized callers. |
| UI Acceptance Tester | [`UI Acceptance Tester`](../_common/roles/ui-acceptance-tester.md) |  | medium | medium | medium | `PROJECT` | persistent | no | — | Learns/maintains project-specific executable UI acceptance coverage. |

## Capability implementation bindings

Bindings fill the gap between reusable role concepts and this workflow's concrete infrastructure. They do not grant permission; see `team/command-matrix.csv`.

| Agent | Role capability | Implementation type | Implementation | Notes |
| --- | --- | --- | --- | --- |
| Designer Reviewer | independent implementation review | command | `code-review` | Separate review context; workflow route decides when used. |
| Designer Reviewer | source-control evidence/operations | command | `source-control` | Direct or dynamically routed according to Team flow/policy. |
| Designer Reviewer | runtime diagnostics | command | `logs` | Normally dynamically routed through Command Runner. |
| Coder | source control | command | `source-control` | Normal implementation lifecycle capability. |
| Coder | runtime diagnostics | command | `logs` | Routed according to Team policy to protect context. |
| Coder | code/filesystem editing | harness | configured harness code/filesystem capability | Concrete harness selected by runtime/profile. |
| Manager | tracked-work/ticket management | command | `ticket-tracker` | Provider resolved from project/profile configuration. |
| Judge | governance source control | command | `source-control` | Applies Human-authored governance changes through authorized path. |
| Command Runner | dynamic bounded command execution | runtime | registered AI Command catalog + caller policy | Resolved command must still be authorized for caller. |
| UI Acceptance Tester | computer use / vision | command | `computer-use` | Harness/provider resolved at runtime. |
| UI Acceptance Tester | UI automation | project | project-configured acceptance automation | For web/Node this may be Playwright; project decides. |
| UI Acceptance Tester | acceptance code/filesystem editing | harness | configured harness code/filesystem capability | Writes/repairs project-owned tests/helpers. |

## Authorization reminder

`agents.md binding != permission`

A concrete command is executable only when the Team command matrix and runtime authorization allow it.