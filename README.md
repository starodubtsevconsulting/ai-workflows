# AI Workflows

Reusable workflow definitions for coordinating AI-assisted work.

> Part of the broader [AI public collection](https://github.com/starodubtsevconsulting/ai), which provides the overview, shared vocabulary, experiments, benchmarks, and links between the published AI projects.

```mermaid
flowchart TD
  Profile["AI Profile: policy/configuration"] --> Workflow["Workflow"]
  Template["Team Template"] --> Workflow
  Roles["Reusable Roles"] --> Workflow
  Workflow --> Flows["Flows"]
  Workflow --> Agents["Runtime Agents"]
  Agents --> Commands["Capabilities / AI Commands"]
  Flows --> Outcome["Coordinated outcome"]
  Commands --> Outcome
```

## Vocabulary

| Term | Meaning | Example / reference |
| --- | --- | --- |
| **Workflow** | Long-lived reusable business/work activity defining domain boundary, team, flows, sources and coordination. | Software Development |
| **Flow** | Bounded process/phase inside a workflow. | Implementation, Testing, Review, Release |
| **Role** | Reusable organizational/behavioral contract. In informal/runtime language, **agent type** may be used as a synonym. | Worker, Manager, Judge, Strategist, Admin |
| **Agent** | Concrete workflow participant fulfilling a Role, with workflow-specific name/configuration and runtime instances. | Coder fulfills Worker |
| **Agent name / identity** | Concrete name/runtime identity of an Agent. | `Coder`, `Coder (1)` |
| **Team** | Workflow-specific participants plus collaboration/security/authorization policy. | Software Development Team |
| **Team Template** | Reusable organizational/authority pattern expressed in Roles and inherited by workflows. | `standard` |
| **Runtime roster** | Current mapping of active Agents/instances to runtime identities and lifecycle state. | Coder (2) active; Coder (1) archived |
| **Contextual knowledge / context** | Working information currently carried by an Agent's active context: objective, decisions, progress, evidence, blockers, assumptions and next action. This is distinct from persistent memory/knowledge. | Coder's current implementation context |
| **Context transfer** | Deliberate transfer of task-relevant contextual knowledge from an outgoing Agent to its replacement before context exhaustion. | Coder (1) → Coder (2) |
| **Context recovery** | Best-effort reconstruction of contextual knowledge after direct context transfer is unavailable or incomplete. | supervisor + tracker + artifacts |
| **Source / Project** | Concrete subject/context a workflow operates on. Project is a common Software Development source type. | Repository/project A |
| **[Profile](https://github.com/starodubtsevconsulting/ai-profile)** | External personal/organization configuration that activates workflows and supplies runtime/project/provider policy. | [AI Profile repository](https://github.com/starodubtsevconsulting/ai-profile) |
| **[Command](https://github.com/starodubtsevconsulting/ai-commands)** | Reusable bounded executable AI capability that Agents may invoke when authorized. | [AI Commands repository](https://github.com/starodubtsevconsulting/ai-commands) |

`Contextual knowledge` is intentionally used when clarity is useful for readers unfamiliar with LLM terminology. In shorter technical descriptions, `context` means the same working information. `Memory` remains reserved for information intentionally persisted beyond the current Agent context/session.

### Role → Agent

```mermaid
flowchart TD
  Role["Role: Worker"] --> Agent["Agent: Coder"]
  Agent --> Runtime["Runtime instance: Coder (1)"]
  Runtime --> Config["Context + model + permissions + lifecycle state"]
```

A **Worker** is a reusable Role. **Coder** is a Software Development Agent fulfilling that Role. `Coder (1)` may be the current runtime identity/instance of that Agent.

## Standard roles

| Role | Purpose | Typical lifecycle character |
| --- | --- | --- |
| **Admin** | Human-controlled workflow administration and recovery. | Protected; outside Manager lifecycle authority. |
| **Judge** | Governance, rule integrity and compliance checking. | Managed but communication-protected. |
| **Strategist** | Workflow-local strategy and durable direction. | Managed; continuity matters. |
| **Manager** | Coordination, staffing and lifecycle management. | Managed; may self-replace. |
| **Worker** | Performs bounded domain work. | Disposable/replaceable by design. |

Software Development examples:

`Worker -> Coder`

`Worker -> Designer Reviewer`

`Worker -> Command Runner`

`Worker -> UI Acceptance Tester`

## Team templates

A **Team Template** packages reusable team organization and authority rules around Roles. The current [`standard` template](_common/team-templates/standard/README.md) contains common capability, communication, lifecycle and command-policy matrices.

```mermaid
flowchart TD
  Standard["Team Template: standard"] --> Roles["Admin / Judge / Strategist / Manager / Worker"]
  Roles --> SD["Software Development"]
  Roles --> Other["Another Workflow"]
  SD --> SDMap["Coder = Worker\nDesigner Reviewer = Worker\nManager = Manager"]
  Other --> OtherMap["Domain Agent = Worker\nManager = Manager"]
```

A concrete workflow selects a template, assigns Roles to concrete Agents, and declares only genuine exceptions/overrides.

## Workflow, Flow and Team

A workflow is the whole reusable activity. A flow is a bounded process inside it.

```mermaid
flowchart TD
  W["Software Development Workflow"] --> I["Implementation Flow"]
  W --> T["Testing Flow"]
  W --> R["Review Flow"]
  W --> D["Debugging Flow"]
  W --> Rel["Release Flow"]
```

The workflow owns the team. Individual flows coordinate whichever subset of that team is needed. An Agent may participate in multiple flows.

## Lifecycle, context transfer and cloning

Agents are replaceable runtime instances. Roles and responsibilities survive individual runtime instances.

The preferred lifecycle is **proactive cloning**. The goal is to replace the Agent before exhaustion so its **contextual knowledge** can be transferred reliably.

With the standard two-signal clone policy, proactive replacement starts when both configured conditions are true:

`compaction/equivalent count >= configured threshold`

**AND**

`context utilization/pressure >= configured threshold`

```mermaid
flowchart TD
  A["Clone conditions reached"] --> B["Stop outgoing Agent"]
  B --> C["Context transfer"]
  C --> D["Outgoing Agent: (cloning) LOCKED"]
  D --> E["Create replacement"]
  E --> F["Pass contextual knowledge to replacement"]
  F --> G["Replacement acknowledges context"]
  G --> H["Validate + update roster/team"]
  H --> I["Replacement ACTIVE"]
  I --> J["Outgoing Agent ARCHIVED"]
```

Context transfer normally includes the current objective, decisions, completed work, current state, evidence, blockers, assumptions and next action. It is task-relevant rather than a raw transcript dump.

If proactive cloning is missed and the old Agent has exhausted/lost useful context, the system enters **recovery cloning**. Direct context transfer can no longer be trusted, so contextual knowledge is reconstructed from authorized evidence such as a supervisor/coordinating Agent, work tracker, session storage, durable memory or work artifacts.

The normative lifecycle and recovery behavior live in [`role.spec.md`](role.spec.md) and [`workflow.spec.md`](workflow.spec.md).

## Repository structure

```text
ai-workflows/
  _common/
    roles/
    team-templates/
      standard/
        README.md
        capability-matrix.csv
        communication-matrix.csv
        lifecycle-matrix.csv
        command-matrix.csv

  software-development/
    workflow.md
    agents.md
    team/
      ... workflow bindings and overrides ...
```

## Governance

Judge provides workflow-scoped governance. Its scheduled monitoring is intentionally bounded sampling rather than continuous full-history review. Human may explicitly request deeper historical audits.

## Concrete workflows

### Software Development

[`software-development/workflow.md`](software-development/workflow.md) is the primary concrete workflow currently used to exercise and refine the architecture. Its concrete Agent configuration is defined in [`software-development/agents.md`](software-development/agents.md).

## Common agent/runtime contract

[`role.spec.md`](role.spec.md) defines the common Role-to-Agent instantiation, context-transfer and lifecycle contract. [`workflow.spec.md`](workflow.spec.md) defines workflow recovery sources and concrete bindings.

## Relationship to AI Commands

Workflows coordinate work. Commands describe reusable executable capabilities that workflows may select. See the [AI Commands repository](https://github.com/starodubtsevconsulting/ai-commands).

## Relationship to AI Profile

Profiles are intentionally outside this repository. See the [AI Profile repository](https://github.com/starodubtsevconsulting/ai-profile).

## Publication boundary

Profiles, client bindings, credentials, private data, organization-specific configuration, local paths, and private runtime integrations are not published here.