# 05 --- Workspace, Repository Relationships and Integrations

## 1. Repository versus Workspace

-   Repository: logical source-control identity.
-   Workspace: PAA-managed project/task working environment.
-   Working Directory: actual directory used by a Run.

One Repository may have multiple clones/worktrees. A Workspace may
contain multiple repositories.

## 2. Repository discovery: deliberately lightweight

PAA is not a code-indexing or code-understanding platform.

### L1 --- default lightweight discovery

-   Git identity and remote URL.
-   Branch and commit.
-   Repository root and top-level directories.
-   Package/build manifests.
-   Basic language/framework signals.
-   README and selected project metadata.

Prefer deterministic parsing. Do not invoke a model for every repository
discovery.

### L2 --- targeted key relationship discovery

Only when required:

-   API contracts.
-   OpenAPI/Proto references.
-   Shared packages.
-   Build and deployment dependencies.
-   Cross-repository configuration.
-   Targeted source files relevant to the current task.

### L3 --- external deep code understanding

Function-level call graphs, full semantic indexing, data flow and
complex impact analysis should be delegated to an existing code
intelligence service or specialized Agent. PAA consumes results and
evidence; it does not duplicate the index.

## 3. Persisted relationship data

Persist only:

-   Repository identity and project binding.
-   Short purpose/technology summary.
-   Important confirmed cross-repository relations.
-   Relation status and evidence references.
-   Source commit/revision and last verified time.

Do not persist full dependency trees, source summaries for every file,
or function-level graphs.

Relationship statuses: `CONFIRMED`, `INFERRED`, `DISPUTED`, `STALE`,
`SUPERSEDED`, `UNKNOWN`.

"No relationship found" is not proof of independence.

## 4. Task impact analysis

Repository Graph and Task DAG are separate.

For each task:

1.  Determine actual repositories and modules in scope.
2.  Reuse current repository summaries and relations.
3.  Check relevant shared contracts and resources.
4.  Investigate only material unknowns.
5.  Decide whether tasks can run in parallel, need ordering, or need
    conflict isolation.
6.  Record the baseline commit set.

Repository dependency does not automatically imply task serialization.

## 5. Workspace readiness

Before code execution, check:

-   Repository binding.
-   Local path or clone availability.
-   Authentication and read/write permission.
-   Current branch and commit.
-   Uncommitted user changes.
-   Existing workspace/task conflicts.
-   Required toolchain and dependencies.
-   Runtime working-directory mapping.

If the workspace is not ready, the assistant may still perform research
and planning. It must not begin code modification until the necessary
workspace and authorization are ready.

Never reset or overwrite uncommitted user changes without explicit
authorization.

## 6. Worktree policy

Prefer independent Git worktrees for concurrent code changes in the same
repository.

A worktree is a collaboration and file-conflict isolation mechanism, not
a security sandbox.

For multi-repository tasks, allocate one working directory per
repository and record the exact version set.

Read-only investigation and review should use read-only or immutable
snapshots where practical.

## 7. Initial integrations

### Local Git

-   Discover repositories.
-   Inspect status, branch and commit.
-   Create and manage authorized worktrees.
-   Preserve user changes.
-   Collect diffs and commits.

### GitHub

-   Repository, branch and commit queries.
-   Issues, pull requests and reviews.
-   Push/create operations only when authorized.
-   Prefer polling for local installations where inbound public webhooks
    are unavailable.
-   Deduplicate webhook deliveries when webhook mode is later used.

### GitHub Actions

-   Discover workflows.
-   Query runs and jobs.
-   Correlate run with exact commit.
-   Retrieve logs/artifacts under permission.
-   Trigger or rerun only where supported and authorized.

A successful CI result must match the intended commit or
multi-repository version set.

### Feishu

-   Use the supported enterprise app and official SDK/connection mode.
-   Receive messages and supported card interactions.
-   Support conversations, goal/task references, information requests,
    decisions, approvals and notifications.
-   Respond quickly to inbound callbacks; persist and process
    asynchronously.
-   Do not treat a chat ID as a Goal ID.
-   Resolve ambiguous replies before applying decisions.

## 8. Polling and reconciliation

Use adaptive polling for active resources and lower-frequency checks for
idle resources.

Persist cursors only after received facts are durably stored. Use
overlapping windows where provider semantics require it. Reconcile after
outages.

Do not have every task or Worker independently poll the same external
repository.

## 9. Future adapters

GitCode, GitLab, Jenkins and enterprise SCM/CI systems use the same
SCM/CI contracts. Their API differences belong in adapters, not core
Task logic.
