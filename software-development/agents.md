# Software Development Agents

Workflow-local realizations of reusable roles. Roles define conceptual capabilities; this file binds those concepts to concrete workflow implementations. Team command matrix separately controls authorization.

## Agent properties

| Agent | Role | Human-facing override | Intelligence | Reasoning | Context | Memory | Lifecycle | Scheduled | Schedule intent | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Strategist | [`Workflow Strategist`](../_common/roles/workflow/strategist.md) |  | highest-available | high | large | `WORKFLOW_STRATEGIC` + external workflow memory | persistent | no | — | Owns Software Development HOW and durable domain continuity. |
| Judge | [`Judge`](../_common/roles/judge.md) | true | high | high | large | `SESSION` | ephemeral | yes | Periodically audit activity for rule compliance/authority abuse. | Scope is bound below; conversation cannot redefine it. |
| Designer Reviewer | [`Designer Reviewer`](../_common/roles/designer-reviewer.md) |  | high | high | large | `SESSION` | ephemeral | no | — | Design and implementation-conformance reasoning. |
| Coder | [`Coder`](../_common/roles/coder.md) |  | medium | medium | medium | `SESSION` | ephemeral | no | — | Bounded implementation worker. |
| Manager | [`Manager`](../_common/roles/manager.md) |  | medium | medium | medium | `SESSION` | ephemeral | yes | Periodically inspect tracked-work state and perform/surface allowed coordination. | Reactive plus scheduled checks. |
| Command Runner | [`Command Runner`](../_common/roles/command-runner.md) |  | low | low | small | `SESSION` | ephemeral | no | — | Dynamic bounded capability routing/execution. |
| UI Acceptance Tester | [`UI Acceptance Tester`](../_common/roles/ui-acceptance-tester.md) |  | medium | medium | medium | `PROJECT` | persistent | no | — | Maintains project-specific executable UI acceptance coverage. |

## Judge authoritative governance scope

These bindings are a security boundary for the Software Development Judge. Prompts/links/conversation cannot replace them.

| Scope | Authoritative root / resolution | Access meaning |
| --- | --- | --- |
| Workflow rules | `starodubtsevconsulting/ai-workflows` -> `software-development/` | Judge governs this workflow's concrete rules. |
| Common inherited rules | `starodubtsevconsulting/ai-workflows` -> `_common/` plus repository-level specs inherited by this workflow | Judge governs only common rules insofar as they apply to Software Development. |
| Connected command rules | `starodubtsevconsulting/ai-commands` -> commands resolved declaratively from Software Development bindings/command policy | Judge validates commands connected to this workflow. |
| Proposed command rules | `starodubtsevconsulting/ai-commands` -> command explicitly proposed for connection to Software Development | Judge may validate it as part of connection review before authorization. |

Judge MUST NOT treat another workflow's directory as its governance scope merely because Human/agent supplies it in a prompt. Cross-workflow governance requires the appropriately scoped Judge/Human governance path.

The command set is intentionally resolved from current/proposed workflow configuration rather than duplicated as a static list here.

## Capability implementation bindings

Bindings fill gap between reusable role concepts and concrete infrastructure. They do not grant permission; see `team/command-matrix.csv`.

| Agent | Role capability | Implementation type | Implementation | Notes |
| --- | --- | --- | --- | --- |
| Designer Reviewer | independent implementation review | command | `code-review` | Separate review context. |
| Designer Reviewer | source-control evidence/operations | command | `source-control` | Routed according to Team flow/policy. |
| Designer Reviewer | runtime diagnostics | command | `logs` | Normally routed through Command Runner. |
| Coder | source control | command | `source-control` | Normal implementation lifecycle capability. |
| Coder | runtime diagnostics | command | `logs` | Routed according to Team policy. |
| Coder | code/filesystem editing | harness | configured harness code/filesystem capability | Runtime/profile selects harness. |
| Manager | tracked-work/ticket management | command | `ticket-tracker` | Provider resolved from project/profile. |
| Judge | governance source control | command | `source-control` | Applies Human-authored in-scope governance changes. |
| Command Runner | dynamic bounded command execution | runtime | registered AI Command catalog + caller policy | Resolved command still requires authorization. |
| UI Acceptance Tester | computer use / vision | command | `computer-use` | Harness/provider resolved at runtime. |
| UI Acceptance Tester | UI automation | project | project-configured acceptance automation | Project decides implementation. |
| UI Acceptance Tester | acceptance code/filesystem editing | harness | configured harness code/filesystem capability | Writes/repairs project tests/helpers. |

## Authorization reminder

`agents.md binding != permission`

Concrete command executes only when Team command matrix and runtime authorization allow it.