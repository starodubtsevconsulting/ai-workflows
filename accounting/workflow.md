# Accounting Workflow

Reusable accounting workflow and second concrete workflow in the catalog.

## Composition

Accounting selects reusable roles from `../_common/roles/` and specializes them for accounting work. The exact Accounting role set and capability boundaries will be defined as the workflow is migrated from private/runtime configuration.

A workflow-level Strategist may own persistent Accounting domain continuity. Execution roles remain ephemeral by default and receive only task-relevant projections of durable workflow memory.

## Runtime independence

The workflow does not bind itself to Hermes, OpenAI, a local model, or another harness/provider. Runtime/profile configuration realizes selected roles as concrete agents.

## Privacy

Client/profile bindings, credentials, private financial data and organization-specific configuration do not belong in this public workflow definition.
