# 09 --- Engineering Plan and Workstreams

## 1. Delivery strategy

Use four major phases: E0, E1, E2 and E3. Do not create many small
formal phases.

Target planning window is approximately 4--6 weeks, subject to E0
findings and actual engineering capacity. This is a planning target, not
a delivery guarantee.

## 2. E0 --- Baseline and technical validation

Target: 1--2 working days.

Validate:

-   OpenCode discovery, launch, model, session, events and recovery.
-   DeepSeek Harness SDK/Profile and real execution.
-   Windows Desktop/background service.
-   SQLite driver, transaction, backup and restore.
-   Windows credential provider.
-   Git worktree and multi-repository workspace.
-   Local API/SSE authentication.
-   Real autonomous execution security boundary.

Exit: no unresolved blocker that prevents the first vertical slice;
shared contracts and E1 backlog are ready.

## 3. E1 --- First real vertical slice

Target: first development week.

End-to-end flow:

User opens Desktop -\> creates Conversation/Goal -\> assistant plans -\>
discovers Runtime/model -\> prepares Workspace -\> launches Agent -\>
executes a real code task -\> verifies result -\> shows evidence -\>
persists state -\> restarts and recovers.

Do not wait for all product pages or integrations before delivering this
slice.

## 4. E2 --- Complete first-release capabilities

Build multi-goal/task scheduling, robust Runtime support,
multi-repository workspace, memory governance, Event Hub, Feishu,
GitHub/Actions, schedules, authorization center, usage UI, extension
contracts and a Research Assistant profile.

Integrate continuously. Do not defer all cross-module testing until the
end.

## 5. E3 --- Evaluation and release

Run the nine task categories in the evaluation plan. Complete security,
stability, clean-install, backup/restore, upgrade, SBOM and release
evidence.

E3 is primarily hardening and acceptance, not a major new feature phase.

## 6. Parallel workstreams

Lead Codex coordinates architecture, contracts, work assignment,
integration and release.

  -----------------------------------------------------------------------
  Stream                              Primary ownership
  ----------------------------------- -----------------------------------
  A --- Core & Control                Domain model, SQLite,
                                      Goal/Plan/Task, Event, Scheduler,
                                      Policy

  B --- Runtime & Intelligence        Agent discovery, Runtime adapters,
                                      model routing, Supervisor, usage

  C --- Workspace & Integration       Repositories, worktrees, Git,
                                      GitHub, CI, Feishu, credentials

  D --- Product & Knowledge           Desktop/Web, Conversation UI,
                                      Memory/Context, Assistant Pack
  -----------------------------------------------------------------------

Use independent branches/worktrees and explicit file ownership. Public
contracts are owned by the designated maintainer (Lead or delegated
contract owner).

## 7. Work package sizing

Target 0.5--2 working days per package. If a package exceeds three
working days without a runnable result, split it or escalate a blocker.

A package must state:

-   Owner.
-   Scope and non-goals.
-   Dependencies.
-   Files/modules owned.
-   Acceptance criteria.
-   Test evidence.
-   Integration target.

## 8. Integration cadence

-   Integrate small completed packages continuously.
-   Daily integration target.
-   At least one runnable product demonstration each week.
-   Avoid long-lived unmerged branches.
-   Re-run affected acceptance checks after contract or behavior
    changes.
-   Gate releases against a specific commit SHA.

## 9. E0 task list

  ID      Owner   Task
  ------- ------- ------------------------------------------
  E0-01   B       OpenCode real integration
  E0-02   B       Harness real integration
  E0-03   D       Tauri and assistantd POC
  E0-04   A       SQLite and persistence POC
  E0-05   C       Windows credential POC
  E0-06   C       Worktree/multi-repo POC
  E0-07   A/D     Local API and SSE security POC
  E0-08   A/B/C   Autonomous execution security assessment

## 10. Stage gates

### E1 exit

One complete real task can be delegated, executed, verified and
recovered after service restart.

### E2 exit

All Phase 1 Must capabilities are integrated and usable. At least one
non-coding workflow is exercised.

### E3 exit

Nine representative task categories evaluated; security and stability
gates passed; release package and handoff evidence complete.

## 11. Scope control

Must: required for first-release value or safety.

Should: useful but may follow after the first vertical slice.

Later: cloud-scale or enterprise capabilities.

Do not add new scope to E1 because it is technically interesting. Put it
in E2 or Later with rationale.
