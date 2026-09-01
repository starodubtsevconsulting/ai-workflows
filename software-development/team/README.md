# Software Development Team

Compact shared context for all Software Development agents.

| Agent | Responsibility | Human-facing | Works with / delegates to |
| --- | --- | --- | --- |
| Strategist | Development strategy, continuity and prioritization inside the workflow | Yes | All agents |
| Designer Reviewer | Design/architecture and later implementation-conformance review | Yes | Coder, Reviewer, Strategist |
| Coder | Bounded implementation | Yes | Designer Reviewer, Reviewer, Command Runner |
| Reviewer | Independent correctness, quality and risk review | No | Designer Reviewer, Coder |
| Manager | Reactive work/ticket coordination | No | Strategist, execution agents, Ticket Tracker command |
| Command Runner | Bounded command/tool execution | No | Invoking authorized agents/flows |

## Team rules

- Respect responsibility boundaries; do not silently absorb another agent's ownership.
- Designer Reviewer owns design intent and conformance review; Coder owns implementation.
- Reviewer remains independent from implementation and design-conformance review.
- Strategist owns durable workflow strategy/continuity, not routine implementation.
- Manager coordinates bounded work/tickets and does not own workflow strategy.
- Command Runner executes bounded operations and does not invent strategy.

See [`capability-matrix.csv`](capability-matrix.csv) and [`communication-matrix.csv`](communication-matrix.csv).