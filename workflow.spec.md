# Workflow Specification

This document defines the common contract and repository structure for every reusable workflow published here.

## Structural consistency principle

Every workflow MUST expose the same required skeleton even when parts are not populated yet. Absence of data is represented by an empty/template file, not by absence of a required file/folder.

## Core model

A workflow represents a reusable human or business activity. A flow is a bounded process inside a workflow.

`Role definition -> agents.md realization -> Team relationships -> command policy -> runtime agent`

## Agent lifecycle: minimal active roster

A workflow SHOULD keep the active agent population as small as practical.

- Create/activate agents only when their responsibility requires them.
- Ephemeral workers should be retired/archived when bounded responsibility ends.
- Do not keep duplicate agents alive without a specific lifecycle/availability reason.
- Persistent agents are justified by durable continuity, scheduled responsibility or another explicit workflow need.
- Temporary overlap is allowed when safe replacement requires it, but should end as soon as the successor is verified.
- Prefer recoverable archival/deactivation over destructive deletion unless a workflow explicitly requires otherwise.

The goal is not minimum agent count at any cost; it is **minimum necessary active authority and resource usage**.

## Optional Admin

A workflow MAY define an `Admin` agent. Admin is not mandatory.

When present, Admin is a Human-facing operational lifecycle/recovery role rather than a normal domain worker.

Typical responsibilities may include:

- initialize/bootstrap the workflow's declared agent roster;
- create/activate agents required by the workflow;
- reinitialize or replace broken/stale agent instances;
- archive/deactivate agents no longer required;
- reconcile runtime roster with workflow definition;
- restore a valid workflow runtime state when normal routing/lifecycle is stuck;
- provide a Human-controlled operational recovery path when agents cannot unblock themselves under normal rules.

Conceptually:

`Human -> normal human-facing agent(s) -> normal workflow work`

Recovery/lifecycle path when Admin exists:

`Human -> Admin -> initialize/reconcile/replace/archive workflow agents`

Admin does not participate in ordinary domain work merely because it has lifecycle authority. Admin also does not replace Judge: governance/rule changes remain Human -> Judge.

Admin authority MUST be explicitly represented in team capability/communication/command policy like any other agent. Its lifecycle authority is not an unrestricted bypass of workflow governance.

### Admin replacement

If Admin itself must be replaced, successor-first handoff is preferred:

`active Admin -> create successor -> verify successor identity/readiness -> archive predecessor`

If successor verification fails, predecessor remains active. Temporary overlap exists only for the bounded handoff and does not imply ordinary Admin-to-Admin communication authority.

## Required folder structure

Every concrete workflow MUST contain `README.md`, `workflow.md`, `agents.md`, and `team/` with `README.md`, `capability-matrix.csv`, `communication-matrix.csv`, and `command-matrix.csv`.

## agents.md

Required for every workflow. It defines workflow-local agent realizations and provider-independent runtime hints.

| Agent | Role | Human-facing override | Intelligence | Reasoning | Context | Memory | Lifecycle | Scheduled | Schedule intent | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |  |  |  |

Every workflow SHOULD make the Admin decision explicit in `agents.md`/workflow documentation: Admin is either defined with concrete properties or intentionally absent.

### Scheduled agent semantics

Every agent realization explicitly declares scheduled yes/no and why it wakes. Scheduling does not mean continuous surveillance and grants no additional authority.

## team/

Required shared coordination/authority contract with capability, communication and command matrices. Commands are not granted by default. Missing communication/command grants are not allowed by default; explicit forbidden entries document intentional no-go boundaries.

## workflow.md

Required authoritative workflow contract with standard sections/placeholders for purpose/boundary, strategic layer, roles/composition, strategies, flows/events, Prompt routing/use cases, Connected commands, memory, inputs/outputs/evidence, runtime and privacy boundaries.

### Prompt routing / use cases

| Example prompt / intent | Route type | Route target | Result / notes |
| --- | --- | --- | --- |
|  |  |  |  |

### Connected commands

Required even when empty; references reusable commands from the AI Commands repository.

## Concepts

**Workflow** = long-lived domain/activity.

**Strategy** = HOW the domain is approached.

**Flow** = bounded process inside workflow.

**Role** = reusable responsibility definition.

**Agent** = workflow/runtime realization of a role.

**Admin** = optional Human-facing workflow lifecycle/recovery agent; not a normal domain worker and not a governance-rule authority.

**Scheduled agent** = periodically/event-triggeredly activated agent for declared purpose.

**Team** = relationships/ownership/communication/command authority among agents.

**Command** = bounded reusable action.

## Memory

Workflows declare memory semantics using `_common/memory.md`. Strategic roles may own persistent memory; execution roles normally use session context unless explicitly specified.

## Runtime boundary

Workflow definitions remain provider independent. Runtime/profile maps abstract requirements to concrete models, schedules/triggers, commands, credentials and integrations and provides final authorization.

## Minimum acceptance checklist

- [ ] required skeleton exists even when empty;
- [ ] workflow explicitly defines Admin or states Admin is intentionally absent;
- [ ] active roster follows minimum-necessary lifecycle principle;
- [ ] ephemeral agents have a retirement/archive path;
- [ ] Admin, when present, is Human-facing and lifecycle/recovery scoped;
- [ ] Admin does not replace Judge/governance;
- [ ] every defined agent declares scheduled yes/no and schedule intent;
- [ ] team matrices exist;
- [ ] command access is not granted unless explicit;
- [ ] Prompt routing/use cases and Connected commands exist;
- [ ] memory/runtime/privacy boundaries are represented.