# Software Development Agents

Workflow-local Agent configuration. Software Development uses the [`standard`](../_common/team-templates/standard/README.md) Team Template.

## Agent properties

| Agent | Role | Intelligence | Reasoning | Context | Memory | Lifecycle | Clone after compactions | Clone at context utilization | Elastic pool | Min ready | Scheduled | Notes |
| --- | --- | --- | --- | --- | --- | --- | ---: | ---: | --- | ---: | --- | --- |
| Strategist | `Strategist` | highest-available | high | large | `WORKFLOW_STRATEGIC` + external workflow memory | persistent | 3 | 85% | disabled | 0 | no | Single strategic continuity participant. |
| Judge | `Judge` | high | high | large | `SESSION` | ephemeral | 3 | 85% | disabled | 0 | yes | Governance participant; not horizontally scaled by default. |
| Designer Reviewer | `Worker` | high | high | large | `SESSION` | ephemeral | 3 | 80% | disabled | 0 | no | Design/architecture and implementation-conformance Worker. |
| Coder | `Worker` | medium | medium | medium | `SESSION` | ephemeral | 1 | 75% | enabled | 0 | no | Additional Coders may be created on demand only for safely independent work. |
| Manager | `Manager` | medium | medium | medium | `SESSION` | ephemeral | 1 | 75% | disabled | 0 | yes | Coordinates work, context lifecycle and staffing. |
| Command Runner | `Worker` | low | low | small | `SESSION` | ephemeral | 2 | 75% | enabled | 1 | no | Elastic pool keeps one ready Runner; busy instances expose assignment such as `(deploy)` or `(logs)`. |
| UI Acceptance Tester | `Worker` | medium | medium | medium | `PROJECT` | persistent | 2 | 80% | disabled | 0 | no | Project-specific UI acceptance Worker. |

`Elastic pool` resolves the required `elastic-pool-enabled` Agent-instantiation property from [`role.spec.md`](../role.spec.md). `Min ready` is meaningful when pool is enabled.

## Elastic capacity

### Command Runner

`elastic-pool-enabled: true`

`elastic-pool-min-ready: 1`

One plain `Command Runner` remains ready. Busy Runners expose assignment labels. Additional capacity is created when required and excess capacity is reused/reset or archived after work settles.

### Coder

`elastic-pool-enabled: true`

`elastic-pool-min-ready: 0`

Coder is horizontally scalable, but additional instances are created only on demand because they are more expensive and carry richer context than Command Runner. Parallel Coder assignments MUST be safely independent/partitioned.

Example:

`Coder (feature-A)`

`Coder (feature-B)`

These are concurrent pool assignments, not replacement generations. Replacement cloning remains a separate lifecycle mechanism.

## Agent lifecycle authority

Common clone lifecycle is inherited from [`role.spec.md`](../role.spec.md). Role lifecycle authority is inherited from the `standard` Team Template.

## Capability implementation bindings

| Agent | Role capability | Implementation type | Implementation | Notes |
| --- | --- | --- | --- | --- |
| Designer Reviewer | independent implementation review | command | `code-review` | Separate review context. |
| Designer Reviewer | source-control evidence/operations | command | `source-control` | Routed according to Team policy. |
| Designer Reviewer | runtime diagnostics | command | `logs` | Normally routed through Command Runner. |
| Coder | source control | command | `source-control` | Normal implementation lifecycle capability. |
| Coder | runtime diagnostics | command | `logs` | Routed according to Team policy. |
| Coder | code/filesystem editing | harness | configured harness code/filesystem capability | Runtime/profile selects harness. |
| Manager | tracked-work/ticket management | command | `ticket-tracker` | Provider resolved from project/profile. |
| Manager | Agent lifecycle/context health | runtime | harness/runtime Agent inspection + lifecycle controls | Context monitoring, cloning, pool staffing, roster transition and archival. |
| Judge | governance source control | command | `source-control` | Applies Human-authored in-scope governance changes. |
| Command Runner | dynamic bounded command execution | runtime | registered AI Command catalog + caller policy | All pool instances use same authorization model. |
| Command Runner | deployment/pipeline execution | runtime/command | configured deployment provider/capability | Long-running deployment may occupy one pool instance while another remains ready. |
| UI Acceptance Tester | computer use / vision | command | `computer-use` | Harness/provider resolved at runtime. |
| UI Acceptance Tester | UI automation | project | project-configured acceptance automation | Project decides implementation. |

## Authorization reminder

`agents.md binding != permission`

Concrete capability/command executes only when Team/runtime authorization allows it.