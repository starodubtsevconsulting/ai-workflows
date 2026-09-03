# Role Specification

This document defines the common contract for reusable Roles and their Agent realizations.

## Required Role properties

Every reusable Role definition MUST contain Properties with at least `level`, `human-facing`, `interaction-mode`, `memory-class`, and `lifecycle`.

A Role MAY provide a default for `elastic-pool-enabled`, but the concrete Agent realization MUST resolve the final value during instantiation.

## Agent instantiation contract

Every runtime AI Agent MUST resolve, at minimum:

- runtime identity/team slot;
- workflow/source binding;
- Role binding;
- model/intelligence/reasoning/context configuration;
- memory/lifecycle/scheduling configuration;
- capability bindings and Team authorization;
- clone policy: `clone-after-compactions`, `clone-at-context-utilization`;
- Elastic Agent Pool policy: `elastic-pool-enabled`.

When `elastic-pool-enabled: true`, the Agent configuration MAY additionally define:

- `elastic-pool-min-ready` — minimum idle/ready capacity to preserve;
- `elastic-pool-max` — optional maximum concurrent pool size;
- pool assignment naming/observability policy;
- completion policy such as reuse/reset vs archive/destroy excess capacity.

When `elastic-pool-enabled: false`, runtime MUST NOT horizontally create additional copies of that Agent merely for concurrency. Replacement cloning remains separately available according to lifecycle policy.

An Agent with unresolved required instantiation parameters is **NOT READY**.

## Elastic Agent Pool

Elastic Agent Pool is a horizontal capacity mechanism available to any Agent whose resolved configuration enables it.

`elastic-pool-enabled: true` means lifecycle/staffing authority may create additional concurrent instances for independent/bounded work according to workflow/runtime policy.

It does **not** mean the runtime must always maintain multiple instances.

Examples:

`Command Runner: enabled, min-ready = 1`

`Coder: enabled, min-ready = 0` — additional Coders may be created on demand for safely independent work.

`Judge: disabled` — governance remains a single logical participant unless explicitly redesigned.

Pool copies are horizontal siblings, not replacement generations. Their work MUST be safely partitioned so concurrent instances do not unknowingly own/mutate the same responsibility.

## Replacement cloning

Cloning is distinct from Elastic Agent Pool:

`Cloning = Agent (N) -> Agent (N+1)` — replace one existing lineage while preserving contextual continuity.

`Elastic Pool = Agent A + Agent B + ...` — add concurrent capacity while existing instances remain active.

Each configured Agent has a stable base name and replacement generations use monotonically increasing numeric generation metadata. `(cloning)` is applied only to the outgoing generation during replacement.

When both configured context-health signals are available, proactive cloning requires:

`compaction/equivalent count >= clone-after-compactions`

**AND**

`context utilization/pressure >= clone-at-context-utilization`

Context transfer must occur before the outgoing instance is locked whenever reliable context remains. Recovery cloning reconstructs contextual knowledge from available workflow evidence when proactive transfer was missed.

## Initialization validation

Candidate configuration MUST be validated against applicable Role, Workflow and Team rules before READY. This includes resolved Elastic Agent Pool policy.

## Role capabilities versus concrete commands

Reusable Roles describe conceptual capabilities, never concrete AI Command dependencies:

`Role capability -> Agent binding -> Team authorization -> Command/provider/harness`

## Communication and trust

Every runtime Agent inherits [`_common/communication.md`](_common/communication.md). Pool copies and clone replacements receive distinct runtime identities and must enter the authoritative roster before normal trusted communication.

## Team/runtime separation

Workflow defines static Team contract; runtime maintains dynamic roster, pool membership, assignments, generations and lifecycle state.

## Contextual knowledge

Contextual knowledge is task-relevant working information held in active Agent context. Proactive replacement transfers it directly to the successor. Recovery reconstructs it from authorized evidence when direct transfer is unavailable.

## Command authority

Concrete commands are never granted at reusable Role level. Workflow implementation binds capabilities and Team policy grants authorization.

## Human participant

Human is not an AI Agent but MUST be represented in workflow Team interaction modeling when applicable.

## Override rule

Reusable Role properties are defaults. Workflow/profile specialization may override explicitly but SHOULD NOT silently broaden authority, privacy access, command permissions, memory scope or pool capacity.

## Acceptance checklist

- [ ] Required Role properties declared.
- [ ] Agent resolves `elastic-pool-enabled` before READY.
- [ ] Enabled pool policy supplies any workflow-required capacity limits/defaults.
- [ ] Pool copies are used only for safely partitioned concurrent work.
- [ ] Elastic scaling is not confused with replacement cloning.
- [ ] Clone-policy thresholds resolved.
- [ ] Context transfer/recovery lifecycle supported.
- [ ] Initialization validated before activation.
- [ ] Team authorization remains authoritative.