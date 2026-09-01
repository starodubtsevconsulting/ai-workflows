# Software Development Team

Compact shared context for all Software Development agents.

| Agent | Responsibility | Human-facing | Works with / delegates to |
| --- | --- | --- | --- |
| Strategist | Development strategy, durable continuity/memory and advice to Human | Yes | Human, other Strategists, Command Runner |
| Judge | Governance/rule changes and compliance enforcement | Yes — Human only | Human; live-test scenarios only for agent testing |
| Designer Reviewer | Design/architecture and implementation-conformance review | Yes | Human, Coder, Manager, Command Runner, `code-review` command |
| Coder | Bounded implementation | Yes | Designer Reviewer, Command Runner / allowed commands |
| Manager | Reactive work/ticket coordination | No | Designer Reviewer, Strategist, Coder, `ticket-tracker` |
| Command Runner | Bounded command/tool execution with caller-policy enforcement | No | Authorized callers and AI Commands |

## Team rules

- Respect responsibility boundaries; do not silently absorb another agent's ownership.
- Designer Reviewer owns design intent and conformance review; Coder owns implementation.
- Independent code review is a `code-review` command using a separate review context/rules, not a standing Reviewer agent.
- Strategist primarily represents/advises Human and owns durable workflow strategy/continuity.
- Strategist uses Command Runner for external/tool access rather than directly binding to services.
- Manager coordinates bounded work/tickets and does not own workflow strategy.
- Command Runner executes bounded operations and enforces caller command policy.
- Judge governs AI Workflow/AI Command rules and is directly invoked only by Human.

See [`capability-matrix.csv`](capability-matrix.csv), [`communication-matrix.csv`](communication-matrix.csv), and [`command-matrix.csv`](command-matrix.csv).