# Agent Communication Protocol

Common protocol for agent-to-agent communication across workflows.

## Identity envelope

Every agent-to-agent message MUST identify the sender and execution context so the receiver does not have to infer who is speaking or which work context applies.

Required fields:

- `agent_id` — stable/runtime identifier of the sending agent instance;
- `agent_name` — human-readable agent name;
- `profile` — profile/environment under which the agent operates;
- `workflow` — active workflow;
- `project` — active project when applicable.

Contextual fields SHOULD be included when applicable, for example:

- `branch`;
- `task_id` / ticket reference;
- `flow`;
- `session_id`;
- `correlation_id` / parent message id;
- repository/workspace identifier;
- other domain-specific execution context.

Conceptual envelope:

```text
From:
  agent_id: <id>
  agent_name: <name>
  profile: <profile>
  workflow: <workflow>
  project: <project-or-null>
  branch: <branch-if-applicable>
  task_id: <task-if-applicable>

Message:
  <request/context>
```

## COPY protocol

Agent delegation/requests follow a **COPY -> work -> REPORT BACK** protocol.

When Agent A sends work/request to Agent B:

1. B validates that it understands the sender/context/request sufficiently to accept responsibility.
2. B replies promptly with `COPY` when accepted.
3. `COPY` means: **I received and understood this request, I accept responsibility for this delegated work, and I will report back to you.**
4. After `COPY`, A does not need to continuously watch/poll B merely to know whether B finished.
5. B MUST report back to A when:
   - work is completed;
   - B is blocked and needs information/action;
   - B cannot continue or must refuse;
   - a material condition changes such that A needs to know.

`COPY` is therefore not merely an acknowledgement of transport receipt. It is an explicit responsibility/report-back contract.

If B does not understand or cannot accept the request, it MUST NOT respond `COPY`; it should instead return the clarification/refusal/blocking reason.

## Report-back envelope

A report back SHOULD preserve the original correlation/task context and identify status, for example:

- `DONE` — delegated responsibility completed;
- `BLOCKED` — cannot proceed without caller/other action;
- `REFUSED` — request is outside authority/policy;
- `FAILED` — attempted work failed;
- `UPDATE` — material intermediate state when necessary.

The receiver MUST send the report to the delegating/calling agent unless the delegation explicitly establishes another return target.

## No polling assumption

Once `COPY` is received, the caller MAY become inactive and rely on report-back. Runtime implementations SHOULD therefore preserve routing/correlation information needed to wake/notify the caller when B reports back.

This protocol is intended to support asynchronous and long-running agent work without requiring the delegator to consume tokens/resources by watching continuously.

## Authority boundary

Communication does not grant authority. B still follows its role, workflow capability matrix, communication matrix, command matrix and runtime authorization. `COPY` MUST NOT be used for work B is not allowed to accept.

## Human communication

The protocol is mandatory for agent-to-agent delegation. Human-facing conversation may use natural interaction and does not need literal `COPY` acknowledgements unless the runtime/workflow chooses to expose them.