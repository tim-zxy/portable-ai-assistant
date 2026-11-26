# 03 --- API, Event and Extension Contracts

**Status:** Contract baseline; exact TypeScript types and OpenAPI
schemas are created and validated during E0/E1.

## 1. API principles

-   REST for commands and resource queries.
-   OpenAPI as the published HTTP contract.
-   SSE for ordered UI updates and resumable event consumption.
-   Async commands return accepted status and resource references.
-   Business state transitions are explicit commands.
-   Server-side authorization is mandatory; UI visibility is not
    security.
-   Use resource revisions and idempotency keys for important writes.

Base path: `/api/v1`.

## 2. API resource groups

  Resource           Main responsibility
  ------------------ ----------------------------------------------------
  `/system`          health, version, capabilities, diagnostics
  `/assistants`      assistant definitions and instances
  `/conversations`   conversations and messages
  `/projects`        projects
  `/repositories`    repository bindings and relationships
  `/workspaces`      workspace preparation and status
  `/goals`           goal lifecycle
  `/plans`           plan revisions and activation
  `/tasks`           task query and lifecycle commands
  `/invocations`     logical delegations
  `/runs`            actual execution attempts
  `/runtimes`        discovery and runtime instances
  `/models`          model bindings and capability status
  `/memory`          memory query and governance
  `/contexts`        context pack inspection/provenance
  `/interactions`    information, decision, approval, acceptance
  `/schedules`       schedule lifecycle
  `/notifications`   notification and delivery
  `/usage`           token and resource usage
  `/integrations`    connector configuration and health
  `/credentials`     credential references, verification and revocation
  `/artifacts`       artifact metadata and controlled download
  `/events`          SSE event stream

## 3. Command semantics

Examples:

-   `POST /goals`
-   `POST /goals/{id}/pause`
-   `POST /goals/{id}/resume`
-   `POST /tasks/{id}/retry`
-   `POST /tasks/{id}/cancel`
-   `POST /runs/{id}/stop`
-   `POST /runs/{id}/input`
-   `POST /interactions/{id}/respond`
-   `POST /schedules/{id}/pause`

Do not allow clients to PATCH arbitrary Task or Goal state fields.

## 4. Async response

A command accepted for background work should return HTTP 202 with:

-   `commandId`
-   `resourceId`
-   `status: accepted`
-   `resourceVersion`

Acceptance does not mean execution or task completion.

## 5. Event envelope

Minimum fields:

``` typescript
interface AssistantEvent {
  id: string;
  schemaVersion: number;
  tenantId?: string;
  source: {
    type: string;
    instanceId: string;
    externalEventId?: string;
  };
  type: string;
  subjectRef?: string;
  actorRef?: string;
  occurredAt?: string;
  receivedAt: string;
  priority: "P0" | "P1" | "P2" | "P3" | "P4";
  correlationId?: string;
  causationId?: string;
  payload: unknown;
  evidenceRefs?: string[];
}
```

Payloads must have event-specific schemas. External actor and resource
identifiers are untrusted until resolved and authorized.

## 6. Runtime SPI

Runtime adapters own vendor-specific process/API/session behavior. They
do not own Goal or Task state.

Required conceptual capabilities:

-   `describe`
-   `discover`
-   `connect`
-   `launch`
-   `capabilities`
-   `health`
-   `createSession`
-   `startRun`
-   `observe`
-   `reconcile`
-   `close`

Optional capabilities include interrupt, append input, approval
interaction, native session recovery and detailed usage.

Capabilities must be discovered and verified. A manifest claim alone is
not proof.

## 7. Integration SPI

Common lifecycle:

-   `describe`
-   `configure`
-   `health`
-   `start`
-   `stop`

Specialized contracts:

-   `ChannelAdapter`
-   `EventSource`
-   `SCMProvider`
-   `CIProvider`
-   `KnowledgeProvider`

SCM and CI remain independent because one repository may use a different
CI provider.

## 8. Capability SPI

Support:

-   Agent Skills and declarative resources.
-   MCP client connections over stdio and Streamable HTTP.
-   PAA-native tools.
-   Vendor-native plugins through their own Runtime.

Do not treat an OpenCode plugin as a PAA extension automatically.

## 9. Environment SPI

Conceptual operations:

-   `prepare`
-   `inspect`
-   `capabilities`
-   `cleanup`

Initial implementations: local directory and Git worktree. Future
implementations: restricted local sandbox and remote sandbox.

## 10. Credential SPI

Credential adapters store and use secrets without exposing secret
material to the UI, model or untrusted extension.

Support references, status, scoped use and revocation. The
callback-based secret use boundary must be implemented carefully; do not
serialize secrets into ordinary application messages or logs.

## 11. Extension manifest

A manifest identifies:

-   Extension ID, version and type.
-   PAA API compatibility.
-   Runtime/protocol compatibility.
-   Entry point.
-   Capabilities and required permissions.
-   Configuration schema.
-   Dependencies and lifecycle.

Built-in adapters may be statically registered in 1.0. A general dynamic
plugin marketplace is out of scope.

## 12. Versioning

-   Version public contracts independently from product versions.
-   Add optional fields compatibly.
-   Do not silently change event meaning.
-   Breaking SPI changes require an ADR and migration path.
-   Runtime-specific compatibility must be tested against actual
    versions.
