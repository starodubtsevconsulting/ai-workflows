# Workflow Specification

Common contract and repository structure for reusable workflows.

## Structural consistency principle

Every workflow MUST expose the same required skeleton even when parts are not populated. Empty/template structures represent missing data rather than removing required structure.

## Core model

A workflow represents a reusable human/business activity. A flow is a bounded process inside it.

A workflow may operate over **multiple sources**. A source is a concrete subject/context to which the reusable workflow is applied, such as a software project/repository, company, property, client/account, media project or another domain-specific unit.

Examples:

`Software Development workflow -> Project A, Project B, Project C`

`Accounting workflow -> Company A, Company B, Company C`

The workflow owns reusable process/team/role policy. The source owns source-specific context, configuration, artifacts and conventions.

`Role definition -> agents.md realization -> Team policy -> Source context -> runtime roster -> command/runtime authorization`

## Sources / projects

Every workflow MUST define how its sources are identified/resolved, even if only one source currently exists.

A source SHOULD expose a stable identifier and the workflow-specific locations/configuration needed by agents. Agents MUST NOT assume that source-specific paths, repositories, credentials, test locations or conventions are globally shared across all sources.

For software development, a source will commonly be a **project**. Each project may define, for example:

- repository/workspace location;
- source-control context;
- ticket/project mapping;
- build/test commands or command configuration;
- product/runtime startup context;
- end-to-end/acceptance-test location;
- project-specific adapters/helpers/fixtures;
- other project conventions needed by the workflow.

A workflow-level role may therefore operate across many projects while resolving project-specific assets from the active source context.

## Team model: policy versus runtime roster

Every workflow owns a **Team** definition under `team/`. Team is the static, version-controlled collaboration/security contract for that workflow.

Team defines roles/agent realizations, capabilities, role-to-role communication, command access, prompt/intent routing, multi-agent flows and workflow-specific staffing/lifecycle authority.

Team does not store current runtime agent IDs. Runtime maintains an authoritative roster binding team slots/role instances to current IDs/state.

`team policy (static) + source context + runtime roster (dynamic) = effective workflow execution context`

All agents inherit common roster-validation/communication from `_common/communication.md` through `role.spec.md`; workflow files do not repeat it per agent.

## Agent lifecycle: minimal active roster

Keep active population as small as practical. Create/activate only when needed; retire/archive bounded workers when responsibility ends; avoid unnecessary duplicates; allow temporary safe handoff overlap; prefer recoverable archival/deactivation over destructive deletion.

Multiple instances of one role are allowed when justified, for example `Coder 1` and `Coder 2`, each with separate runtime identity.

## Staffing authorities

Workflow explicitly declares which agents may change runtime membership. Admin may provide lifecycle/recovery authority; Manager may receive normal staffing authority. Every staffing change updates authoritative runtime roster/trust state.

## Optional Admin

Workflow MAY define Admin. When present, it is Human-facing workflow lifecycle/recovery authority. Governance/rule changes remain Human -> Judge. Admin replacement uses successor-first verified handoff.

## Required folder structure

Every workflow MUST contain `README.md`, `workflow.md`, `agents.md`, and `team/` with `README.md`, `capability-matrix.csv`, `communication-matrix.csv`, and `command-matrix.csv`.

## agents.md

Defines workflow-local agent realizations/provider-independent runtime hints.

| Agent | Role | Human-facing override | Intelligence | Reasoning | Context | Memory | Lifecycle | Scheduled | Schedule intent | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |  |  |  |

## team/

Required shared static coordination/authority contract. Missing communication/command grants mean not allowed. Runtime IDs do not belong in static matrices.

## workflow.md

Required authoritative workflow contract with purpose/boundary, sources/projects, strategic layer, roles/composition, strategies, flows/events, prompt routing/use cases, connected commands, memory, inputs/outputs/evidence, runtime and privacy boundaries.

### Sources / projects

Every concrete workflow keeps this section even if currently empty/single-source.

| Source type | Identifier / resolution | Source-specific configuration / artifacts | Notes |
| --- | --- | --- | --- |
|  |  |  |  |

### Prompt routing / use cases

| Example prompt / intent | Route type | Route target | Result / notes |
| --- | --- | --- | --- |
|  |  |  |  |

### Connected commands

Required even when empty; references reusable AI Commands.

## Concepts

**Workflow** = long-lived reusable domain/activity.

**Source** = concrete subject/context the workflow operates on.

**Project** = common Software Development source type.

**Role** = reusable responsibility definition.

**Agent** = workflow/runtime realization of a role.

**Team** = static workflow-specific collaboration/security policy.

**Runtime roster** = dynamic mapping of team instances to current runtime IDs/state.

**Flow** = bounded process inside workflow.

**Command** = bounded reusable action.

## Memory

Workflows declare memory semantics using `_common/memory.md`. Persistent knowledge that is source-specific SHOULD preserve source identity/scope so context from different projects/companies is not silently mixed.

## Runtime boundary

Workflow definitions remain provider independent. Runtime/profile resolves active source/project and maps abstract requirements to concrete models, IDs/roster, paths, schedules/triggers, commands, credentials/integrations and final authorization.

## Minimum acceptance checklist

- [ ] required skeleton exists even when empty;
- [ ] source/project concept and resolution are defined;
- [ ] source-specific context is not silently shared across sources;
- [ ] static Team policy is separate from dynamic runtime roster;
- [ ] common communication/trust rules are inherited;
- [ ] Admin is explicitly defined or absent;
- [ ] staffing authorities are explicit and update roster;
- [ ] every agent declares scheduled yes/no and schedule intent;
- [ ] team matrices exist;
- [ ] command/communication access requires explicit grant;
- [ ] prompt routing/use cases and connected commands exist;
- [ ] memory/runtime/privacy boundaries are represented.