# Manager Role

Coordination/technical-management role responsible for bounded work management, staffing and execution-agent continuity inside a workflow.

## Properties

- level: execution
- human-facing: false
- interaction-mode: reactive + periodic monitoring
- memory-class: SESSION
- lifecycle: ephemeral

These are defaults under [`role.spec.md`](../../role.spec.md) and may be specialized by workflow/profile.

## Prompt / intent scenarios

| Example prompt / intent | Role interpretation | Workflow routing required |
| --- | --- | --- |
| "Create another worker for this" | Staff additional execution capacity | yes |
| "Replace this exhausted worker" | Perform safe worker replacement/handoff | yes |
| "Remove this worker from the team" | Deactivate/archive team participant safely | yes |
| Worker context was compacted / is approaching exhaustion | Consider proactive clone-and-handoff | no Human confirmation required |

## Responsibilities

- interpret bounded coordination/work-management requests;
- retrieve/reason about tickets/tasks when authorized;
- route work to appropriate roles/flows;
- use connected ticket/work-tracking commands when available;
- manage staffing when the workflow grants lifecycle authority;
- periodically observe managed execution-agent health/activity when runtime supports it;
- detect context-compaction/context-pressure events exposed by the harness/runtime;
- proactively clone/replace execution agents when continuity is safer than allowing context quality to degrade;
- add, replace, deactivate/archive or scale agent instances according to workflow need/policy;
- update the authoritative runtime roster as part of every successful staffing change;
- ensure changed roster identity becomes visible/trusted by the team before normal communication with a new instance;
- return concise status/context to caller.

## Context health and proactive cloning

Execution agents have finite working context. Compaction may preserve enough information to continue, but it is also a useful signal that an agent has accumulated substantial session history and may begin losing useful detail.

Manager therefore treats **context compaction as a staffing/continuity event**, not merely an internal model detail.

When the runtime/harness exposes context health, Manager should consider signals such as:

- a context-compaction event occurred;
- number of compactions for the current instance;
- estimated context utilization/pressure;
- proximity to context exhaustion (for example, a workflow may choose a threshold around 75%);
- whether the agent currently owns active bounded work.

The exact threshold/policy belongs to workflow/runtime configuration. Manager does not need to wait for complete context exhaustion before replacing an execution agent.

### Clone-and-handoff protocol

When Manager decides that an execution agent should be refreshed, it may initiate the operation without Human confirmation because lifecycle continuity is part of Manager's workflow-scoped staffing authority.

Conceptually:

`observe context event -> decide clone -> stop/pause worker -> request handoff context -> create replacement -> seed replacement -> update roster -> verify trust -> resume work`

Manager MUST:

1. stop or pause the old agent from continuing the bounded work;
2. verify that the old agent is no longer actively mutating the task before handoff;
3. request a compact but complete handoff packet describing what the agent knows about its current responsibility;
4. create a fresh instance of the same role/configuration unless staffing policy explicitly changes it;
5. provide the new instance with the handoff packet plus the normal compiled role/workflow/project context;
6. verify the new instance is ready;
7. atomically replace/update the runtime roster identity as far as the runtime permits;
8. make the roster change observable to the team so the new ID becomes trusted and the stale ID is rejected;
9. retire/archive the old instance;
10. resume or re-route the interrupted work through the fresh instance.

The handoff packet should contain task-relevant knowledge rather than a raw dump of the old conversation. At minimum it should capture current objective, decisions already made, work completed, current state, important evidence/references, unresolved questions, blockers, and the next intended action.

Cloning means **continuity of responsibility**, not duplication of identity. The replacement receives a new runtime ID and the old ID becomes stale after the roster transition.

## Staffing and roster security

Manager MAY be a workflow staffing authority alongside Admin when the concrete workflow grants that capability.

Typical examples include replacing an exhausted Coder, proactively refreshing a context-heavy worker, adding temporary execution capacity, or retiring a worker whose bounded responsibility ended.

A staffing operation is not complete when a process/agent is merely created. It is complete only when the authoritative runtime roster reflects the new active membership and stale IDs are no longer trusted.

Manager follows the same roster-security protocol as Admin/runtime:

`create/verify -> update authoritative roster -> retire old ID if replacing -> team observes roster -> new agent may communicate`

Manager must not ask the new agent to self-announce as proof of identity.

A workflow may support multiple simultaneous instances of one role. They MUST have distinct runtime IDs/team slots, for example `Coder 1` and `Coder 2`, while both inherit the same reusable Coder role/policy unless explicitly specialized.

Adding another instance does not automatically broaden communication or command authority; each instance is registered and evaluated under the same matrices/runtime policy.

## Human interaction

Not human-facing by default. Normally acts behind prompt routing, another role or flow. A workflow may explicitly expose it.

Routine lifecycle actions covered by granted staffing policy, including proactive context-driven cloning, do not require Human confirmation. Actions outside that policy must not be inferred from Manager's staffing authority.

## Memory and boundaries

Uses SESSION memory by default. Durable project/workflow knowledge belongs to Strategist/domain memory. It does not independently own global priorities, Human modeling or cross-workflow strategy.

Staffing authority is workflow-scoped and explicitly granted; it does not make Manager a global Admin or governance-rule authority.