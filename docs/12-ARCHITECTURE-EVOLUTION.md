# 12 --- Architecture Decisions, Temporary Strategies and Evolution

## 1. Purpose

This register prevents Phase 1 shortcuts from becoming undocumented
permanent architecture.

Use three record types:

-   ADR: durable architecture decision.
-   TDR: temporary strategy with an explicit migration trigger.
-   Debt: known defect or maintainability issue requiring remediation.

## 2. TDR template

``` yaml
id: TDR-NNN
title: Short title
status: PLANNED
owner: workstream
current_solution: ...
reason: ...
limitations:
  - ...
target_solution: ...
migration_trigger:
  - ...
exit_criteria:
  - ...
introduced_in: ...
```

## 3. Initial TDR register

  -------------------------------------------------------------------------
  ID                Phase 1 strategy  Future direction    Trigger
  ----------------- ----------------- ------------------- -----------------
  TDR-001           Windows           Enterprise          Cloud centralized
                    Credential        Vault/KMS           credentials
                    Manager                               

  TDR-002           SQLite            PostgreSQL          Multi-instance
                                                          authoritative
                                                          state

  TDR-003           Local             Distributed event   Multi-Cell
                    Inbox/Outbox      transport           deployment

  TDR-004           SCM/CI polling    Central webhook     Cloud integration
                                      gateway             ingress

  TDR-005           Local worktree    Remote sandbox      Cloud isolated
                                                          execution

  TDR-006           Local Runtime     Distributed         Multiple Workers
                    Registry          capability registry 

  TDR-007           SQLite FTS/basic  Specialized         Measured
                    search            retrieval service   retrieval
                                                          insufficiency

  TDR-008           Local assistantd  Cloud Control Plane Cloud assistant
                                                          deployment

  TDR-009           Built-in/static   Isolated extension  Third-party
                    adapter           process             extension
                    registration                          ecosystem

  TDR-010           Local model       Enterprise Model    Central model
                    discovery         Gateway             governance

  TDR-011           Local user        Enterprise          Enterprise cloud
                    identity          SSO/multi-tenancy   

  TDR-012           Local filesystem  Object storage      Cloud artifact
                    artifacts                             management
  -------------------------------------------------------------------------

These are planned evolution points, not mandatory pre-1.0 migrations.

## 4. TDR lifecycle

`PLANNED -> READY_TO_MIGRATE -> MIGRATING -> CLOSED`

Alternative outcomes: `ACCEPTED_LONG_TERM`, `CANCELLED`.

Review TDRs at each major phase exit and before formal release.

## 5. ADR requirements

An ADR records:

-   Context and problem.
-   Requirements and constraints.
-   Considered options.
-   Decision.
-   Consequences and trade-offs.
-   Validation evidence.
-   Superseded decision, if applicable.

Breaking public SPI, data ownership, security boundary or infrastructure
choices require an ADR.

## 6. Evolution principles

-   Design replaceable boundaries, not speculative abstractions
    everywhere.
-   Do not replace SQLite before there is a measured requirement.
-   Do not build cloud infrastructure during local product delivery.
-   Keep domain contracts stable and implementation adapters
    replaceable.
-   Preserve migration evidence and data compatibility.
-   Record implementation shortcuts when they affect future
    architecture.

## 7. Debt management

Every debt item includes severity, user impact, security impact, owner,
target phase and acceptance criteria. Do not classify every TODO as
architecture debt.
