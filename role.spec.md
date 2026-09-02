# Role Specification

This document defines the common contract for reusable roles.

A role is a reusable responsibility/behavior definition. A runtime agent is an implementation/instance of a role inside a particular workflow/profile.

## Human-facing `why.md`

Every reusable role MUST have a companion human-facing `why.md` explanation.

`role definition = what it is / what it must do`

`why.md = why we introduced it / what problem it solves`

`why.md` is explanatory/non-normative and may use stories, examples, analogies, diagrams/media, trade-offs and references. Keep normative authority in role/spec files and avoid duplicating full role contract.

## Required role properties

Every role definition MUST contain Properties with at least `level`, `human-facing`, `interaction-mode`, `memory-class`, and `lifecycle`.

## Agent instantiation contract

A role definition is not yet a runtime agent. Instantiating a role as an agent requires the workflow/profile/runtime to fill the role's required runtime parameters and bindings.

Every runtime AI agent MUST have, at minimum:

- concrete runtime identity/team slot;
- workflow binding and applicable source/project resolution;
- role binding;
- model/intelligence/reasoning/context configuration required by the workflow;
- memory configuration;
- lifecycle configuration;
- scheduling configuration (`yes/no` plus intent when scheduled);
- required capability implementation bindings;
- applicable communication/command/lifecycle authorization from Team policy;
- clone policy with `clone-after-compactions` and `clone-at-context-utilization` thresholds.

Clone policy is harness-neutral. The runtime uses reliable context-health signals exposed by the active harness/provider/runtime. When both configured signals are available, **proactive cloning requires both thresholds to be reached**:

`compaction/equivalent count >= clone-after-compactions`

**AND**

`context utilization/pressure >= clone-at-context-utilization`

The compaction threshold indicates that meaningful context churn has already occurred; the utilization threshold indicates that the current context is again sufficiently full to justify replacement. Using both avoids cloning immediately after a compaction merely because its historical count increased.

If a harness cannot expose one of the configured signals, runtime MAY use an explicitly configured harness-specific equivalent policy. It MUST NOT silently reinterpret an unavailable signal or invent context-health data. Such fallback/equivalent policy must preserve the goal: replace the Agent while it can still produce reliable knowledge transfer.

A workflow/profile may explicitly override values for a concrete agent instance, but required parameters MUST resolve before the instance is considered ready.

An agent with unresolved required instantiation parameters is **NOT READY**. It MUST NOT enter normal active workflow operation or be trusted as an initialized team participant.

Conceptually:

`Role + workflow bindings + runtime parameters + Team authority -> candidate Agent -> validation -> READY Agent`

The workflow specification defines where workflow-local values are declared; this role specification defines that the values are required to instantiate the role.

## Initialization validation

Agent initialization is a governance boundary because it determines whether a runtime participant actually satisfies the role/workflow/team contract.

The lifecycle/staffing authority that creates an agent is responsible for assembling a complete candidate instance. It MUST NOT mark/register that candidate as normally active merely because the process/chat/session exists.

Before normal activation, the candidate configuration MUST be validated against applicable role, workflow and Team rules.

Judge is the governance validator for this contract when the workflow defines a Judge. The candidate agent does not directly ask Judge for approval by default. Instead, the lifecycle/staffing authority submits or exposes the candidate configuration/runtime facts for validation. This preserves the normal communication boundary while giving Judge a bounded initialization-validation path.

Conceptually:

`lifecycle authority creates candidate -> Judge validates applicable rules -> PASS -> roster/team activation`

`lifecycle authority creates candidate -> Judge finds violation -> NOT READY -> lifecycle authority fixes/recreates -> validate again`

Initialization validation is a narrow governance interaction, not general permission for arbitrary agents to converse with Judge.

Judge may also detect invalid/incomplete initialized agents during its scheduled runtime compliance audits. A participant that is missing required initialization data remains a governance violation even if it was accidentally admitted to the roster.

When a workflow has no Judge, the runtime/lifecycle authority MUST still perform deterministic structural validation of required fields before activation; absence of Judge does not waive the instantiation contract.

## Role capabilities versus concrete commands

Reusable roles describe **conceptual capabilities/responsibilities**, never concrete AI Command dependencies.

Examples:

- Worker may require a domain-specific capability such as source control in a Software Development workflow.
- Manager may require work/ticket-tracking capability.

A reusable role MUST NOT bind those concepts to concrete AI Commands. Concrete binding belongs to workflow agent realization:

`Role conceptual capability -> agents.md implementation binding -> Team command authorization -> Command/provider/harness`

A role may describe what a capability means in domain language. It must remain valid if the workflow later implements that capability with a different command, harness-native feature, provider or mechanism.

## Prompt / intent scenarios

Every role MUST contain a prompt/intent scenario table, even when empty.

| Example prompt / intent | Role interpretation | Workflow routing required |
| --- | --- | --- |
|  |  |  |

Reusable roles describe recognized intent, not workflow-specific peers/commands/orchestration. When routing is required, runtime agent consults active workflow/team definition.

## Inherited agent communication and trust

Every runtime agent inherits [`_common/communication.md`](_common/communication.md). Individual role files MUST NOT duplicate common protocol unless documenting role-specific exception/restriction.

## Team/runtime separation

Reusable roles do not own concrete team membership. Workflow defines static Team contract; runtime maintains dynamic roster mapping concrete IDs to team slots/instances.

## Knowledge transfer

**Knowledge transfer** is the deliberate handoff of task-relevant working knowledge from an outgoing Agent to its replacement before the outgoing Agent loses reliable access to that knowledge.

Knowledge transfer is the primary reason proactive cloning happens before context exhaustion.

The transfer SHOULD preserve at least:

- current objective/responsibility;
- important decisions and why they were made;
- work already completed;
- current state and relevant evidence/references;
- unresolved questions and blockers;
- assumptions/constraints;
- next intended action.

It is not normally a raw transcript dump. The outgoing Agent produces a compact, task-relevant handoff while it is still capable of accurately describing its working state. The lifecycle authority transports that handoff to the replacement. The replacement MUST acknowledge/incorporate the supplied knowledge before it is considered ready to continue the responsibility.

## Common clone lifecycle

Every runtime AI agent MUST understand that its instance may be cloned/replaced as part of workflow lifecycle management.

The reusable role MUST NOT hard-code a particular ordinary role such as Manager as the cloning authority. Which participant may initiate cloning is determined by the active workflow/team configuration and its authoritative permission/matrix rules.

### Proactive cloning

Proactive cloning is the normal/healthy path. It starts **before context exhaustion**, while the outgoing Agent can still provide reliable knowledge transfer.

```mermaid
flowchart TD
    A[Monitor Agent context health]
    B{Compaction threshold reached?}
    C{Utilization threshold reached?}
    D[Continue monitoring]
    E[Stop ordinary work]
    F[Request knowledge transfer]
    G[Outgoing Agent produces handoff]
    H[Mark outgoing Agent: (cloning)]
    I[Lock outgoing Agent]
    J[Create replacement]
    K[Apply Agent configuration]
    L[Pass knowledge transfer to replacement]
    M[Replacement acknowledges knowledge]
    N[Judge validates candidate]
    O{PASS?}
    P[Fix or recreate candidate]
    Q[Update roster / team configuration]
    R[Propagate team change]
    S[Replacement ACTIVE]
    T[Outgoing Agent ARCHIVED]

    A --> B
    B -- No --> D --> A
    B -- Yes --> C
    C -- No --> D
    C -- Yes --> E --> F --> G --> H --> I --> J --> K --> L --> M --> N --> O
    O -- No --> P --> N
    O -- Yes --> Q --> R --> S --> T
```

The critical invariant is:

`knowledge transfer MUST happen before (cloning) lock and before context exhaustion`

Once the outgoing Agent has surrendered its handoff, it is locked and must not continue ordinary work.

### Recovery cloning after context loss

Recovery cloning is the degraded/emergency path. It applies when proactive replacement was missed and the Agent is already exhausted or otherwise unable to produce trustworthy knowledge transfer.

In this state the system MUST NOT pretend that the old Agent still possesses recoverable working knowledge.

Conceptually:

`context lost -> stop/lock exhausted Agent -> create replacement -> reconstruct best available context -> validate -> activate -> archive old Agent`

Reconstruction may later use other evidence sources such as workflow artifacts, task/ticket state, source control, runtime evidence, durable memory, or knowledge held by another supervising/coordinating participant. Those recovery mechanisms are separate from normal knowledge transfer and may be incomplete.

Recovery cloning therefore has weaker continuity guarantees than proactive cloning. The architecture SHOULD prefer proactive cloning whenever reliable context-health signals make it possible.

### Clone signal behavior

When an agent receives a clone/replace lifecycle signal, it MUST first validate the sender against the current authoritative team configuration/permission model. A claimed role name or conversational assertion of authority is not sufficient.

If the sender is not authorized, the agent MUST refuse the lifecycle request and remain in its current state.

If the sender is authorized and the Agent still has reliable context, it MUST stop ordinary work, produce the requested knowledge transfer, then enter `(cloning)` when marked by the lifecycle authority/runtime.

The visible runtime name is part of the lifecycle state representation:

`<agent name> -> <agent name> (cloning)`

An agent whose authoritative runtime identity/name is marked `(cloning)` MUST treat itself as locked even if another participant asks it to resume work.

While `(cloning)`:

- no domain/task work is allowed;
- no new work may be accepted;
- no flow may be started or resumed;
- no ordinary commands/tools may be invoked;
- no ordinary mutations are allowed;
- ordinary inbound/outbound agent communication MUST be refused;
- only minimal lifecycle/protocol communication with an authorized lifecycle authority is permitted.

The Agent does not create its own replacement, transport its handoff to the replacement, update the roster, notify the team, or archive itself unless a concrete workflow explicitly grants such lifecycle authority. Those responsibilities belong to the authorized lifecycle authority/runtime.

## Command authority — not granted by default

Concrete commands are never granted at reusable role level. Workflow implementation binds capabilities and grants commands.

`Role capability -> Workflow agent realization -> command-matrix grant -> Runtime authorization -> Command`

Omission means not granted; explicit `forbidden` means intentional no-go.

## Human participant

Every workflow starts from or ultimately serves Human. Human is not an AI agent but MUST be represented in workflow team communication/capability modeling when interaction exists.

## Human-facing semantics

`human-facing` is a default characteristic and may be explicitly overridden by workflow/profile.

## Interaction mode

- `reactive` — acts when invoked/routed by Human, role, flow, event or schedule.
- `proactive` — may initiate work/communication when mandate/runtime allow it.
- `mixed` — supports both.

Interaction mode does not grant authority.

## Override rule

Reusable role properties are defaults. Workflow/profile specialization may override explicitly but SHOULD NOT silently broaden authority, privacy access, command permissions or memory scope.

## Required role sections

Every role SHOULD define purpose/responsibility, properties, prompt/intent scenarios, responsibilities, boundaries, memory/lifecycle behavior, Human interaction expectations and conceptual capability needs where relevant.

## Acceptance checklist

- [ ] Purpose/responsibility is defined.
- [ ] Required Properties are declared.
- [ ] Companion human-facing `why.md` exists.
- [ ] Prompt/intent scenario table exists even if empty.
- [ ] Role describes conceptual capabilities rather than concrete AI Commands.
- [ ] Common communication/trust protocol is inherited rather than duplicated.
- [ ] Runtime agent inherits proactive/recovery clone lifecycle and validates cloning authority.
- [ ] Agent instantiation resolves every required runtime parameter, including both clone thresholds, before READY.
- [ ] Proactive cloning requires configured context-health conditions and occurs before exhaustion.
- [ ] Knowledge transfer is completed before `(cloning)` lock when reliable context remains.
- [ ] Initialization is validated before normal roster/team activation.
- [ ] Role itself grants no concrete commands.
- [ ] Workflow agent realization binds required capabilities to implementations.
- [ ] Team command matrix remains authoritative for command permission.