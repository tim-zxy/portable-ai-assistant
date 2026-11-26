# E0 --- First Codex Startup Prompt

Use this prompt only after:

1.  The Git repository `portable-ai-assistant` has been created and
    initialized.
2.  This documentation package has been copied into the repository's
    `docs/` directory.
3.  The user has manually created the root `AGENTS.md` based on
    `docs/10-ENGINEERING-RULES.md`.
4.  A baseline documentation commit has been created.
5.  The development environment is ready.

------------------------------------------------------------------------

You are the Lead Codex for Portable AI Assistant.

Read the root `AGENTS.md` and the complete consolidated engineering
baseline in `docs/`.

This is a product implementation project. The objective is a complete
Windows personal assistant, not an architecture-only prototype.

## Execute E0 only

Use `docs/14-E0-TECHNICAL-VALIDATION.md` as the E0 work plan.

1.  Inspect the current machine, repository, toolchain and existing
    files.
2.  Identify supported OpenCode and DeepSeek Harness installations and
    actual versions.
3.  Validate real discovery, launch, model configuration, session,
    events, interruption and recovery paths.
4.  Validate Tauri Desktop and the independent assistantd process.
5.  Validate SQLite persistence, transactions and backup/restore.
6.  Validate Windows credential storage and secret handling.
7.  Validate Git worktree and multi-repository workspace behavior.
8.  Validate local API authentication, Origin/CSRF and SSE recovery.
9.  Assess the actual security boundary for autonomous execution.
10. Record all findings and evidence.

## Parallel work

Use independent Codex sessions or subagents where available. Coordinate
four logical workstreams:

-   A: Core and Control.
-   B: Runtime and Intelligence.
-   C: Workspace and Integration.
-   D: Product and Knowledge.

Do not serialize independent investigations. Do not create overlapping
file ownership. Freeze shared contracts before dependent implementation.

## Engineering discipline

-   Do not re-analyze settled design decisions without new evidence.
-   Do not add unnecessary dependencies or infrastructure.
-   Do not implement cloud infrastructure.
-   Do not expand Phase 1 scope.
-   Do not substitute mocks for mandatory real Runtime validation.
-   Do not create repetitive tests to inflate test counts.
-   Record meaningful temporary strategies in
    `ARCHITECTURE_EVOLUTION.md`.
-   Preserve user data and existing uncommitted work.
-   Do not expose credentials in prompts, logs or reports.

## Required E0 outputs

Produce:

1.  E0 technical validation report.
2.  OpenCode and Harness capability matrix.
3.  Technology/version matrix.
4.  Windows Desktop/background POC findings.
5.  SQLite and credential POC findings.
6.  Workspace and execution-security findings.
7.  ADR/TDR updates.
8.  A clear blocker/risk register.
9.  A dependency-aware E1 work package plan.
10. Updated `TASK_BOARD.md` and `CURRENT_STATUS.md`.

Timebox E0 to 1--2 working days. If a real blocker exists, document
evidence and options. Do not silently downgrade requirements.

When E0 exits successfully, prepare to start E1: the first real vertical
slice from user request to accepted Agent output and restart recovery.

Do not start broad E1 implementation before the E0 exit criteria are
satisfied.
