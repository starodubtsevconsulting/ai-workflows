# Software Development Team

Compact shared context for all Software Development agents.

| Agent | Responsibility | Human-facing | Works with / delegates to |
| --- | --- | --- | --- |
| Strategist | Software Development strategy (HOW), durable domain continuity and workflow memory | Context-dependent | Workflow team and Global Governor when routed |
| Judge | Governance/rule changes and compliance enforcement | Yes — Human only | Human; live-test scenarios only for agent testing |
| Designer Reviewer | Design/architecture and implementation-conformance review | Yes | Human, Coder, Manager, Command Runner, `code-review` command |
| Coder | Bounded implementation | Yes | Designer Reviewer, Command Runner / allowed commands |
| Manager | Ticket/work coordination, estimate validation/recording and staffing | No | Designer Reviewer, Strategist, Coder, `ticket-tracker` |
| Command Runner | Bounded command/tool execution with caller-policy enforcement | No | Authorized callers and AI Commands |

## Terminology

**Ticket** is the canonical Software Development workflow term for a tracked unit of work managed through Manager/ticket tracker.

Natural-language callers may say `task`, `issue`, `work item`, or similar wording. Manager may interpret those as **ticket** when context clearly means tracked software work. This synonym handling is semantic convenience only; workflow documentation, routes and Manager responses SHOULD use `ticket`.

If `task` could instead mean an agent assignment/delegation or another non-ticket concept, Manager MUST NOT silently reinterpret it as a ticket; it asks for clarification.

## Prompt / intent routing

| Example prompt / intent | Entry agent | Workflow flow / route | Result |
| --- | --- | --- | --- |
| "Review this pull request: <url>" | Designer Reviewer | Pull request review flow | Review against ticket scope |
| "Create a ticket for this work" | Designer Reviewer / authorized caller | Ticket creation + estimation flow | Evidence-backed estimated ticket |
| "Create a task for this work" | Designer Reviewer / authorized caller | Synonym -> ticket when context is unambiguous | Evidence-backed estimated ticket |
|  |  |  |  |

### Ticket creation + estimation flow

Manager owns ticket creation and estimate acceptance/recording. It is intentionally not assumed to have enough reasoning capacity to independently decompose underspecified software work.

An estimate is a **claim that requires supporting evidence**. A bare value such as `4h` is not sufficient merely because it came from a more intelligent agent.

The reasoning-capable caller provides an **estimation packet** containing:

- proposed scope/outcome;
- concrete decomposition into implementation pieces/subtasks/checklist items;
- estimate per piece when useful, or enough sizing information to explain total;
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

`Implement runtime roster validation — 4h` alone is not reviewable evidence.

Typical route:

1. Designer Reviewer determines actionable implementation scope.
2. Designer Reviewer decomposes work into reviewable pieces/checklist items.
3. Designer Reviewer derives/proposes estimate and states assumptions/uncertainty.
4. Designer Reviewer sends estimation packet to Manager.
5. Manager evaluates whether decomposition reasonably supports estimate.
6. Manager MAY accept, challenge or reject estimate as insufficiently supported.
7. If unsupported, Manager returns `BLOCKED` and requests decomposition/evidence/clarification.
8. If coherent, Manager accepts/normalizes estimate and records ticket through authorized ticket-tracking path.
9. Manager reports ticket identifier and accepted estimate to caller.

Example rejection:

`Designer Reviewer -> Manager: create ticket "Implement X"; estimate=4h`

`Manager -> Designer Reviewer: BLOCKED; estimate is unsupported. Provide decomposition/checklist and assumptions that explain the 4h estimate.`

Example acceptance:

`Designer Reviewer -> Manager: create ticket; scope=...; checklist=[...]; piece estimates=[...]; total=4h; assumptions=...`

`Manager -> Designer Reviewer: COPY`

`Manager -> ticket-tracker: create/update ticket with accepted scope/decomposition/estimate`

`Manager -> Designer Reviewer: DONE; ticket=<id>; accepted estimate=4h`

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
- Use **ticket** as canonical terminology for tracked software work.
- Treat `task`/`issue`/`work item` as ticket synonyms only when context is unambiguous.
- Software estimates must be evidence-backed; a bare estimate is insufficient.
- Reasoning-capable callers provide decomposition/checklist, assumptions and proposed estimate.
- Manager owns estimate acceptance/recording and may reject unsupported estimates.
- Designer Reviewer owns design intent/conformance review; Coder owns implementation.
- Independent code review is `code-review`, not a standing Reviewer agent.
- Workflow Strategist owns Software Development HOW/durable domain continuity.
- Command Runner executes bounded operations and enforces caller command policy.
- Judge governs AI Workflow/AI Command rules and is directly invoked only by Human.

See [`capability-matrix.csv`](capability-matrix.csv), [`communication-matrix.csv`](communication-matrix.csv), and [`command-matrix.csv`](command-matrix.csv).