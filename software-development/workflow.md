# Software Development Workflow

Reusable software-development workflow.

## Composition

This workflow realizes reusable Roles through [`agents.md`](agents.md) and coordinates them through [`team/`](team/README.md).

The team includes Workflow Strategist, Designer Reviewer, Coder, Deployer, Manager, Command Runner, UI Acceptance Tester and Judge, with Admin when configured.

## Lifecycle model

The Strategist provides durable workflow continuity. Workers are bounded/session-oriented unless explicitly configured otherwise. Designer Reviewer owns design intent and implementation-conformance review. Deployer owns mechanical deployment execution, not deployment/release strategy.

## Flows

- [`implementation`](flows/implementation.md)
- [`testing`](flows/testing.md)
- [`debugging`](flows/debugging.md)
- [`review`](flows/review.md)
- [`release`](flows/release.md)

## Prompt routing / use cases

| Example prompt / intent | Route type | Route target | Result / notes |
| --- | --- | --- | --- |
| “Let's design this.” / “How should we build this?” | agent | Designer Reviewer | Human-facing design discussion. |
| “Let's code this.” / “Implement this change.” | flow | [`implementation`](flows/implementation.md) | Designer Reviewer may delegate implementation to Coder. |
| “Run the tests.” / “Build this.” | agent | Command Runner | Bounded execution through configured commands/capabilities. |
| “Show me the diff.” / “What changed in Git?” | agent | Command Runner | Routes through `source-control`. |
| “Commit these changes.” / “Push these changes.” / “Create a branch.” | agent | Command Runner | Bounded `source-control` operation subject to authorization. |
| “Check the new ticket.” / “Create a ticket for this.” | agent | Manager | Manager owns ticket/work tracking. |
| “Review whether this follows the design.” | agent | Designer Reviewer | Design-conformance review. |
| “Why is this failing?” / “Debug this.” | flow | [`debugging`](flows/debugging.md) | Enter debugging/investigation flow. |
| “Deploy this branch to dev.” | agent | Deployer | Deploy supplied/resolved branch to `dev` using configured deployment capability. |
| “Deploy this branch to test.” | agent | Deployer | Deploy supplied/resolved branch to `test`; Deployer does not choose the branch/environment independently. |
| “Deploy the snapshot to dev.” | agent | Deployer | Deploy supplied/resolved snapshot artifact/version to `dev`. |
| “Deploy release 1.2.3 to test.” | agent | Deployer | Deploy explicit release/version to explicit environment, subject to gates/authorization. |
| “Deploy the current approved release.” | flow | [`release`](flows/release.md) | Release flow resolves/approves deployment intent, then routes bounded execution to Deployer. |
| “Let's release it.” | flow | [`release`](flows/release.md) | Enter release/readiness flow; Deployer executes only after required target/artifact/environment are resolved. |
| “What should we build first?” | strategist | Workflow Strategist | Workflow-level prioritization/strategy. |

These are semantic routing examples, not exact required phrases.

## Deployer invocation contract

Deployer is a specialized Software Development Worker. A caller/flow invoking it must provide or authoritatively resolve the deployment inputs before execution.

### Common context

Like other workflow Agents, Deployer operates within resolved runtime context:

`Profile -> Workflow -> Project -> Deployer`

Profile/runtime supplies provider credentials/configuration; Workflow supplies deployment semantics/policy; Project supplies repository/pipeline/environment configuration.

### Deployment inputs

| Input | Required | Meaning / examples |
| --- | --- | --- |
| Project/source | yes | Concrete software project/repository being deployed. |
| Environment/target | yes | `dev`, `test`, staging, production or another project-defined target. |
| Deployable selector | yes | What to deploy: branch, snapshot/artifact, release/version, commit or another project-defined deployable. |
| Branch | conditional | Required when deployable selector is a branch, e.g. `feature/foo`. |
| Release/version | conditional | Required when deploying a release, e.g. `1.2.3`. |
| Snapshot/artifact | conditional | Required when deploying a snapshot/build artifact. |
| Commit/ref | conditional | Used when project deployment is based on an exact source-control ref. |
| Pipeline/deployment implementation | runtime binding | Concrete GitHub/Bitbucket/GitLab/cloud/other capability resolved during Agent/workflow instantiation. |

Exactly one authoritative deployable must be resolved before Deployer performs mutation. If target or deployable is ambiguous, Deployer stops and returns the missing/ambiguous input rather than choosing strategically.

## Deployer capability binding

For Software Development, Deployer commonly needs both:

1. **source-control capability** — inspect/resolve authorized branches, commits, tags/releases and relevant source-control state;
2. **deployment/pipeline capability** — trigger/observe the configured deployment mechanism.

Concrete implementation is bound in [`agents.md`](agents.md). The source-control capability may resolve to the reusable `source-control` AI Command, while pipeline execution may be provided by GitHub, Bitbucket, GitLab, cloud-native deployment tooling, another AI Command or harness/runtime integration.

Binding a capability does not grant permission. Team command/capability policy remains authoritative.

## Connected commands

Commands are defined in the [AI Commands repository](https://github.com/starodubtsevconsulting/ai-commands).

| Command | Reference | Used by / purpose |
| --- | --- | --- |
| `source-control` | [AI Commands / source-control](https://github.com/starodubtsevconsulting/ai-commands/tree/main/source-control) | Source-control operations; Deployer may receive this binding for deployment source/ref resolution when authorized. |
| `ticket-tracker` | [AI Commands / ticket-tracker](https://github.com/starodubtsevconsulting/ai-commands/tree/main/ticket-tracker) | Manager keeps work/progress synchronized and provides durable recovery context. |

## Context and memory

Workflow/project facts, decisions and learned principles belong to durable Software Development memory through the strategic layer. Workers receive task-relevant contextual knowledge rather than owning permanent conversational memory by default.

Ticket/work tracking and durable artifacts provide recovery context when direct context transfer is unavailable.

## Runtime independence

This workflow defines responsibilities, routing and boundaries rather than concrete models/providers/harnesses. Runtime/profile configuration resolves `agents.md` requirements into actual Agent instances and command/provider integrations.

## Privacy boundary

Credentials, private repository configuration, ticket-system credentials, deployment credentials and organization-specific private data remain runtime/profile concerns.