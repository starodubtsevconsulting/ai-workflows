# Software Development Agents

Workflow-local realizations of reusable roles. Values are provider-independent defaults/hints; runtime infrastructure maps them to concrete models, capabilities and schedules.

| Agent | Role | Human-facing override | Intelligence | Reasoning | Context | Memory | Lifecycle | Scheduled | Schedule intent | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Strategist | [`Workflow Strategist`](../_common/roles/workflow/strategist.md) |  | highest-available | high | large | `WORKFLOW_STRATEGIC` + external workflow memory | persistent | no | — | Owns Software Development HOW, durable domain decisions and continuity. |
| Judge | [`Judge`](../_common/roles/judge.md) | true | high | high | large | `SESSION` | ephemeral | yes | Periodically audit agent/workflow activity for rule compliance, authority abuse and governance violations. | Human-only direct governance surface; scheduled audits are periodic/sampled rather than continuous surveillance. |
| Designer Reviewer | [`Designer Reviewer`](../_common/roles/designer-reviewer.md) |  | high | high | large | `SESSION` | ephemeral | no | — | Designs and performs design-conformance review; independent review uses `code-review` command. |
| Coder | [`Coder`](../_common/roles/coder.md) |  | medium | medium | medium | `SESSION` | ephemeral | no | — | Bounded implementation worker instantiated for assigned work. |
| Manager | [`Manager`](../_common/roles/manager.md) |  | medium | medium | medium | `SESSION` | ephemeral | yes | Periodically inspect ticket/work state and perform or surface allowed coordination actions. | Reactive to workflow agents when invoked, plus periodic operational checks. |
| Command Runner | [`Command Runner`](../_common/roles/command-runner.md) |  | low | low | small | `SESSION` | ephemeral | no | — | Bounded command execution invoked by authorized callers. |