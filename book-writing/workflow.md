# Book Writing Workflow

## Purpose

Turn an idea, body of research, lived experience, or partial manuscript into a coherent book while preserving author intent, voice, factual integrity, continuity, and a deliberate path from concept to finished manuscript.

## Strategic layer

This workflow composes a Workflow Strategist from [`_common/roles/workflow/strategist.md`](../_common/roles/workflow/strategist.md). The Strategist uses `WORKFLOW_STRATEGIC` memory to retain the book's durable domain state across sessions: premise, audience, themes, structure, voice decisions, research conclusions, character/concept continuity, editorial decisions, rejected directions and the rationale behind them.

The Strategist knows the book and its writing strategy. It does not model or reshape the human author. WHY this book matters relative to other life/work goals, WHEN the author should work on it, and how it competes with other workflows belong to the Global Governor above this workflow.

## Candidate execution roles

A runtime may compose ephemeral agents from common or future reusable roles such as:

- Researcher — gather and verify bounded source material.
- Outliner / Architect — develop structure, chapter progression and argument/story shape.
- Writer — produce bounded manuscript sections in the established voice and constraints.
- Editor — improve clarity, coherence, pacing and consistency without silently changing intent.
- Reviewer — challenge continuity, unsupported claims, contradictions and quality gaps.

These are `SESSION` agents by default. They receive only the manuscript context and strategic-memory projection needed for their current task. Useful decisions, evidence and outcomes return to the Workflow Strategist rather than becoming permanent personal memory of an execution agent.

## Example flow

`intent/research -> book strategy -> outline -> chapter work -> review/edit -> continuity check -> manuscript`

The exact flow is not required to be linear. Research, outlining, drafting and revision may iterate while the Workflow Strategist preserves continuity and records consequential decisions.

## Memory boundary

Book/project memory belongs to the workflow, not to individual Writer or Editor agents. See [`_common/memory.md`](../_common/memory.md).

Typical durable memory includes:

- premise, audience and intended outcome;
- themes and constraints;
- outline and structural decisions;
- voice/style decisions;
- research findings and provenance;
- characters, concepts, terminology and continuity facts;
- editorial decisions and rationale;
- rejected approaches worth remembering;
- review findings and resulting lessons.

Raw manuscript/source files remain authoritative artifacts and should not be replaced by extracted memory summaries.

## Runtime boundary

The workflow declares roles, relationships, memory requirements and process constraints. The runtime/profile chooses concrete models, providers, harnesses, tools and memory implementations.