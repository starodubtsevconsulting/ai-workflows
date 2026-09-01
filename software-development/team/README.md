# Software Development Team

Compact shared context for all Software Development agents.

| Agent | Responsibility | Human-facing | Works with / delegates to |
| --- | --- | --- | --- |
| Strategist | Software Development strategy (HOW), durable domain continuity and workflow memory | Context-dependent | Workflow team and Global Governor when routed |
| Judge | Governance/rule changes and compliance enforcement | Yes — Human only | Human; live-test scenarios only for agent testing |
| Designer Reviewer | Design/architecture and implementation-conformance review | Yes | Human, Coder, Manager, Command Runner, `code-review` command |
| Coder | Bounded implementation | Yes | Designer Reviewer, Command Runner / allowed commands |
| Manager | Work/ticket coordination, estimation recording and staffing | No | Designer Reviewer, Strategist, Coder, `ticket-tracker` |
| Command Runner | Bounded command/tool execution with caller-policy enforcement | No | Authorized callers and AI Commands |

## Prompt / intent routing

Workflow-specific MCP-like routing layer. Reusable roles recognize intent; Team maps intent to concrete collaboration.

| Example prompt / intent | Entry agent | Workflow flow / route | Result |
| --- | --- | --- | --- |
| "Review this pull request: <url>" | Designer Reviewer | Pull request review flow | Review against ticket scope |
| "Create a task for this work" | Designer Reviewer / authorized caller | Ticket creation + estimation flow | Estimated actionable ticket |
|  |  |  |  |

### Ticket creation + estimation flow

Manager owns ticket creation/recording but is intentionally not assumed to have enough reasoning capacity to independently estimate underspecified software work.

The reasoning-capable caller should provide Manager with an **estimation packet** whenever possible:

- proposed scope/outcome;
- relevant implementation/design context;
- proposed decomposition/subtasks when useful;
- dependencies/uncertainties/assumptions;
- caller's best estimate (for example hours/points, according to project convention).

Typical route:

1. Designer Reviewer determines enough implementation scope to make the task actionable.
2. Designer Reviewer decomposes the work when decomposition requires higher reasoning capacity.
3. Designer Reviewer proposes its best estimate and sends scope + decomposition + assumptions + estimate to Manager.
4. Manager validates that the packet is sufficiently complete/coherent for ticket creation.
5. If estimate/context is missing or weak, Manager reports `BLOCKED` back to the caller and explicitly asks for the missing decomposition/context/best estimate rather than inventing precision.
6. When sufficient, Manager may sanity-check/normalize the estimate using project conventions and records it through the authorized ticket-tracking path.
7. Manager reports the created/updated ticket identifier and recorded estimate back to caller.

Example conversation shape:

`Designer Reviewer -> Manager: create task; scope=...; subtasks=...; estimate=~4h; assumptions=...`

`Manager -> Designer Reviewer: COPY`

`Manager -> ticket-tracker: create/update ticket with supplied scope/estimate`

`Manager -> Designer Reviewer: DONE; ticket=<id>; estimate=4h`

If the caller says only `create a task` without enough estimation information, Manager does not silently manufacture a detailed estimate. It asks the reasoning-capable caller for its best estimate/decomposition first.

### Pull request review flow

1. Designer Reviewer asks Command Runner for bounded pull-request metadata.
2. Workflow resolves/extracts ticket reference through configured capability path.
3. Designer Reviewer asks Manager for authoritative ticket scope.
4. Manager reports bounded scope back.
5. Designer Reviewer invokes `code-review` with PR URL + ticket reference + scope.
6. `code-review` returns bounded independent findings/evidence.
7. Designer Reviewer interprets findings against design intent and reports to Human.

## Team rules

- Respect responsibility boundaries; do not silently absorb another agent's ownership.
- Reusable roles know responsibility/intents; Team owns concrete peer/command orchestration.
- Higher-reasoning callers provide Manager with decomposition/estimation context when Manager is asked to create estimated software work.
- Manager owns recording/coordination and may challenge insufficient estimation input; it does not pretend confidence it does not have.
- Designer Reviewer owns design intent/conformance review; Coder owns implementation.
- Independent code review is `code-review`, not a standing Reviewer agent.
- Workflow Strategist owns Software Development HOW/durable domain continuity.
- Command Runner executes bounded operations and enforces caller command policy.
- Judge governs AI Workflow/AI Command rules and is directly invoked only by Human.

See [`capability-matrix.csv`](capability-matrix.csv), [`communication-matrix.csv`](communication-matrix.csv), and [`command-matrix.csv`](command-matrix.csv).