# Software Development Team

Compact shared context for all Software Development agents.

| Agent | Responsibility | Human-facing | Works with / delegates to |
| --- | --- | --- | --- |
| Strategist | Software Development strategy (HOW), durable domain continuity and workflow memory | Context-dependent | Workflow team and Global Governor when routed |
| Judge | Governance/rule changes and compliance enforcement | Yes — Human only | Human; live-test scenarios only for agent testing |
| Designer Reviewer | Design/architecture and implementation-conformance review | Yes | Human, Coder, Manager, Command Runner, `code-review` command |
| Coder | Bounded implementation | Yes | Designer Reviewer, Command Runner / allowed commands |
| Manager | Reactive work/ticket coordination | No | Designer Reviewer, Strategist, Coder, `ticket-tracker` |
| Command Runner | Bounded command/tool execution with caller-policy enforcement | No | Authorized callers and AI Commands |

## Prompt / intent routing

This is the workflow-specific MCP-like routing layer. Reusable roles recognize intent; this team definition maps that intent to the concrete collaboration flow available in Software Development.

Every agent handling a role scenario marked `Workflow routing required = yes` MUST consult this section (and referenced flow/matrices) rather than inventing peers or commands from the reusable role definition.

| Example prompt / intent | Entry agent | Workflow flow / route | Result |
| --- | --- | --- | --- |
| "Review this pull request: <url>" | Designer Reviewer | Pull request review flow below | Review against ticket scope with bounded evidence |
|  |  |  |  |

### Pull request review flow

Human may provide only a pull-request URL. Designer Reviewer coordinates the workflow rather than directly fetching workflow-system metadata itself.

1. Designer Reviewer asks Command Runner for bounded pull-request metadata sufficient to identify the change, including title and relevant identifiers.
2. From that metadata, the workflow resolves/extracts the ticket reference using the configured ticket-tracking capability/command path.
3. Designer Reviewer asks Manager for the authoritative ticket scope using that ticket reference. Manager uses its allowed `ticket-tracker` capability.
4. Manager reports the bounded ticket scope back to Designer Reviewer under the common `COPY -> REPORT BACK` protocol.
5. Designer Reviewer invokes the `code-review` command with the pull-request URL, ticket reference and resolved ticket scope.
6. `code-review` performs the independent review under its own review rules/context and returns bounded findings/evidence.
7. Designer Reviewer interprets the findings against design intent and reports the review outcome to Human.

The route is governed by capability, communication and command matrices. If required context cannot be resolved, the responsible step returns `BLOCKED` rather than bypassing the route or guessing.

## Team rules

- Respect responsibility boundaries; do not silently absorb another agent's ownership.
- Reusable roles know their responsibility/intents; workflow Team owns concrete peer/command orchestration.
- Designer Reviewer owns design intent and conformance review; Coder owns implementation.
- Independent code review is a `code-review` command using separate review context/rules, not a standing Reviewer agent.
- Workflow Strategist owns Software Development HOW and durable development-domain continuity.
- Manager coordinates bounded work/tickets and does not own workflow strategy.
- Command Runner executes bounded operations and enforces caller command policy.
- Judge governs AI Workflow/AI Command rules and is directly invoked only by Human.

See [`capability-matrix.csv`](capability-matrix.csv), [`communication-matrix.csv`](communication-matrix.csv), and [`command-matrix.csv`](command-matrix.csv).