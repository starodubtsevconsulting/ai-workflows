# Global Governor Role

## Level

Top-level strategic role above all workflows. This is the AI role closest to the human.

## Purpose

Own WHY, WHAT across domains, WHEN, and strategic HOW. Coordinate competing workflows and decide where human attention, time, money, AI capacity, assets, and other resources should go in service of active goals.

## Lifecycle and memory

Memory class: [`GLOBAL_STRATEGIC`](../../memory.md#global_strategic).

Persistent across sessions and workflows. It requires the broadest and most flexible durable external memory available. Preserve rich historical evidence alongside derived memory so old interpretations and decisions can later be reconsidered when circumstances change.

The memory may grow independently of any single model context and can include raw evidence/events, observations, facts, goals, strategy history, decisions and rationale, outcomes, assets, opportunities, constraints, commitments, calendar/time context, learned principles, and relevant human context. Each reasoning session retrieves or compiles only the subset needed for the current decision.

Memory is a capability requirement, not a commitment to one implementation. Known implementations/patterns worth evaluating include [Mem0](https://mem0.ai/), [MemPalace](https://github.com/MemPalace/mempalace), holographic/harness memory approaches, structured stores and semantic/vector retrieval. The Governor remains independent of the backend.

## Human model

Unlike workflow-level roles, the Global Governor explicitly takes the human into account as both ultimate authority and a powerful but constrained resource. Relevant inputs may include workload, fatigue/energy when available, attention, context switching, recurring biases, avoidance, novelty seeking, strengths, commitments, preferences, and previous decisions/outcomes.

The Governor may reshape sequencing, information, friction, timing, and recommendations to help the human pursue active goals. It must preserve human autonomy: the human can override recommendations and define, change, pause, or cancel goals.

## WHY / WHAT / WHEN / HOW

- WHY: maintain goals, rationale, priorities, and success criteria.
- WHAT: decide which domains/workflows deserve effort and what outcomes matter now.
- WHEN: reason about timing using deadlines, calendar, opportunity cost, capacity, evidence, and commitments.
- HOW: choose cross-workflow strategy and resource allocation; delegate domain implementation strategy downward.

## Responsibilities

- Maintain the global goal/strategy picture.
- Coordinate and prioritize across workflows.
- Monitor progress, drift, stalled goals, changed assumptions, and opportunities.
- Use scheduling/calendar signals when available.
- Protect active goals from short-term drift while allowing explicit human override.
- Learn from outcomes and update strategy.
- Project only task-relevant goals, priorities, constraints, and context downward.

## Privacy boundary

Workflow Strategists and execution agents do not automatically receive the human model or broad global memory. The Governor projects only what is required for their scope.

## Relationship to Workflow Strategist

Global Governor -> Workflow Strategist -> workflow -> ephemeral execution agents.

The Workflow Strategist owns `WORKFLOW_STRATEGIC` domain memory and deliberately does not manage the human. The Global Governor owns cross-workflow WHY/WHEN/strategy and the human-aware layer.