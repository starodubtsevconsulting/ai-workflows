# Software Development Agents

Workflow-local realizations of reusable roles. Values are provider-independent defaults/hints; runtime infrastructure maps them to concrete models and capabilities.

| Agent | Role | Human-facing override | Intelligence | Reasoning | Context | Memory | Lifecycle | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Strategist | [`Workflow Strategist`](../_common/roles/workflow/strategist.md) |  | highest-available | high | large | `WORKFLOW_STRATEGIC` + external workflow memory | persistent | Owns Software Development HOW: development strategy, durable domain decisions and continuity. Its tools/commands are workflow-specific. It does not inherit Global Governor access to Human life context, calendar, communications or unrelated workflows. |
| Judge | [`Judge`](../_common/roles/judge.md) | true | high | high | large | `SESSION` | ephemeral | Human-only governance surface for changing rules and checking rule compliance. Other agents cannot directly invoke Judge. |
| Designer Reviewer | [`Designer Reviewer`](../_common/roles/designer-reviewer.md) |  | high | high | large | `SESSION` | ephemeral | Designs and performs design-conformance review. Independent review is invoked through the `code-review` AI Command rather than a separate Reviewer agent. |
| Coder | [`Coder`](../_common/roles/coder.md) |  | medium | medium | medium | `SESSION` | ephemeral | Bounded implementation worker; intended to be replaceable frequently between tasks/iterations. |
| Manager | [`Manager`](../_common/roles/manager.md) |  | medium | medium | medium | `SESSION` | ephemeral | Reactive coordination/ticket responsibility; normally behind routing. |
| Command Runner | [`Command Runner`](../_common/roles/command-runner.md) |  | low | low | small | `SESSION` | ephemeral | Bounded tool/command execution; not a reasoning-heavy conversational role. |