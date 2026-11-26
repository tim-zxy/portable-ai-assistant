# 08 --- Desktop, Web UI and Interaction

## 1. UX principles

-   Conversation is the natural entry point.
-   Goal and Task are the work-management units.
-   Agent and resource details are available on demand.
-   Show important progress, risks and decisions without overwhelming
    users with tool logs.
-   UI is a view of authoritative server state.
-   Closing UI must not stop background work.

## 2. Navigation

### Work

-   Home
-   Conversations
-   Projects and Goals
-   Tasks
-   Inbox / Decisions

### Operations

-   Agent Execution
-   Memory and Knowledge
-   Automation
-   Usage and Resources

### Settings

-   Assistant
-   Runtime and Models
-   Skills/Tools/Extensions
-   Projects and Repositories
-   Integrations
-   Credentials and Authorization
-   System, Storage and Diagnostics

## 3. Home dashboard

Show:

-   Active goals.
-   Tasks currently executing.
-   Items requiring user action.
-   Important milestones and risks.
-   Recent accepted artifacts.
-   Runtime/service health.
-   Usage summary.

Do not show a misleading overall percentage if task completion is not
measurable.

## 4. Conversation

Support Chat, Task and Goal interaction modes, with automatic intent
recognition where safe.

Message types may include:

-   Text.
-   Goal and Plan cards.
-   Task and Run cards.
-   Interaction requests.
-   Progress summaries.
-   Artifact cards.
-   Error/blocker cards.

Cards must resolve current resource state rather than hard-code a stale
status into historical message text.

## 5. Conversation and Task separation

One Conversation may relate to several Goals/Tasks. A Task has its own
execution context. Different unrelated Tasks should not share native
Agent context by default.

The user can continue a Goal from another Conversation. Every new
instruction that changes work must be resolved to an explicit target.

## 6. Project workspace

Tabs:

-   Overview.
-   Repositories.
-   Goals.
-   Plans.
-   Tasks.
-   Knowledge.
-   Artifacts.
-   Activity.
-   Settings.

Repository graph should show only key relationships and distinguish
confirmed from inferred links.

## 7. Goal details

Show:

-   Original objective and constraints.
-   Current active plan revision.
-   Milestones and task DAG.
-   Current execution and next action.
-   Blockers and user decisions.
-   Accepted outputs and evidence.
-   Usage and resource summary.
-   Plan change history.

## 8. Task details

Tabs:

-   Overview.
-   Execution.
-   Dependencies.
-   Workspace.
-   Context.
-   Verification.
-   Artifacts.
-   History.

Show actual Runtime/model where known, native session references,
working directory, base/result commits, tool activity and evidence.

## 9. Agent execution center

Provide:

-   Runtime instances and health.
-   Sessions and active Runs.
-   Actual model and usage.
-   Recent progress and tool events.
-   Child Agents where supported.
-   Diff and artifact references.
-   Errors and recovery actions.

Users should not need to open the vendor UI for routine task
observation. Provide an optional native-runtime link where appropriate.

## 10. Memory center

Overview, personal memory, assistant experience, project/repository
knowledge, decisions, lessons and governance. Provide provenance and
controlled edit/archive/delete actions.

## 11. Inbox

Unify information requests, decisions, approvals, acceptance and
selected exception handling. Notification delivery is separate from
request state.

Support the same authoritative request from Desktop, Web and Feishu. Use
revision checks to prevent stale approvals.

## 12. Automation and notification

Automation supports time, event and condition triggers. Users may create
schedules in natural language, but the saved schedule must show scope,
timezone, trigger, action, permission and recovery policy.

Notifications are classified and aggregated. Do not send every tool call
to the user.

## 13. Usage center

Show model, project, Goal, Task, Run and role dimensions where data is
available. Distinguish complete, partial and unknown usage.

Model comparisons should use comparable task types and quality outcomes,
not token count alone.

## 14. Desktop lifecycle

-   Desktop window may hide to tray.
-   Assistant service continues independently.
-   Explicitly distinguish closing the UI from stopping the service.
-   User logout, sleep and shutdown limitations are visible.
-   Tray supports open, status, pending items, pause scheduling and safe
    service stop.

## 15. API and real-time updates

Use REST/OpenAPI and SSE. Fetch a consistent state snapshot with a
cursor, then subscribe from that cursor. On disconnect, resume from
cursor; if history is unavailable, reload the authoritative snapshot.

SSE is for presentation, not task execution. Background services must
continue without an open UI.

## 16. Frontend baseline

React, TypeScript, Vite, Tailwind CSS, shadcn/ui, React Router and
TanStack Query are proposed. Add graph/chart libraries only where
required.

Keep business state on the backend. UI state is for transient
interaction only.

## 17. Cloud UI evolution

Reuse the same business UI. Add team, shared assistant, Worker, quota
and enterprise administration pages only when cloud capabilities exist.
Hiding a menu is not an authorization boundary.
