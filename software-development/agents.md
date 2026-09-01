# Software Development Agents

Workflow-local realizations of reusable roles. Values are provider-independent defaults/hints; runtime infrastructure maps them to concrete models and capabilities.

| Agent | Role | Human-facing override | Intelligence | Reasoning | Context | Memory | Lifecycle | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Strategist | [`Workflow Strategist`](../_common/roles/workflow/strategist.md) |  | highest-available | high | largest-available | `WORKFLOW_STRATEGIC` + external memory | persistent | Preserves development strategy, decisions and continuity across sessions; controls disclosure of strategic memory to execution agents according to policy/Human authorization. |
| Judge | [`Judge`](../_common/roles/judge.md) | true | high | high | large | `SESSION` | ephemeral | Human-only governance surface for changing rules and checking rule compliance. Other agents cannot directly invoke Judge. |
| Designer Reviewer | [`Designer Reviewer`](../_common/roles/designer-reviewer.md) |  | high | high | large | `SESSION` | ephemeral | Designs and performs design-conformance review. No external memory by default; may request strategic context through the Strategist when needed. |
| Coder | [`Coder`](../_common/roles/coder.md) |  | medium | medium | medium | `SESSION` | ephemeral | Bounded implementation worker; intended to be replaceable frequently between tasks/iterations. |
| Reviewer | [`Reviewer`](../_common/roles/reviewer.md) |  | high | high | medium | `SESSION` | ephemeral | Independent correctness/quality/risk review, separate from Designer Reviewer's design-conformance review. |
| Manager | [`Manager`](../_common/roles/manager.md) |  | medium | medium | medium | `SESSION` | ephemeral | Reactive coordination/ticket responsibility; normally behind routing. |
| Command Runner | [`Command Runner`](../_common/roles/command-runner.md) |  | low | low | small | `SESSION` | ephemeral | Bounded tool/command execution; not a reasoning-heavy conversational role. |