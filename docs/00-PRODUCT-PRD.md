# 00 --- Product Requirements Document

**Status:** Consolidated baseline\
**Release target:** PAA Personal Edition 1.0\
**Primary platform:** Windows x64

## 1. Product vision

Portable AI Assistant (PAA) is a persistent, user-controlled work
assistant that coordinates multiple Agent Runtimes and models to
complete real work.

The user provides goals, constraints and decisions. PAA handles the work
lifecycle: understand, clarify, plan, allocate, execute, observe,
recover, verify, report and learn.

PAA is not a replacement for every Agent. It is the coordination and
continuity layer above Agents.

## 2. Problems to solve

1.  Users must manually choose, start and switch Agents and models.
2.  Complex work is poorly decomposed and tracked across multiple tasks
    and repositories.
3.  Agent sessions, context and results are fragmented.
4.  Users must repeatedly ask for progress or restart stalled work.
5.  Model quotas, permissions and resource limits are difficult to
    manage.
6.  Workspaces, credentials and external integrations are configured
    separately.
7.  Agent work often lacks reliable verification and durable evidence.
8.  Knowledge is lost between conversations, tasks and projects.

## 3. Product principles

-   **Goal-centric:** the user's goal is the primary unit of
    responsibility.
-   **Agent-agnostic:** Runtime integrations are replaceable.
-   **Model-aware:** planning and execution use models appropriate to
    the task, permission and budget.
-   **Event-driven:** important changes trigger deterministic handling
    or intelligent decisions.
-   **Durable:** task state, events, approvals and results survive
    process restarts.
-   **Verifiable:** completion requires evidence, not merely an Agent's
    claim.
-   **User-controlled:** authorization is explicit, inspectable and
    revocable.
-   **Local-first:** the first release works without a cloud Control
    Plane.
-   **Extensible:** professional assistants and enterprise integrations
    are added through packs and adapters.
-   **Lean:** avoid duplicating code intelligence, model hosting or
    infrastructure already provided by specialist systems.

## 4. Target users and use cases

### 4.1 Primary user

An individual who uses one or more Agents for technical, research,
documentation, data or operational work and wants a persistent assistant
to coordinate that work.

### 4.2 First-release use cases

-   Delegate a complex coding goal involving one or more repositories.
-   Run multiple independent goals concurrently.
-   Use different models for planning, execution and review.
-   Discover installed Agents and configured models.
-   Prepare isolated Git worktrees for tasks.
-   Track Agent execution and token usage.
-   Recover after Agent or assistant restarts.
-   Receive Feishu updates and respond to decisions.
-   Track GitHub pull requests and GitHub Actions.
-   Schedule recurring checks and reports.
-   Maintain project, repository and task knowledge.
-   Revoke permissions from a unified authorization center.
-   Complete a non-coding research task using the same core.

## 5. Product surfaces

  -----------------------------------------------------------------------
  Surface                             Responsibility
  ----------------------------------- -----------------------------------
  Desktop                             Primary local user experience, tray
                                      and native notifications

  Web                                 Shared React workbench served by
                                      the local assistant

  CLI                                 Initialization, diagnostics,
                                      service management and automation

  Feishu                              Remote conversation, notifications,
                                      information requests and approvals

  Background service                  Persistent local control plane;
                                      independent of the UI window
  -----------------------------------------------------------------------

Closing Desktop or the browser must not stop a running assistant
service. Power-off, sleep and user logout limitations must be explicit.

## 6. Core capabilities

### 6.1 Assistant and conversations

-   One default assistant in the first release.
-   Multiple user-visible conversations.
-   Conversations may relate to multiple goals and tasks.
-   A goal may be continued from different conversations.
-   Internal Agent runs do not automatically create user-visible chat
    windows.

### 6.2 Goal and task management

-   Create and update goals.
-   Clarify missing requirements.
-   Create versioned plans.
-   Decompose plans into tasks and dependencies.
-   Run independent tasks concurrently.
-   Track blockers, approvals and deadlines.
-   Pause, resume, cancel, retry and verify tasks.
-   Preserve execution and decision history.

### 6.3 Agent and model management

-   Discover supported local Agent installations and configurations.
-   Distinguish installed, running, connectable, launchable and usable
    states.
-   Discover models from supported native APIs/configuration.
-   Verify model permission and availability when needed.
-   Prefer the strongest qualified model for complex planning.
-   Route execution and review according to capability, cost, policy and
    availability.
-   Launch managed Runtime instances on demand.
-   Do not silently take over user-owned TUI/Desktop sessions.

### 6.4 Workspace and repositories

-   Discover local Git repositories.
-   Bind repositories to projects.
-   Maintain a compact repository summary and key cross-repository
    relationships.
-   Prepare task-specific workspaces and Git worktrees.
-   Protect existing uncommitted user changes.
-   Support multi-repository task workspaces.
-   Keep repository relationships separate from task dependency graphs.

### 6.5 Memory and context

-   Four memory layers: personal/assistant, project/repository,
    task/experience, native Agent session context.
-   Evidence-backed knowledge with source and revision.
-   Incremental refresh and stale-state handling.
-   Context assembly scoped to the task and its permissions.
-   User-visible memory overview and editing/governance controls.
-   Never store credential secrets in ordinary memory.

### 6.6 Events and proactive work

-   Durable local event intake and processing.
-   Idempotency, retry, reconciliation and quarantine.
-   Fast deterministic handlers and intelligent Supervisor handling.
-   Goal liveness checks.
-   Persistent Schedule/Timer.
-   Notifications and user interactions.
-   Resume work automatically after valid user responses.

### 6.7 Integrations

First release: Feishu, local Git, GitHub and GitHub Actions. GitCode and
enterprise systems are later adapters.

### 6.8 Security

-   Unified Policy, Grant, Approval and Credential models.
-   User-visible authorization center.
-   Text/voice revocation with ambiguity clarification.
-   OS-backed local secret storage.
-   Controlled external operations.
-   Audit and redaction.
-   Explicit limitations of local same-user process isolation.

## 7. Non-goals for 1.0

-   Distributed cloud Control Plane.
-   Enterprise multi-tenancy and organization-wide sharing.
-   Thousands of cloud Workers.
-   Kubernetes orchestration platform.
-   Self-hosted model inference platform.
-   New code-indexing or code-graph engine.
-   General-purpose third-party plugin marketplace.
-   Full implementation of Claude, Codex or GitCode adapters.
-   Automatic execution of arbitrary untrusted code without a verified
    isolation boundary.

## 8. Product success criteria

PAA must demonstrate:

-   A complete user-to-delivery workflow.
-   Real OpenCode and DeepSeek Harness execution.
-   Multiple independent and related tasks.
-   Multi-repository coordination.
-   Recovery after service/Runtime interruption.
-   Accurate status and evidence.
-   Effective authorization and revocation.
-   At least one real non-coding workflow.
-   Measurable improvement against a human-plus-single-Agent baseline on
    representative complex tasks.

## 9. Release gates

A release candidate must pass the nine task categories in
`11-REAL-TASK-EVALUATION.md`, the E3 release gates, security checks and
clean Windows installation tests.
