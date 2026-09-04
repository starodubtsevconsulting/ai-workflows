# Proxy Role

Proxy is a reusable Role for an Agent whose visible workflow identity represents another Agent/model that performs the actual delegated intelligence/work outside the current harness/runtime boundary.

## Properties

- level: execution
- human-facing: false by default
- interaction-mode: reactive
- memory-class: `SESSION` by default
- lifecycle: ephemeral by default
- intelligence: low by default
- reasoning: low by default
- elastic-pool-enabled: configurable

## Purpose

Proxy exists when the current Agent host/client cannot directly instantiate or use the desired model/Agent, but can reach it through a bridge such as MCP.

Example:

```text
Designer Reviewer
  -> Proxy Coder
    -> MCP
      -> local Coder/model
    <- result
  <- result
```

From the workflow/team perspective, `Proxy Coder` is the visible Agent endpoint. The actual coding intelligence lives behind that endpoint.

## Agent specialization

Proxy is the Role. Concrete Agents are named for what they represent:

`Proxy -> Proxy Coder`

`Proxy -> Proxy Designer Reviewer`

`Proxy -> Proxy Strategist`

`Proxy -> Proxy Command Runner`

The concrete Agent configuration resolves the represented responsibility/target, bridge and provider/runtime details.

## Responsibilities

- represent the configured external/remote/local target Agent/model inside the workflow;
- accept authorized requests intended for that represented responsibility;
- forward task intent and required contextual knowledge through the configured bridge;
- wait while the target performs the work;
- return the target result/status to the caller with minimal transformation;
- preserve target/provenance information when operationally useful;
- surface target/bridge failure rather than silently substituting intelligence.

## Boundaries

Proxy does not itself own the domain intelligence it represents.

It MUST NOT independently redesign the delegated task, silently replace the configured target, broaden caller/target authority, expose provider credentials, or claim target work as independently produced by the Proxy Agent.

## Bridge/target binding

The Role does not hard-code MCP, local hosting, a model, endpoint or provider.

Concrete Agent instantiation resolves:

- represented responsibility/Agent kind;
- target Agent/model;
- bridge/transport capability;
- target/provider/runtime configuration;
- timeout/cancellation/status capabilities when available;
- context projection policy;
- workflow physical-scope projection and enforcement;
- Elastic Agent Pool policy if enabled.

Example:

`Proxy Coder -> MCP -> locally hosted Coder -> local model`

## Context and memory

Proxy forwards only contextual knowledge required for the delegated work. Persistent memory remains separately configured for the represented/target Agent or workflow; proxying does not itself imply durable memory.

Proxy receives the same resolved physical access scope as other Agents in its workflow. It validates explicit paths before
delegation and passes the bounded scope to the target session as initialization context. The bridge/target must also enforce
that boundary; prompt text alone is not a security control.

When a request falls outside scope, Proxy returns the sanitized scope reason from the boundary check. It does not invoke the
target and does not collapse that policy result into a generic bridge or handshake failure.

## Elastic capacity

Proxy Agents may enable Elastic Agent Pool when concurrent independent target conversations are useful. Pool copies remain proxies for independently assigned work; they are not replacement clones.

## Authorization

Proxy never lends authority. The caller must be authorized to communicate with the Proxy Agent and to exercise the represented capability according to Team/runtime policy.

## Naming

Concrete Agent names make proxying explicit:

`Proxy Coder`

Runtime naming follows the common convention:

`Proxy Coder (1) [feature-A]`

where `(1)` is replacement generation and `[feature-A]` is assignment.
