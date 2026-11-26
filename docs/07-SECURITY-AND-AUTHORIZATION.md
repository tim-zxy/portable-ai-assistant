# 07 --- Security, Authorization and Credentials

## 1. Security model

PAA can execute code and operate external systems. Security is a core
product requirement, not a post-release enhancement.

Separate:

-   Policy: what is allowed or denied.
-   Grant: what a user/admin has authorized.
-   Approval: approval for a specific operation and version.
-   Credential: secret material used to authenticate.

## 2. Authorization scopes

-   User
-   Assistant
-   Project
-   Repository
-   Goal
-   Task
-   Operation

Effective permission is the restrictive intersection of applicable
system, user, assistant, project, task, Runtime and external-provider
constraints.

A connected account does not automatically grant all business
operations.

## 3. Authorization registry

Persist grants with:

-   Owner and assistant.
-   Scope type and ID.
-   Actions and resources.
-   Source of grant.
-   Status and validity.
-   Expiry and revocation time.
-   Policy revision.
-   Audit references.

Approval must bind to a specific operation, resource, parameters and
relevant revision. An approval for an old plan must not silently
authorize a changed plan.

## 4. Unified authorization center

Phase 1 must allow users to:

-   View effective grants.
-   Filter by project, repository, assistant and provider.
-   Inspect source, scope, expiry and recent use.
-   Revoke individual or grouped grants.
-   Review history.

Support natural-language or voice revocation. If scope is ambiguous,
clarify. Revocation itself must not require approval.

On revocation:

1.  Persist the new grant/policy revision.
2.  Block new matching operations immediately.
3.  Invalidate authorization caches.
4.  Notify relevant Runtime/Worker.
5.  Inspect active runs and uncertain external side effects.
6.  Reconcile already-issued operations.
7.  Update UI and audit.

Revocation cannot undo an already-completed external side effect.

## 5. Credential separation

Configuration stores credential references, not secret values. Memory,
Context, logs and normal API responses must never contain secret
material.

Preferred Phase 1 providers:

-   Windows Credential Manager.
-   Native Agent authentication.
-   Git Credential Manager or existing SSH configuration.
-   A current-user DPAPI fallback only if justified and validated.

Do not copy all existing Agent, Git or model credentials into PAA.

## 6. Credential use

Use a trusted Connector/Credential Broker boundary. Agents should
request business operations; they should not retrieve arbitrary tokens.

Avoid secrets in:

-   Command-line arguments.
-   Model prompts.
-   Conversation messages.
-   Environment inheritance.
-   Logs and crash reports.
-   Artifacts and diagnostics.
-   Normal backups.

If a Runtime must use native credentials, document the actual trust
boundary. Same-user local processes may have access beyond logical PAA
controls; encryption at rest does not isolate malicious same-user code.

## 7. Execution trust levels

Use capability-based admission rather than assuming a label alone
guarantees safety.

-   Trusted local: current-user environment, native controls, no strong
    OS isolation guarantee.
-   Restricted execution: verified file/process/network constraints.
-   Isolated sandbox: separate VM or trusted sandbox for untrusted
    workloads.

Git worktrees are not sandboxes. PAA policy is not an OS security
boundary. If a task requires isolation that the environment cannot
enforce, fail closed for that operation.

## 8. Threats

-   Prompt injection in source, README, issue, PR, CI logs or web
    content.
-   Malicious Skill, MCP server or plugin.
-   Cross-project file access.
-   Credential leakage.
-   Unauthorized external side effects.
-   Stale approval.
-   Duplicate event/operation.
-   Local API abuse from browser origins.
-   Tenant and memory leakage in future cloud.

Treat external content as untrusted data, not privileged instructions.

## 9. Local API and Desktop

-   Loopback binding by default.
-   Authenticated local sessions.
-   Host/Origin validation.
-   CSRF protection for state-changing operations.
-   Authorized SSE subscriptions.
-   Artifact-ID-based file access with path containment.
-   Strict Tauri IPC capabilities.
-   No arbitrary shell API exposed to the UI.
-   Avoid permanent tokens in URLs or browser storage.

Exact Tauri/WebView authentication design is an E0 validation item.

## 10. Audit

Record actor, scope, decision, policy revision, operation, resource,
result and correlation IDs. Do not log secret values or private model
reasoning.

## 11. Cloud evolution

Future cloud requires tenant/user/project/repository authorization,
enterprise IAM/SSO, Vault/KMS, temporary credentials, worker identity,
network policy, data classification and audit.

Database row-level security is defense in depth, not the only
authorization mechanism.
