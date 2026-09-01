# Workflow Specification

Common contract and repository structure for reusable workflows.

## Structural consistency principle

Every workflow MUST expose the same required skeleton even when parts are not populated.

## Core model

A workflow represents a reusable human/business activity. A flow is a bounded process inside it. A workflow may operate over multiple sources/projects.

`Role conceptual capabilities -> agents.md implementation bindings -> Team policy/authorization -> Source context -> runtime`

## Sources / projects

Every workflow MUST define how its sources are identified/resolved, even if only one source exists. Source owns source-specific context/configuration/artifacts/conventions.

Examples:

`Software Development -> Project A, Project B`

`Accounting -> Company A, Company B`

## Role capability implementation

Reusable roles define conceptual capabilities. `agents.md` turns those concepts into concrete workflow-local implementations.

Example:

`Coder role: source-control capability`

`Software Development agents.md: source-control capability -> AI Command source-control`

`team/command-matrix.csv: Coder -> source-control = allowed`

The three layers have different meanings:

| Layer | Responsibility |
| --- | --- |
| Role | Defines conceptual capability/responsibility. No concrete AI Command binding. |
| `agents.md` | Defines how that capability is implemented for this workflow agent. |
| `team/command-matrix.csv` | Authorizes or forbids concrete command invocation. |

Implementation binding does **not** grant authority. A command bound in `agents.md` remains unusable unless Team command policy grants it.

Capabilities may be implemented by AI Commands, harness-native capabilities, project/runtime facilities, memory providers or other mechanisms.

## Team model

Every workflow owns static Team definition under `team/`. Team defines collaboration/security/command authorization. Runtime maintains dynamic roster IDs/state.

## Agent lifecycle

Keep active population as small as practical. Prefer recoverable archival/deactivation over destructive deletion. Multiple instances of one role are allowed when justified.

## Staffing authorities

Workflow declares which agents may change runtime membership. Staffing changes update authoritative runtime roster/trust state.

## Optional Admin

Workflow MAY define Admin as Human-facing lifecycle/recovery authority. Governance remains Human -> Judge.

## Required folder structure

Every workflow MUST contain `README.md`, `workflow.md`, `agents.md`, and `team/` with required matrices.

## agents.md

Defines workflow-local realizations of reusable roles and concrete implementation bindings for their conceptual capabilities.

### Agent properties

| Agent | Role | Human-facing override | Intelligence | Reasoning | Context | Memory | Lifecycle | Scheduled | Schedule intent | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |  |  |  |

### Capability implementation bindings

Every workflow keeps this table even when empty.

| Agent | Role capability | Implementation type | Implementation | Notes |
| --- | --- | --- | --- | --- |
|  |  | `command` / `harness` / `runtime` / `project` / other |  |  |

Examples:

| Agent | Role capability | Implementation type | Implementation | Notes |
| --- | --- | --- | --- | --- |
| Coder | source control | command | `source-control` | Permission still comes from command matrix. |
| UI Acceptance Tester | computer use / vision | command | `computer-use` | Harness/provider resolved at runtime. |
| Coder | code editing | harness | configured code/filesystem capability | Not an AI Command. |

## team/

Required static coordination/authority contract. Missing communication/command grants mean not allowed. Runtime IDs do not belong in static matrices.

## workflow.md

Required authoritative workflow contract with purpose/boundary, sources/projects, strategic layer, roles/composition, strategies, flows/events, prompt routing/use cases, connected commands, memory, inputs/outputs/evidence, runtime/privacy boundaries.

### Sources / projects

| Source type | Identifier / resolution | Source-specific configuration / artifacts | Notes |
| --- | --- | --- | --- |
|  |  |  |  |

### Prompt routing / use cases

| Example prompt / intent | Route type | Route target | Result / notes |
| --- | --- | --- | --- |
|  |  |  |  |

### Connected commands

Required even when empty; references reusable AI Commands available to workflow implementation. Actual per-agent permission remains in command matrix.

## Concepts

**Workflow** = long-lived reusable domain/activity.

**Source** = concrete subject/context workflow operates on.

**Project** = common Software Development source type.

**Role** = reusable responsibility + conceptual capability definition.

**Agent** = workflow/runtime realization that binds role capabilities to concrete implementations.

**Team** = static workflow-specific collaboration/security/authorization policy.

**Runtime roster** = dynamic mapping of team instances to current IDs/state.

**Flow** = bounded process inside workflow.

**Command** = bounded reusable action/implementation option.

## Memory

Persistent source-specific knowledge SHOULD preserve source identity/scope.

## Runtime boundary

Runtime/profile resolves active source/project and maps requirements/bindings to concrete models, providers, IDs, paths, schedules, credentials and final authorization.

## Minimum acceptance checklist

- [ ] required skeleton exists;
- [ ] source/project resolution defined;
- [ ] roles contain conceptual capabilities, not concrete command bindings;
- [ ] `agents.md` contains capability implementation bindings table;
- [ ] concrete command bindings are authorized separately by command matrix;
- [ ] static Team policy separate from runtime roster;
- [ ] every agent declares scheduled yes/no;
- [ ] command/communication access requires explicit grant;
- [ ] memory/runtime/privacy boundaries represented.