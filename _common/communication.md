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

Contextual fields SHOULD be included when applicable: `branch`, `task_id`, `flow`, `session_id`, `correlation_id`, repository/workspace identifier, and other domain-specific context.

## Receiver-side authorization

When Agent A calls/delegates to Agent B, **B is responsible for validating that A is allowed to communicate/delegate to B before accepting the request**.

B checks the effective workflow `team/communication-matrix.csv` (plus any runtime/profile restrictions) using the sender identity/context from the message envelope.

Rules:

- explicit allowed A -> B relationship: B may continue evaluating/accepting the request;
- explicit forbidden A -> B relationship: B MUST refuse;
- missing A -> B relationship: not granted by default, therefore B MUST refuse;
- B MUST perform this check before returning `COPY`;
- B MUST NOT ask Judge for routine authorization.

Judge is not in the normal communication critical path. Judge may later inspect communication/history during scheduled governance audits and identify attempted bypasses, abuse or policy failures.

An unauthorized communication attempt SHOULD be recorded/auditable so Judge can inspect it later.

Conceptually:

`A -> B -> B checks communication policy -> allowed? -> COPY/work`

not:

`A -> B -> Judge -> authorization`

## COPY protocol

Agent delegation/requests follow **AUTHORIZE -> COPY -> work -> REPORT BACK**.

When Agent A sends work/request to Agent B:

1. B validates sender identity/context.
2. B verifies A -> B communication/delegation is allowed.
3. B validates that it understands the request and that accepting it is within B's own authority.
4. B replies promptly with `COPY` only when all checks pass.
5. `COPY` means: **I received and understood this request, verified that you are authorized to delegate/contact me for it, accepted responsibility, and I will report back to you.**
6. After `COPY`, A does not need to continuously watch/poll B merely to know whether B finished.
7. B MUST report back when work is completed, blocked, refused/cannot continue, failed, or materially changes in a way A needs to know.

If B does not understand, is not authorized to accept, or the caller is not authorized to contact/delegate to B, B MUST NOT respond `COPY`.

## Report-back status

A report back SHOULD preserve original correlation/task context and use a clear status such as:

- `DONE`
- `BLOCKED`
- `REFUSED`
- `FAILED`
- `UPDATE`

B sends the report to the delegating/calling agent unless delegation explicitly establishes another return target.

## No polling assumption

Once `COPY` is received, the caller MAY become inactive and rely on report-back. Runtime implementations SHOULD preserve routing/correlation information needed to wake/notify the caller when B reports back.

## Authority boundary

Communication does not grant capability/command authority. B still follows role, capability matrix, communication matrix, command matrix and runtime authorization. Receiver-side communication authorization is one gate, not the only gate.

## Human communication

The protocol is mandatory for agent-to-agent delegation. Human-facing conversation may use natural interaction and does not need literal `COPY` acknowledgements unless the runtime/workflow chooses to expose them.