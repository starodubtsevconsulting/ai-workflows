# AI Workflows

Reusable workflow definitions for coordinating AI-assisted work.

> Part of the broader [AI public collection](https://github.com/starodubtsevconsulting/ai), which provides the overview, shared vocabulary, experiments, benchmarks, and links between the published AI projects.

```mermaid
flowchart TD
  Profile["Profile: policy/configuration"] --> Workflow["Workflow"]
  Template["Team Template"] --> Workflow
  Roles["Reusable Roles"] --> Workflow
  Workflow --> Flows["Flows"]
  Workflow --> Agents["Runtime Agents"]
  Agents --> Commands["Capabilities / AI Commands"]
  Flows --> Outcome["Coordinated outcome"]
  Commands --> Outcome
```

## Vocabulary

The repository uses a small set of terms deliberately. The distinction matters because reusable definitions, workflow configuration and running AI participants are different things.

| Term | Meaning | Example |
| --- | --- | --- |
| **Workflow** | Long-lived reusable business/work activity. It defines the domain boundary, team, flows, sources and coordination. | Software Development |
| **Flow** | Bounded process/phase inside a workflow. | Implementation, Testing, Review, Release |
| **Role Type** | Generic organizational/lifecycle category shared across workflows. It describes what kind of participant a role is. | Worker, Manager, Judge |
| **Role** | Reusable behavioral contract specializing a role type: responsibilities, boundaries, capabilities, lifecycle expectations and memory needs. | Coder, Designer Reviewer |
| **Agent** | Runtime instance of a role inside a workflow. It has concrete identity/configuration and may be replaced over time. | Coder 1 |
| **Agent name / identity** | Concrete runtime identity of an instantiated role. The name is not the reusable role itself. | `Coder 1`, `Frontend Coder` |
| **Team** | Workflow-specific set of participants plus collaboration/security/authorization policy. | Software Development Team |
| **Team Template** | Reusable organizational/authority pattern expressed primarily in role types and inherited by workflows. | `standard` |
| **Runtime roster** | Current mapping of active team slots/agents to runtime identities and lifecycle state. | Coder 1 active; Coder 0 archived |
| **Command / capability** | Bounded executable ability available to authorized agents. Roles describe conceptual capabilities; workflows bind them to implementations. | source control, ticket tracking |
| **Source / Project** | Concrete subject/context a workflow operates on. Project is a common Software Development source type. | Repository/project A |
| **Profile** | External personal/organization configuration that activates workflows and supplies concrete runtime/project/provider policy. | Private consulting profile |

### Role Type → Role → Agent

This is one of the central distinctions:

```mermaid
flowchart TD
  Type["Role Type: Worker"] --> Role["Role: Coder"]
  Role --> Agent["Agent: Coder 1"]
  Agent --> Runtime["Runtime identity + context + model + permissions"]
```

A **Worker** is not the name of a particular agent. It is a reusable type. **Coder** is a reusable role that specializes Worker. **Coder 1** is a running Agent created from that role.

The same pattern applies to other participants:

`Judge -> Judge role -> concrete Judge agent`

`Manager -> Manager role -> concrete Manager agent`

The role and agent may happen to use the same display name; they are still different architectural layers.

## Standard role types

The current standard team organization uses five role types:

| Role type | Purpose | Typical lifecycle character |
| --- | --- | --- |
| **Admin** | Human-controlled workflow administration and recovery. | Protected; outside Manager lifecycle authority. |
| **Judge** | Governance, rule integrity and compliance checking. | Managed but communication-protected. |
| **Strategist** | Workflow-local strategy and durable direction. | Managed; continuity matters. |
| **Manager** | Coordination, staffing and lifecycle management. | Managed; may self-replace. |
| **Worker** | Performs bounded domain work. | Disposable/replaceable by design. |

Concrete workflow roles map into these types. For Software Development, for example:

`Worker -> Coder`

`Worker -> Designer Reviewer`

`Worker -> Command Runner`

`Worker -> UI Acceptance Tester`

The role-type list is intentionally small. A new type should be introduced only when a role genuinely requires organizational/lifecycle semantics that do not fit an existing type.

## Team templates

A workflow should not copy nearly identical matrices merely because it has differently named workers.

A **Team Template** packages reusable team organization and authority rules around role types. The current [`standard` template](_common/team-templates/standard/README.md) contains the default lifecycle policy for Admin, Judge, Strategist, Manager and Worker.

```mermaid
flowchart TD
  Standard["Team Template: standard"] --> Types["Admin / Judge / Strategist / Manager / Worker"]
  Types --> SD["Software Development"]
  Types --> Other["Another Workflow"]
  SD --> SDMap["Coder = Worker\nDesigner Reviewer = Worker\nManager = Manager"]
  Other --> OtherMap["Domain Worker = Worker\nManager = Manager"]
```

A concrete workflow selects a template, binds its concrete roles to role types, and declares only genuine exceptions/overrides. If a future workflow needs a fundamentally different organization, another named Team Template can be introduced rather than duplicating or weakening the standard one.

## Workflow, Flow and Team

A workflow is the whole reusable activity. A flow is a bounded process inside it; it is not a synonym for route.

For example:

```mermaid
flowchart TD
  W["Software Development Workflow"] --> I["Implementation Flow"]
  W --> T["Testing Flow"]
  W --> R["Review Flow"]
  W --> D["Debugging Flow"]
  W --> Rel["Release Flow"]
```

The workflow owns the team. Individual flows coordinate whichever subset of that team is needed for that bounded process. An Agent may therefore participate in multiple flows.

## Repository structure

Top-level non-underscore folders are concrete workflows. Shared building blocks live under `_common/` and are not workflows.

```text
ai-workflows/
  _common/
    roles/
      ... reusable role contracts ...
    team-templates/
      standard/
        README.md
        lifecycle-matrix.csv

  software-development/
    workflow.md
    agents.md
    team/
      ... workflow bindings and overrides ...
```

## Roles, workflows, and agents

A **role** is a reusable behavioral contract. Roles are defined once under [`_common/roles/`](_common/roles/).

A **workflow** is a reusable business/work process. Its `workflow.md` selects/specializes common roles, defines flows and domain coordination, and selects reusable capabilities.

An **agent** is the runtime realization of a role inside a selected workflow. Agents are not the primary reusable definitions in this repository.

Conceptually:

`Role Type -> Role -> workflow realization -> Agent -> runtime identity`

The workflow remains independent of a particular organization, client, project, model provider, harness, or hosting model.

## Lifecycle and cloning

Agents are replaceable runtime instances. Roles and responsibilities survive individual agent instances.

Every Agent must satisfy the common instantiation contract from [`role.spec.md`](role.spec.md), including clone policy. Context-health signals are harness-neutral: compaction/equivalent count is preferred when available, with context utilization/pressure as fallback.

The common clone lifecycle is:

`ACTIVE -> handoff -> (cloning) LOCKED -> ARCHIVED`

while a validated replacement receives the responsibility and becomes active. The full vertical lifecycle diagram and normative rules live in [`role.spec.md`](role.spec.md).

Lifecycle/control authorization is distinct from ordinary agent communication. For example, a Manager may be allowed to send Judge an authorized clone signal while remaining forbidden from ordinary conversation with Judge.

## Governance

Judge provides workflow-scoped governance. Its cheap scheduled monitoring is intentionally bounded sampling rather than continuous full-history review. Human may explicitly request deeper historical audits when additional assurance is worth the context/token cost.

Judge also provides narrow lifecycle governance gates such as candidate-agent initialization validation. These control-plane interactions do not create general conversational permission between agents and Judge.

## Concrete workflows

### Software Development

[`software-development/workflow.md`](software-development/workflow.md) is the primary concrete workflow currently used to exercise and refine the architecture. Its concrete agent realizations are defined in [`software-development/agents.md`](software-development/agents.md).

## Common agent/runtime contract

[`role.spec.md`](role.spec.md) defines the common Role-to-Agent instantiation and lifecycle contract. [`workflow.spec.md`](workflow.spec.md) defines how workflows supply concrete bindings while avoiding duplication of role-level normative rules.

Common Team Templates provide reusable organizational/authority policy. Workflow-local matrices should represent bindings and genuine overrides rather than copies of common policy.

## Relationship to AI Commands

Workflows coordinate work. Commands describe reusable executable capabilities that workflows may select. The public command catalog is available in the [AI Commands repository](https://github.com/starodubtsevconsulting/ai-commands).

A command may be reused by many workflows, and a workflow may compose many commands without owning their implementations.

## Publication boundary

Profiles, client bindings, credentials, private data, organization-specific configuration, local paths, and private runtime integrations are not published here. Concrete workflows should be published only after portability, documentation, privacy, and security review.
