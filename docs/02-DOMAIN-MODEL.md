# 02 --- Domain Model and State Ownership

## 1. Core entity hierarchy

`User -> Assistant -> Conversation -> Goal -> Plan Revision -> Task DAG -> Invocation -> Run -> Native Session / Evidence / Artifact`

This is a conceptual relationship, not a strict one-to-many database
hierarchy. Conversations and goals can be many-to-many linked.

## 2. Main entities

  ----------------------------------------------------------------------------
  Entity                  Meaning                      Authority
  ----------------------- ---------------------------- -----------------------
  User                    Local user in Phase 1;       Identity
                          tenant user later            

  AssistantDefinition     Reusable assistant profile   Assistant Registry

  AssistantInstance       User's configured assistant  Assistant Store

  Conversation            User-visible dialogue thread Conversation Store

  Message                 User/assistant/tool-facing   Conversation Store
                          message                      

  Project                 Logical work grouping        Project Store

  Repository              Logical source repository    Repository Registry
                          identity                     

  RepositoryBinding       External/local repository    Repository Registry
                          mapping                      

  Workspace               Project/task working         Workspace Manager
                          environment                  

  Goal                    Persistent user objective    Goal Store

  PlanRevision            Versioned plan               Plan Store

  Task                    Independently executable     Task Engine
                          work unit                    

  TaskDependency          Directed dependency edge     Task Engine

  Invocation              Logical delegation to a      Execution Manager
                          Runtime/Agent                

  Run                     One actual execution attempt Run Store

  NativeSession           Vendor-native                Runtime Adapter
                          conversation/session         
                          reference                    

  InteractionRequest      Information, decision,       Interaction Service
                          approval or acceptance       
                          request                      

  AuthorizationGrant      Durable permission grant     Authorization Registry

  Approval                Approval for a specific      Approval Store
                          operation/version            

  CredentialRef           Reference to secret material Credential Manager

  MemoryRecord            Long-term knowledge item     Memory Store

  Event                   Durable fact received or     Event Hub
                          generated                    

  Schedule                Time/event/condition trigger Timer Engine

  Artifact                File or deliverable with     Artifact Store
                          metadata                     

  UsageRecord             Actual model/resource        Usage Collector
                          consumption                  
  ----------------------------------------------------------------------------

## 3. Conversation and work relationship

-   An Assistant can have many Conversations.
-   A Conversation can relate to multiple Goals and Tasks.
-   A Goal can be continued from multiple Conversations.
-   A Task does not require its own visible Conversation.
-   Internal Agent execution messages are not automatically user-visible
    conversations.
-   Every user instruction that changes work must be associated with an
    unambiguous target or clarified.

## 4. Goal and Plan

A Goal includes:

-   User intent and normalized objective.
-   Success criteria.
-   Constraints and non-goals.
-   Project and resource scope.
-   Authorization references.
-   Current Plan Revision.
-   Overall lifecycle state.

A Goal may have multiple Plan Revisions. Only one revision is active at
a time. A revision change records reason, affected tasks and invalidated
evidence.

## 5. Task

A Task should be small enough to execute and verify independently.

Minimum fields:

-   ID and Goal ID.
-   Plan Revision.
-   Description and acceptance criteria.
-   Required capabilities.
-   Repository and workspace scope.
-   Dependencies.
-   Priority and budget.
-   State and state revision.
-   Assigned Runtime/role.
-   Invocation references.
-   Evidence and Artifact references.

The Task DAG must be acyclic. Repository relationship graphs may contain
cycles and must not be copied directly into the Task DAG.

## 6. Invocation and Run

-   Invocation represents a logical delegation.
-   Run represents one actual attempt.
-   A single Invocation may have multiple Runs due to retry, model
    switch or recovery.
-   Every Run records actual Runtime, model if available, launch mode,
    Workspace, start/end, status, usage completeness and evidence.
-   A Run's failure does not automatically mean the Task is failed; the
    Task Engine applies retry/replan policy.

## 7. Suggested lifecycle states

### Goal

`DRAFT -> ACTIVE -> WAITING -> COMPLETED`

Additional terminal/administrative states: `PAUSED`, `CANCELLED`,
`FAILED`, `ARCHIVED`.

### Task

`DRAFT -> BLOCKED | READY -> DISPATCHED -> RUNNING -> VERIFYING -> SUCCEEDED`

Alternative states: `WAITING_USER`, `WAITING_EXTERNAL`, `RETRY_WAIT`,
`FAILED`, `CANCELLED`, `SKIPPED`.

State changes must be explicit commands handled by Task Engine, not
arbitrary database patches.

### Run

`CREATED -> STARTING -> RUNNING -> COMPLETED | FAILED | INTERRUPTED | CANCELLED | UNKNOWN`

An `UNKNOWN` or uncertain external side effect requires reconciliation
before retry.

### Interaction Request

`CREATED -> PENDING -> RESPONDED -> VALIDATING -> RESOLVED`

Other states: `EXPIRED`, `CANCELLED`, `INVALIDATED`.

Notification delivery status is separate from Interaction status.

## 8. Repository relationship model

Persist only high-value facts:

-   Repository identity and project binding.
-   Purpose and major technology.
-   Confirmed key relationships.
-   Relationship type and confidence/status.
-   Evidence reference and source commit/revision.
-   Last verified time.

Relationship statuses: `CONFIRMED`, `INFERRED`, `DISPUTED`, `STALE`,
`SUPERSEDED`, `UNKNOWN`.

Do not persist full source trees, function call graphs or duplicated
code indexes.

## 9. Multi-repository task baseline

For cross-repository integration, record a version set:

-   Repository identity.
-   Base commit.
-   Working branch/worktree.
-   Result commit.
-   Contract/version references.
-   Verification evidence.

A multi-repository delivery is not atomic merely because the Task is
marked complete. Integration verification must identify the exact
version set tested.

## 10. Data consistency

-   Use transactions for state changes and corresponding Outbox events.
-   Use optimistic revision checks for user decisions and sensitive
    commands.
-   Use idempotency keys for externally retried commands.
-   Do not delete historical evidence when a plan is revised.
-   Distinguish current state from historical event facts.
