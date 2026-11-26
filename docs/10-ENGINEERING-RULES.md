# 10 --- Engineering Rules and Codex Operating Standard

This document is the source for the root `AGENTS.md` that the user will
create manually. Do not create or overwrite the user's root AGENTS.md
without instruction.

## 1. Product mission

Build a complete, independently usable Windows personal AI assistant.
This is an implementation project, not an architecture-only exercise.

## 2. Authoritative baseline

Follow this priority:

1.  Explicit user-confirmed decisions.
2.  Consolidated engineering baseline and approved ADRs.
3.  Phase scope and task acceptance criteria.
4.  Existing implementation and public contracts.
5.  Agent assumptions.

If a genuine conflict exists, document it and propose a decision. Do not
silently revive superseded proposals.

## 3. Work organization

-   Lead Codex coordinates four independent workstreams.
-   Freeze shared contracts before dependent implementation.
-   Use separate branches/worktrees.
-   Assign clear file ownership.
-   Integrate small work packages continuously.
-   Avoid long-lived unmerged branches.
-   Do not make one session implement every module serially.
-   Do not create excessive parallel workers that increase integration
    overhead.

## 4. Simplicity and defensive programming

Defend genuine boundaries: external input, database concurrency,
authorization, credentials, filesystem paths, process lifecycle, network
calls, state transitions and uncertain side effects.

Do not add speculative abstractions, duplicate validation, redundant
try/catch blocks, generic factories with one current implementation,
unneeded fallback paths or compatibility branches for hypothetical
future versions.

Every significant abstraction must have a current use case or an
approved extension boundary.

Prefer simple, readable, maintainable code.

## 5. Dependencies

Do not introduce a new infrastructure dependency without documenting:

-   Current problem.
-   Why existing tools are insufficient.
-   Operational cost.
-   Security and licensing implications.
-   Migration impact.

No microservices, Redis, MQ, Kubernetes, vector database or plugin
marketplace in Phase 1 without an ADR.

## 6. Testing

Prioritize:

1.  Domain invariants and state transitions.
2.  Database and integration correctness.
3.  Real Runtime compatibility.
4.  End-to-end workflows.
5.  Security and recovery.

Do not use mocks to claim mandatory real Runtime capabilities. Do not
create tests only to inflate test counts.

Record exact commands, environment, commit and results. Unknown or
unavailable external services are blockers/limitations, not passes.

## 7. Failure handling

Inspect existing evidence before rerunning. Avoid repeating a
deterministic failure. Do not weaken acceptance criteria to make CI
green. Preserve failure artifacts and classify environment, product and
integration failures separately.

## 8. Decision control

Autonomously make ordinary local implementation decisions within the
approved design.

Request review before:

-   Breaking public contracts.
-   Changing authoritative data ownership.
-   Changing security boundaries.
-   Adding major infrastructure.
-   Performing destructive/irreversible operations.
-   Expanding Phase 1 scope.

Do not ask the user to approve routine coding details.

## 9. Temporary solutions

Register meaningful temporary implementations in
`ARCHITECTURE_EVOLUTION.md` with current solution, reason, limitations,
target, trigger, exit criteria, owner and status.

A normal unfinished UI item is a task, not automatically a TDR.

## 10. Progress records

Maintain:

-   `CURRENT_STATUS.md`
-   `TASK_BOARD.md`
-   `DECISION_LOG.md`
-   `ARCHITECTURE_EVOLUTION.md`
-   `TEST_EVIDENCE.md`

Update after meaningful integration, milestones, blockers and phase
transitions.

## 11. Completion definition

A task is not complete merely because code was written. It must satisfy
acceptance criteria, pass relevant checks and provide evidence. A
product capability is not complete merely because a page or interface
exists.
