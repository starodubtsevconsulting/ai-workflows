# Workflow Specification

Common contract and repository structure for reusable workflows.

## Structural consistency principle

Every workflow MUST expose the same required skeleton even when parts are not populated. Empty/template structures represent missing data rather than removing required structure.

## Core model

A workflow represents a reusable human/business activity. A flow is a bounded process inside it.

`Role definition -> agents.md realization -> Team policy -> runtime roster -> command/runtime authorization`

## Team model: policy versus runtime roster

Every workflow owns a **Team** definition under `team/`. Team is the static, version-controlled collaboration/security contract for that workflow.

Team defines:

- which roles/agent realizations participate;
- responsibilities/capabilities;
- which role-to-role communication routes are allowed;
- which commands each agent realization may use;
- prompt/intent routing and multi-agent flows;
- workflow-specific staffing/lifecycle authority.

Team does **not** store current runtime agent IDs in version-controlled matrices.

The runtime maintains a separate **authoritative runtime roster** binding team slots/role instances to current `agent_id` values and active/inactive state. This roster changes when Admin/Manager/runtime performs staffing.

`team policy (static) + runtime roster (dynamic) = effective team identity/communication trust`

All agents inherit the common roster-validation and communication protocol from [`_common/communication.md`](_common/communication.md) through [`role.spec.md`](role.spec.md). Workflow files MUST NOT repeat those common rules for each agent.

## Agent lifecycle: minimal active roster

Keep active population as small as practical. Create/activate only when needed; retire/archive bounded workers when responsibility ends; avoid unnecessary duplicates; allow temporary safe handoff overlap; prefer recoverable archival/deactivation over destructive deletion.

Multiple instances of one reusable role are allowed when workflow need justifies them, for example `Coder 1` and `Coder 2`. Each is a separate runtime roster entry/ID under the same role policy unless explicitly specialized.

## Staffing authorities

A workflow explicitly declares which agent realizations may change runtime team membership.

Admin, when present, is a Human-facing operational lifecycle/recovery authority for its workflow.

Manager MAY also receive normal staffing authority where the workflow requires work-capacity management, such as adding/replacing/retiring Coders.

Every authorized staffing operation MUST follow the common runtime-roster update protocol. Staffing is incomplete until the authoritative roster/trust state is updated and the team can observe it.

## Optional Admin

A workflow MAY define Admin. Admin is not mandatory. When present, it is Human-facing and provides workflow operational lifecycle/recovery authority. Governance/rule changes remain Human -> Judge.

If Admin itself is replaced, use successor-first handoff: create/verify successor, update trust/roster, then archive predecessor. Failed verification leaves predecessor active.

## Required folder structure

Every workflow MUST contain `README.md`, `workflow.md`, `agents.md`, and `team/` with `README.md`, `capability-matrix.csv`, `communication-matrix.csv`, and `command-matrix.csv`.

## agents.md

Defines workflow-local agent realizations/provider-independent runtime hints.

| Agent | Role | Human-facing override | Intelligence | Reasoning | Context | Memory | Lifecycle | Scheduled | Schedule intent | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |  |  |  |

Every workflow SHOULD explicitly define Admin or state it is absent.

## team/

Required shared static coordination/authority contract. Commands and communication are not granted by default. Missing grants mean not allowed; explicit forbidden entries document intentional no-go boundaries.

Runtime IDs do not belong in these static matrices.

## workflow.md

Required authoritative workflow contract with purpose/boundary, strategic layer, roles/composition, strategies, flows/events, prompt routing/use cases, connected commands, memory, inputs/outputs/evidence, runtime and privacy boundaries.

### Prompt routing / use cases

| Example prompt / intent | Route type | Route target | Result / notes |
| --- | --- | --- | --- |
|  |  |  |  |

### Connected commands

Required even when empty; references reusable AI Commands.

## Concepts

**Workflow** = long-lived domain/activity.

**Role** = reusable responsibility definition.

**Agent** = workflow/runtime realization of a role.

**Team** = static workflow-specific collaboration/security policy.

**Runtime roster** = dynamic authoritative mapping of team instances to current runtime IDs/state.

**Flow** = bounded process inside workflow.

**Command** = bounded reusable action.

## Memory

Workflows declare memory semantics using `_common/memory.md`. Strategic roles may own persistent memory; execution roles normally use session context unless explicitly specified.

## Runtime boundary

Workflow definitions remain provider independent. Runtime/profile maps abstract requirements to concrete models, IDs/roster, schedules/triggers, commands, credentials/integrations and final authorization.

## Minimum acceptance checklist

- [ ] required skeleton exists even when empty;
- [ ] static Team policy is defined separately from dynamic runtime roster;
- [ ] common communication/trust rules are inherited rather than duplicated;
- [ ] workflow explicitly defines Admin or states Admin absent;
- [ ] staffing authorities are explicit;
- [ ] staffing changes update authoritative runtime roster;
- [ ] multiple role instances can be represented safely when needed;
- [ ] active roster follows minimum-necessary principle;
- [ ] every defined agent declares scheduled yes/no and schedule intent;
- [ ] team matrices exist;
- [ ] command/communication access is not granted unless explicit;
- [ ] prompt routing/use cases and connected commands exist;
- [ ] memory/runtime/privacy boundaries are represented.