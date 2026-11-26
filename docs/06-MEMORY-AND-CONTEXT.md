# 06 --- Memory, Repository Knowledge and Context

## 1. Four memory layers

  -----------------------------------------------------------------------
  Layer                   Scope                   Examples
  ----------------------- ----------------------- -----------------------
  L1                      User and assistant      Stable preferences,
                          long-term memory        collaboration rules,
                                                  verified assistant
                                                  experience

  L2                      Project/repository      Project overview,
                          knowledge               architecture,
                                                  repository purpose, key
                                                  relations, ADRs

  L3                      Task and experience     Decisions, outcomes,
                          memory                  errors, fixes and
                                                  verification evidence

  L4                      Native Agent session    Vendor-native session
                          context                 state and short-lived
                                                  execution context
  -----------------------------------------------------------------------

Do not create a fifth layer. Specialized indexes and summaries are views
or storage mechanisms within these layers.

## 2. Authority and separation

-   Conversation history is not automatically long-term memory.
-   Task state is authoritative in Task Store.
-   Commit and CI facts come from their source systems.
-   Credential secrets never enter ordinary memory.
-   Native Agent sessions remain vendor-owned; PAA stores references and
    permitted metadata.
-   Memory summaries do not overwrite source evidence.

## 3. Memory record

A memory item should include:

-   ID and layer/scope.
-   Content or structured fact.
-   Source/evidence references.
-   Source revision or commit where relevant.
-   Confidence/status.
-   Created, verified and last-used times.
-   Access scope and provenance.
-   Supersession/expiry metadata.

## 4. Project knowledge overview

A project overview should summarize:

-   Purpose and business context.
-   Architecture and major components.
-   Repository list and key relationships.
-   Technology stack.
-   Current effective decisions.
-   Build/test/deployment approach.
-   Important verified lessons.
-   Knowledge gaps and stale areas.

Repository knowledge is concise. Detailed source-level understanding is
delegated to code intelligence or targeted Agent investigation.

## 5. Relationship and knowledge maintenance

-   Initial lightweight discovery.
-   Reuse existing knowledge on new tasks.
-   Refresh only when relevant sources change.
-   Mark affected facts stale when their source commit changes.
-   Revalidate important facts before high-impact use.
-   Retain provenance and historical decisions.
-   Do not recursively summarize summaries indefinitely.

## 6. Candidate and promotion lifecycle

Suggested lifecycle:

`OBSERVED -> CANDIDATE -> VALIDATED -> ACTIVE -> STALE / SUPERSEDED / ARCHIVED`

Routine facts may be promoted automatically when evidence is strong and
policy allows it. Important architectural constraints, disputed facts
and broad sharing changes may require user confirmation.

Raw task/run facts should be persisted independently of model-based
memory extraction so that an extraction failure does not lose execution
history.

## 7. Context Pack

A Context Pack is task-specific, bounded and permission-filtered.

It may include:

-   Goal and task requirements.
-   Relevant plan decisions.
-   Project/repository summaries.
-   Key repository relationships.
-   Relevant ADRs.
-   Prior task lessons.
-   Current workspace/commit information.
-   Necessary source evidence references.
-   Runtime and tool capabilities.

It must not include unrelated project history, all user memory or
credential secrets.

## 8. Context selection

1.  Resolve task and project scope.
2.  Apply authorization before retrieval.
3.  Retrieve relevant current facts.
4.  Prefer evidence with recent verified source versions.
5.  Resolve conflicts or mark them explicitly.
6.  Enforce token/context budget.
7.  Record provenance for important injected facts.

## 9. Storage and aging

Separate:

-   Physical storage capacity.
-   Active memory count.
-   Context token budget.

Use deduplication, structured summaries, aging, archive and source-based
refresh. Do not delete raw evidence merely because a summary exists.

Aging reduces retrieval priority; it does not automatically invalidate a
fact.

## 10. User memory center

Support:

-   Overview and category navigation.
-   Personal memory.
-   Assistant experience.
-   Project and repository knowledge.
-   Decisions and lessons.
-   Source inspection.
-   Edit, correct, invalidate, archive and delete where permitted.
-   Conflict and stale knowledge management.

## 11. Cloud permissions

Future scopes: Private, Assistant, Project, Repository, Workspace and
Organization.

Derived summaries, indexes and caches inherit source access
restrictions. If safe field-level separation is not possible, restrict
the entire derived object.

## 12. Memory evaluation

Measure retrieval usefulness, stale fact misuse, source traceability,
unnecessary context volume and reuse across tasks. Do not optimize only
for memory count.
