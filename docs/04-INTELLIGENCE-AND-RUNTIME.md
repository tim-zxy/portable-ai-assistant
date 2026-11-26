# 04 --- Intelligence, Agent Runtime and Model Routing

## 1. Responsibility split

PAA decides what work is required and coordinates it. Runtime adapters
operate the selected Agent. Models perform reasoning within the supplied
task and permissions.

Do not let an Agent adapter become a second Supervisor or Task Engine.

## 2. Planning policy

Complex goals should use a strong qualified planning model. Planning is
a high-leverage decision point because poor decomposition creates
downstream rework.

Model selection must consider:

-   Task complexity and required reasoning.
-   Actual model availability and authorization.
-   Data classification and allowed provider.
-   Context window and required tools.
-   Latency, cost, quotas and budget.
-   Measured performance on comparable tasks.

Do not infer strength from model name alone. Maintain configurable
capability ratings and update them using evaluation evidence.

## 3. Planning workflow

1.  Parse the user's objective and constraints.
2.  Identify missing information and inspect available project
    knowledge.
3.  Ask only for essential missing information.
4.  Discover relevant repositories and capabilities.
5.  Build a task-specific context.
6.  Produce a plan with acceptance criteria and dependencies.
7.  Review the plan for completeness, feasibility and unnecessary work.
8.  Activate a versioned plan.
9.  Dispatch ready tasks.
10. Replan only when meaningful new evidence changes the plan.

Avoid repeated full replanning after every small event.

## 4. Deterministic and intelligent paths

Use deterministic Fast Path for:

-   State transitions.
-   Dependency unlocking.
-   Idempotency and event deduplication.
-   Quota/health state updates.
-   Schedule firing.
-   Cancellation and permission revocation.
-   Known retry and cleanup rules.

Use Supervisor/Planner reasoning for:

-   Ambiguous requirements.
-   Cross-repository impact.
-   Complex failures.
-   Model or Runtime substitution when trade-offs matter.
-   Significant plan changes.
-   User decision preparation.

Do not call a model for routine state updates.

## 5. Runtime discovery

Discover supported Agent installations using bounded mechanisms:

-   PATH and known executable locations.
-   Official installation metadata.
-   Supported native service/API discovery.
-   Native configuration and profile discovery.
-   User-specified custom paths.

Do not scan the entire disk or execute unknown binaries.

Distinguish:

-   Installed.
-   Running.
-   Connectable.
-   Launchable.
-   Configured.
-   Authenticated.
-   Verified usable.

The states are not interchangeable.

## 6. Runtime launch modes

Preferred order, subject to actual support:

1.  Native SDK/API.
2.  Headless or server mode.
3.  CLI. 
4.  TUI/Desktop for human interaction.

Do not automate TUI/Desktop using unreliable keyboard simulation as the
primary execution interface.

Support lifecycle policies:

-   `managed`: PAA starts and owns the managed instance.
-   `attached`: connect to an existing user-authorized instance.
-   `manual`: wait for user to start/connect.
-   `remote`: future remote Worker.

Do not silently adopt a user's active interactive session.

## 7. First Runtime integrations

OpenCode and DeepSeek Harness are required for Phase 1. Exact SDK,
Server, Profile and CLI behavior must be verified in E0 against the
installed versions.

The adapter must record which capabilities are genuinely supported,
including session creation, observation, interruption, reconciliation
and usage.

Do not claim feature parity between adapters if one lacks a capability.

## 8. Model discovery and routing

Discover models from official Runtime APIs and native effective
configuration first. Use read-only configuration parsing only when
necessary.

Distinguish:

-   Discovered.
-   Configured.
-   Authenticated/authorized.
-   Verified.
-   Temporarily unavailable.
-   Quota exhausted.
-   Unknown.

Do not test every discovered model at startup. Verify the selected
primary model and use lazy verification for alternatives.

Prefer native authentication and profile inheritance. Do not copy user
secrets into PAA.

When switching models, preserve task requirements and data policy.
Create a handoff summary if the new Runtime cannot reuse the native
session.

## 9. Sessions and concurrency

A Task has its own execution context by default. A native Session may be
reused within a Task when the Runtime supports it and the context
remains appropriate.

Unrelated tasks should not share native context or writable workspace.

An Agent may have multiple sessions, but the actual parallel capacity
must be measured and registered. Do not equate session count with safe
concurrent execution capacity.

## 10. Failure and recovery

-   Reconcile actual Runtime state before retry.
-   Treat uncertain external side effects as unknown until queried.
-   Preserve failed Run evidence.
-   Use bounded retry budgets.
-   Avoid retry loops for deterministic environment failures.
-   Switch models only to qualified and authorized alternatives.
-   If no suitable Runtime/model exists, place the task in a clear
    waiting state.

## 11. Usage

Collect actual input/output/cache/reasoning token fields where
available. Preserve `unknown` or `partial` when the Runtime cannot
provide complete usage. Never invent exact token values.

## 12. Multi-agent collaboration

The Supervisor owns delegation and handoff. Each delegated task has
explicit scope, expected output, acceptance criteria and resource
permissions.

Independent work may run in parallel. Shared interface or file conflicts
require coordination. Cross-Agent handoff uses a bounded Handoff Pack
containing task state, evidence, decisions, relevant context and
remaining work.
