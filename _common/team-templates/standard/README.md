# Standard Team Template

Reusable default team organization for workflows built around Human control, governance, strategy, coordination and disposable workers.

## Role types

The template operates on **role types**, not concrete workflow role names.

| Role type | Meaning |
| --- | --- |
| `Admin` | Human-controlled workflow administration/recovery. |
| `Judge` | Governance/compliance authority. |
| `Strategist` | Workflow-local strategy and durable direction. |
| `Manager` | Coordination, staffing and lifecycle authority. |
| `Worker` | Performs bounded domain work; normally disposable/replaceable. |

Concrete reusable roles specialize a role type, and runtime agents instantiate those roles:

`Role Type -> Role -> Agent`

Example:

`Worker -> Coder -> Coder 1`

A workflow using this template declares its concrete role-to-type bindings. It does not duplicate template policy unless it needs an explicit exception.

## Lifecycle defaults

[`lifecycle-matrix.csv`](lifecycle-matrix.csv) is authoritative for default lifecycle/control-plane relationships between role types.

The template intentionally distinguishes ordinary communication from lifecycle control. Permission to clone an agent does not imply permission to converse with that agent through an otherwise forbidden communication route.

## Workflow usage

A workflow using this template should declare:

1. template: `standard`;
2. each concrete workflow role/agent's role type;
3. only workflow-specific matrix overrides/exceptions.

If a future workflow requires fundamentally different organizational semantics, introduce another named team template instead of weakening `standard` with unrelated exceptions.