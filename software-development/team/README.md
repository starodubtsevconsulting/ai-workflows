# Software Development Team

Compact shared context for all Software Development agents.

| Agent | Responsibility | Human-facing | Works with / delegates to |
| --- | --- | --- | --- |
| Strategist | Software Development strategy (HOW), durable domain continuity and workflow memory | Context-dependent | Workflow team and Global Governor when routed |
| Judge | Governance/rule changes and compliance enforcement | Yes — Human only | Human; live-test scenarios only for agent testing |
| Designer Reviewer | Design/architecture and implementation-conformance review | Yes | Human, Coder, Manager, Command Runner, `code-review` command |
| Coder | Bounded implementation | Yes | Designer Reviewer, Command Runner / allowed commands |
| Manager | Work/ticket coordination, estimate validation/recording and staffing | No | Designer Reviewer, Strategist, Coder, `ticket-tracker` |
| Command Runner | Bounded command/tool execution with caller-policy enforcement | No | Authorized callers and AI Commands |

## Prompt / intent routing

| Example prompt / intent | Entry agent | Workflow flow / route | Result |
| --- | --- | --- | --- |
| "Review this pull request: <url>" | Designer Reviewer | Pull request review flow | Review against ticket scope |
| "Create a task for this work" | Designer Reviewer / authorized caller | Ticket creation + estimation flow | Evidence-backed estimated ticket |
|  |  |  |  |

### Ticket creation + estimation flow

Manager owns ticket creation and estimate acceptance/recording. It is intentionally not assumed to have enough reasoning capacity to independently decompose underspecified software work.

An estimate is a **claim that requires supporting evidence**. A bare value such as `4h` is not sufficient merely because it came from a more intelligent agent.

The reasoning-capable caller provides an **estimation packet** containing enough evidence for Manager to evaluate whether the estimate is credible:

- proposed scope/outcome;
- concrete decomposition into implementation pieces/subtasks/checklist items;
- estimate per piece when useful, or enough sizing information to explain the total;
- dependencies, uncertainty and assumptions;
- caller's proposed total estimate according to project convention.

Example:

```text
Scope: add runtime roster validation
- [ ] load authoritative roster: ~1h
- [ ] validate sender identity before COPY: ~1h
- [ ] replacement/roster update path: ~1h
- [ ] tests: ~1h
Total proposed estimate: ~4h
Assumptions: existing runtime registry API is available
```

This is reviewable evidence. `Implement runtime roster validation — 4h` by itself is not.

Typical route:

1. Designer Reviewer determines actionable implementation scope.
2. Designer Reviewer decomposes the work into reviewable pieces/checklist items.
3. Designer Reviewer derives/proposes an estimate from that decomposition and states assumptions/uncertainty.
4. Designer Reviewer sends the estimation packet to Manager.
5. Manager evaluates whether the decomposition reasonably supports the proposed estimate.
6. Manager MAY accept the estimate, challenge it, or reject it as insufficiently supported.
7. If unsupported, Manager returns `BLOCKED` and asks the caller for decomposition/evidence or clarification. It does not record the estimate merely because the caller supplied a number.
8. If the evidence is coherent, Manager accepts or normalizes the estimate according to project conventions and records the ticket through the authorized ticket-tracking path.
9. Manager reports the ticket identifier and accepted estimate back to caller.

Example rejection:

`Designer Reviewer -> Manager: create task "Implement X"; estimate=4h`

`Manager -> Designer Reviewer: BLOCKED; estimate is unsupported. Provide decomposition/checklist and assumptions that explain the 4h estimate.`

Example acceptance:

`Designer Reviewer -> Manager: create task; scope=...; checklist=[...]; piece estimates=[...]; total=4h; assumptions=...`

`Manager -> Designer Reviewer: COPY`

`Manager -> ticket-tracker: create/update ticket with accepted scope/decomposition/estimate`

`Manager -> Designer Reviewer: DONE; ticket=<id>; accepted estimate=4h`

The goal is not false precision. Manager validates that there is a visible reasoning trail behind the estimate and that the ticket is decomposed enough to make progress/checking possible.

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
- Software estimates must be evidence-backed; a bare estimate is not sufficient.
- Reasoning-capable callers provide decomposition/checklist, assumptions and proposed estimate.
- Manager owns estimate acceptance/recording and may reject unsupported estimates.
- Manager does not manufacture decomposition when a more capable caller is responsible for supplying it.
- Designer Reviewer owns design intent/conformance review; Coder owns implementation.
- Independent code review is `code-review`, not a standing Reviewer agent.
- Workflow Strategist owns Software Development HOW/durable domain continuity.
- Command Runner executes bounded operations and enforces caller command policy.
- Judge governs AI Workflow/AI Command rules and is directly invoked only by Human.

See [`capability-matrix.csv`](capability-matrix.csv), [`communication-matrix.csv`](communication-matrix.csv), and [`command-matrix.csv`](command-matrix.csv).