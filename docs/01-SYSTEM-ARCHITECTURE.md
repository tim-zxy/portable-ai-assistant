# 01 --- System Architecture

**Status:** Consolidated baseline; detailed implementation choices
subject to E0 validation.

## 1. Architecture decision

Phase 1 uses a **modular monolith** in a local background process
(`assistantd`), with a shared React application hosted by Tauri Desktop
and local Web.

Do not introduce microservices or distributed infrastructure in Phase 1.

## 2. Logical architecture

### 2.1 Presentation

-   Desktop shell: Tauri 2.
-   Shared UI: React + TypeScript + Vite.
-   Web access: authenticated local application endpoint.
-   CLI: service lifecycle, discovery, diagnostics and backup/restore.
-   Feishu: a channel adapter that invokes the same application
    services.

### 2.2 Application and domain modules

-   Assistant and Conversation.
-   Project and Repository.
-   Goal and Plan.
-   Task Engine and dependency graph.
-   Invocation and Run management.
-   Supervisor and Planner.
-   Scheduler and resource admission.
-   Runtime and Model Registry.
-   Workspace Manager.
-   Memory and Context.
-   Event Hub and Watchdog.
-   Interaction and Notification.
-   Policy and Authorization.
-   Credential Manager.
-   Integration and Extension Registry.
-   Artifact and Usage.

### 2.3 Infrastructure

-   SQLite for authoritative local business state.
-   Filesystem for artifacts, logs and workspace data.
-   Windows Credential Manager as the preferred local secret provider.
-   Process host for managed Runtime lifecycle.
-   Durable local Inbox/Outbox and schedule occurrences.
-   REST/OpenAPI for commands and queries.
-   SSE for UI updates.

## 3. Dependency direction

Presentation calls application services. Application services use domain
services and ports. Adapters implement ports. Domain modules must not
import UI code or vendor-specific Runtime implementations.

Preferred direction:

`UI / Channel -> Application Services -> Domain Contracts -> Adapters / Infrastructure`

Runtime adapters must not directly update Goal or Task state. They
report observations and results; the Task Engine decides state
transitions.

## 4. Core authority boundaries

  Fact                                       Authoritative owner
  ------------------------------------------ ------------------------
  User messages                              Conversation Store
  Goal and plan version                      Goal/Plan services
  Task status and dependencies               Task Engine
  Runtime process and native session state   Runtime Manager
  Actual execution attempt                   Run Store
  Model binding and verification             Model Registry
  User authorization                         Authorization Registry
  Secret material                            Credential Provider
  Repository identity and binding            Repository Registry
  Long-term knowledge                        Memory Store
  Event receipt and processing               Event Hub
  Schedule and occurrence                    Timer Engine
  Artifact metadata                          Artifact Store

No second module may create a competing authoritative state for the same
fact.

## 5. Local deployment topology

A current-user `assistantd` process owns the local control plane.
Desktop/Web are clients and may close independently. Agent processes are
launched or connected on demand.

Default listener is loopback only. The service must use authenticated
local access and must not expose its management API to the LAN by
default.

The assistant service may run under the current Windows user. Do not
default to LocalSystem or administrator privileges.

## 6. Extension boundaries

Keep these extension categories distinct:

1.  Runtime Extension --- Agent process, session and run control.
2.  Integration Extension --- IM, SCM, CI and external services.
3.  Capability Extension --- Skills, Tools, MCP and native plugins.
4.  Environment Extension --- directories, worktrees and future
    sandboxes.
5.  Credential Provider --- OS vault, native Agent credentials and
    future enterprise vault.

These are contracts, not a requirement to build a dynamic plugin
marketplace in 1.0. Built-in adapters may be statically registered.

## 7. Cloud evolution

Cloud deployment moves the complete Control Plane to the cloud:
Assistant state, Goal/Task, Memory, Event Hub, Scheduler, Policy and
identity.

Workers execute on local devices, cloud sandboxes or enterprise secure
nodes. A local Worker reports capabilities and accepts authorized
execution requests; it does not own a second global Supervisor or Task
Scheduler.

Cloud architecture may later use:

-   PostgreSQL for authoritative state.
-   Redis for bounded hot caches.
-   Durable message infrastructure for event transport.
-   Object storage for artifacts.
-   Enterprise IAM and Vault.
-   Cell-based partitioning for scale.

These are future implementations, not Phase 1 dependencies.

## 8. Technology baseline

  Area                 Proposed choice                   Status
  -------------------- --------------------------------- ---------------------------------
  Language             TypeScript                        Proposed
  Runtime              Node.js                           Validate version in E0
  Backend              Fastify                           Proposed
  UI                   React + Vite                      Proposed
  Desktop              Tauri 2                           Validate packaging and IPC
  API                  REST + OpenAPI                    Proposed
  Live updates         SSE                               Proposed
  Local DB             SQLite                            Validate driver and concurrency
  UI components        Tailwind CSS + shadcn/ui          Proposed
  Tests                Vitest + Playwright               Proposed
  Package management   One lockfile and workspace tool   Freeze in E0

## 9. Architecture constraints

-   No microservices in Phase 1.
-   No Redis/MQ/Kubernetes in Phase 1.
-   No self-built code indexing service.
-   No full dependency graph persistence.
-   No permanent Agent process per logical Assistant.
-   No requirement that the UI remain open for tasks to progress.
-   No cloud migration work before the local product is proven.
-   Add abstraction only at real replacement boundaries.
