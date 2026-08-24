# AI Workflows

```mermaid
flowchart TD
  Actor["Actor: organization or user"]
  Actor --> Profile["Profile: policy and configuration"]
  Profile --> Workflow["Workflow: business or work process"]
  Workflow --> Commands["AI Commands: reusable executable skills"]
  Commands --> Outcome["Outcome: coordinated AI-assisted work"]
```

Reusable workflow definitions for coordinating AI-assisted work.

## Status: TODO

```mermaid
flowchart TD
  Actor["Actor: public AI Workflows repository"]
  Actor --> Today["Today: README and design placeholder only"]
  Today --> Review["Prepare portable workflows in the private source"]
  Review --> Later["Publish approved workflows one at a time"]
  Later --> Outcome["Outcome: future reusable workflow catalog"]
```

This repository is intentionally a placeholder. No workflow definitions,
profiles, agents, runtime implementation, project bindings, configuration, or
credentials are published here yet.

## Relationship to AI Commands

```mermaid
flowchart TD
  Actor["Actor: workflow"]
  Actor --> Process["Define a reusable business or work process"]
  Process --> Select["Select and coordinate bounded capabilities"]
  Select --> Commands["Use public AI Commands when applicable"]
  Commands --> Outcome["Outcome: workflow behavior with reusable command contracts"]
```

Workflows describe how work is coordinated. Commands describe reusable
capabilities that may be selected by a workflow. The public command catalog is
available at [AI Commands](https://github.com/starodubtsevconsulting/ai-commands).

The two repositories are intentionally separate: a command can be useful in
many workflows, and a workflow can select multiple commands without owning
their implementations.

## Workflow concept

```mermaid
flowchart TD
  Actor["Actor: profile selects a workflow"]
  Actor --> Context["Supply organization, policy, and project context"]
  Context --> Steps["Workflow coordinates decisions, commands, and optional agents"]
  Steps --> Evidence["Collect bounded outputs and completion evidence"]
  Evidence --> Outcome["Outcome: repeatable process independent of one profile"]
```

A workflow represents a reusable business or work process. It should remain
independent of one organization, client, profile, project, AI product, runtime,
or hosting model. External context supplies the configuration and sources needed
for a particular use.

## Planned portable shape

```mermaid
flowchart TD
  Actor["Actor: future workflow contribution"]
  Actor --> Contract["Required workflow Markdown contract"]
  Contract --> Optional["Optional agents, guides, tests, adapters, runtime, or UI"]
  Optional --> Validate["Validate portability and visual-first documentation"]
  Validate --> Outcome["Outcome: self-contained reusable workflow folder"]
```

The expected minimum is one human-readable workflow contract. Agent teams,
guides, acceptance scenarios, machine-readable manifests, adapters, executable
runtime code, and visual applications will remain optional and workflow-owned.
The final public template will be added when the first workflow is prepared for
review.

## Publication boundary

```mermaid
flowchart TD
  Actor["Actor: candidate workflow"]
  Actor --> Private["Develop and validate in the private canonical source"]
  Private --> Sanitize["Remove profiles, clients, credentials, paths, and private integrations"]
  Sanitize --> Review["Open an explicit public review"]
  Review --> Outcome["Outcome: curated public workflow"]
```

Future workflows will be published only after portability, documentation,
privacy, and security review. Until then, this README is the complete public
repository content.
