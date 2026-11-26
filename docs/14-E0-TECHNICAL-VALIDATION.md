# 14 --- E0 Technical Validation Plan

**Timebox target:** 1--2 working days\
**Purpose:** eliminate blocking technical uncertainty before the first
vertical slice.

## 1. E0 rules

-   Inspect the actual development machine and existing repository
    first.
-   Use real supported Agent versions.
-   Record exact versions and commands.
-   Do not claim a capability from documentation alone.
-   Do not replace required Runtime tests with mocks.
-   Keep proofs small and disposable.
-   Record failures and alternatives.
-   Do not begin broad E1 implementation until shared contracts and
    blockers are resolved.

## 2. Validation work packages

### E0-01 --- OpenCode

Verify:

-   Installation and version discovery.
-   Native configuration/profile discovery without exposing secrets.
-   Server/SDK launch and connection options.
-   Model discovery and actual selected-model invocation.
-   Session creation and task execution.
-   Event/output observation.
-   Interrupt, reconciliation and process cleanup.
-   Working-directory control.

Deliverable: capability matrix with exact tested version and
limitations.

### E0-02 --- DeepSeek Harness

Verify:

-   Installed SDK/CLI and version.
-   Supported Profile and launch mode.
-   Model/provider discovery.
-   Real model invocation.
-   Session/run lifecycle.
-   Event observation and cleanup.
-   Actual working-directory behavior.

Deliverable: capability matrix and adapter recommendation.

### E0-03 --- Windows Desktop/background process

Verify:

-   Tauri 2 build and WebView2 requirements.
-   Desktop connects to assistantd.
-   Window close does not stop assistantd.
-   Tray/start/stop behavior.
-   Current-user startup approach.
-   Clean-machine installation feasibility.

Deliverable: minimal runnable POC and packaging findings.

### E0-04 --- SQLite

Verify:

-   Selected driver and native dependencies.
-   Transactions and concurrent access.
-   Migration strategy.
-   Backup snapshot and restore.
-   Crash/restart behavior.

Deliverable: minimal persistence POC.

### E0-05 --- Credential storage

Verify Windows Credential Manager access under current-user identity.

Test:

-   Store/retrieve through a trusted adapter.
-   No secret in ordinary database fields.
-   No secret in logs or error messages.
-   Revocation/deletion.
-   Document same-user process trust limitations.

Deliverable: credential provider POC and threat limitations.

### E0-06 --- Workspace

Verify:

-   Local repository discovery.
-   Clone or existing repository binding.
-   Git worktree create/remove.
-   Multiple worktrees.
-   Uncommitted user changes are preserved.
-   Working-directory mapping to both Runtimes.

Deliverable: workspace POC.

### E0-07 --- Local API and SSE

Verify:

-   Loopback binding.
-   Local authentication bootstrap.
-   Host/Origin validation.
-   CSRF protection.
-   SSE subscription authorization.
-   Cursor resume and snapshot reconciliation.
-   Desktop/WebView compatibility.

Deliverable: security and transport POC.

### E0-08 --- Autonomous execution security

Verify actual enforcement for:

-   File read/write scope.
-   Process launch and child processes.
-   Runtime tool permissions.
-   Network access where claimed.
-   Credential isolation.
-   Cancellation and revocation.
-   Behavior when required isolation is unavailable.

Deliverable: security capability matrix and explicit no-go cases.

## 3. E0 output files

-   `E0-TECHNICAL-VALIDATION-REPORT.md`
-   `RUNTIME-COMPATIBILITY.md`
-   `TECHNOLOGY-VERSION-MATRIX.md`
-   `SECURITY-GAP-REGISTER.md`
-   Updated ADR/TDR.
-   Updated `TASK_BOARD.md`.
-   Updated `CURRENT_STATUS.md`.
-   E1 work packages and dependency map.

## 4. Exit criteria

E0 may exit when:

-   Both target Runtimes have a verified integration path or a
    documented blocker.
-   Desktop/background architecture is viable.
-   SQLite and credential storage choices are validated.
-   Workspace isolation approach is demonstrated.
-   Local API security approach is demonstrated.
-   Shared domain and SPI contracts are ready.
-   No unresolved blocker prevents E1.

If a blocker exists, present the evidence, options, impact and
recommendation. Do not hide it to meet the timebox.
