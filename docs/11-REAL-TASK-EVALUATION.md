# 11 --- Real Task Evaluation and Baseline Comparison

## 1. Evaluation objective

Prove that PAA improves end-to-end work delivery in complex, multi-step,
multi-tool and long-running scenarios.

Do not evaluate only by code volume, number of Agents, test count or
response speed.

## 2. Nine task categories

  -------------------------------------------------------------------------------------
  ID                Category            Example           Main capability
  ----------------- ------------------- ----------------- -----------------------------
  T1                Knowledge Q&A       Retrieve a        Memory/context
                                        verified project  
                                        decision          

  T2                Deep research       Multi-source      Planning, retrieval, evidence
                                        research report   

  T3                Single-repository   Implement a real  Runtime/workspace
                    development         feature           

  T4                Multi-repository    Change client,    Relationship analysis and
                    development         service and       parallelism
                                        contract repos    

  T5                Failure repair      Diagnose CI       Event/recovery/verification
                                        failure, fix and  
                                        rerun             

  T6                Document and        Produce an        Artifact/tool orchestration
                    presentation        editable          
                                        deliverable       

  T7                Data analysis       Analyze a real    Structured tools and
                                        spreadsheet       verification

  T8                Continuous          Scheduled check,  Event/Timer
                    operations          Feishu            
                                        notification and  
                                        approval          

  T9                Long-running mixed  Clarification,    End-to-end orchestration
                    task                multiple Agents,  
                                        quota switch and  
                                        restart           
  -------------------------------------------------------------------------------------

## 3. Required evidence per scenario

Each scenario records:

-   Initial user request and input data.
-   Environment and repository versions.
-   Runtime and model actually used.
-   Plan and task breakdown.
-   User interventions and approvals.
-   Execution timeline.
-   Token/resource usage where available.
-   Failures and recovery actions.
-   Final artifacts.
-   Independent acceptance result.

## 4. Baseline comparison

Compare PAA with a human-plus-single-Agent workflow on the same task.

Control where practical:

-   Same task and input.
-   Same model or comparable capability.
-   Similar resource/token budget.
-   Same acceptance rubric.
-   Independent reviewer.

Record differences rather than claiming PAA must win every individual
task.

## 5. Metrics

  Metric                    Definition
  ------------------------- ------------------------------------------------
  Task success rate         Accepted tasks / attempted tasks
  Requirement coverage      Satisfied requirements / valid requirements
  End-to-end time           Delegation to accepted result
  Human intervention        Number and type of required user actions
  Rework rate               Rework caused by planning/execution errors
  Token per accepted task   Recorded token use per accepted task
  Recovery rate             Recoverable interruptions successfully resumed
  Evidence completeness     Required evidence present and traceable
  Memory reuse              Relevant prior knowledge reused correctly
  Unauthorized action       Any action outside effective authorization

Token data must preserve partial/unknown status where the Runtime does
not report complete usage.

## 6. Hard gates

-   No known unresolved release-blocking security issue.
-   No unauthorized external side effect.
-   No false completion claim.
-   Task completion requires acceptance evidence.
-   Multi-repository verification identifies exact commit set.
-   User revocation blocks subsequent relevant operations.
-   Service restart does not silently lose durable tasks/events.
-   Real OpenCode and Harness execution is demonstrated.

## 7. Acceptance rubric

Score each scenario on:

1.  Correctness.
2.  Requirement coverage.
3.  Evidence quality.
4.  Execution reliability.
5.  Appropriate autonomy.
6.  Time and resource efficiency.
7.  User experience.

The evaluator should be independent of the Agent that performed the task
where practical.

## 8. Release report

The final report includes scenario outcomes, baseline comparison,
limitations, failed cases, security results and unresolved TDRs. Do not
summarize only successful demonstrations.
