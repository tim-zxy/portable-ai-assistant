# Portable AI Assistant --- Engineering Baseline

**Product:** Portable AI Assistant (PAA)\
**Recommended repository:** `portable-ai-assistant`\
**Baseline version:** `0.1.0`\
**Package revision:** `0.1.1`\
**Status:** Consolidated design baseline for E0; not implementation
evidence\
**Target:** Windows x64 Personal Edition, with an extension path to
enterprise and cloud deployment

## 1. Purpose

This repository is the authoritative engineering baseline for building
PAA. It consolidates D01--D12 and subsequent clarifications into
implementation-oriented documents.

PAA is a general-purpose assistant that accepts user goals, plans and
coordinates work across Agent Runtimes, models, repositories, tools and
execution environments, tracks progress, handles interruptions, verifies
outcomes and maintains useful long-term knowledge.

## 2. Package contents

-   Root `AGENTS.md`: Codex operating instructions.
-   `docs/00` through `docs/13`: the **14 formal engineering
    documents**.
-   `docs/14-E0-TECHNICAL-VALIDATION.md`: E0 technical validation plan.
-   `docs/15-E0-STARTUP-PROMPT.md`: first Codex task prompt.
-   `PACKAGE-MANIFEST.json`: generated file hashes and sizes.

The 14 formal engineering documents are included in this initial
package. They are not a later deliverable.

## 3. Read order

1.  Root `AGENTS.md`
2.  `docs/00-PRODUCT-PRD.md`
3.  `docs/01-SYSTEM-ARCHITECTURE.md`
4.  `docs/02-DOMAIN-MODEL.md`
5.  `docs/03-API-AND-SPI-CONTRACTS.md`
6.  `docs/04-INTELLIGENCE-AND-RUNTIME.md`
7.  `docs/05-WORKSPACE-AND-INTEGRATIONS.md`
8.  `docs/06-MEMORY-AND-CONTEXT.md`
9.  `docs/07-SECURITY-AND-AUTHORIZATION.md`
10. `docs/08-UI-AND-INTERACTION.md`
11. `docs/09-ENGINEERING-PLAN.md`
12. `docs/10-ENGINEERING-RULES.md`
13. `docs/11-REAL-TASK-EVALUATION.md`
14. `docs/12-ARCHITECTURE-EVOLUTION.md`
15. `docs/13-RELEASE-AND-HANDOFF.md`
16. `docs/14-E0-TECHNICAL-VALIDATION.md`
17. `docs/15-E0-STARTUP-PROMPT.md`

## 4. Decision status

-   **FROZEN:** user-confirmed product or architectural constraint.
-   **PROPOSED:** recommended design to follow unless E0 reveals a
    material issue.
-   **VALIDATE:** requires a real proof of concept.
-   **LATER:** explicitly outside the first release.

E0 findings must update ADRs, TDRs, compatibility matrices and the
implementation plan. Do not treat an unverified proposal as proven.

## 5. Repository preparation

Recommended repository: `portable-ai-assistant`.\
Example local path: `D:\\workspace\\portable-ai-assistant`.

1.  Create and initialize the Git repository.
2.  Extract this package into the repository root, preserving
    `AGENTS.md`, `README.md`, `docs/` and `PACKAGE-MANIFEST.json`.
3.  Add an appropriate `.gitignore`.
4.  Review and commit the baseline.
5.  Run the prompt in `docs/15-E0-STARTUP-PROMPT.md`.

The package already includes `AGENTS.md`; do not create a duplicate. Do
not pre-create every source directory. E0 should validate the toolchain
and propose the minimal project skeleton.

## 6. First-release scope

The first release targets:

-   Windows x64 Desktop, Web, CLI and a local background assistant
    service.
-   One default assistant managing multiple conversations, projects,
    goals and tasks.
-   OpenCode and DeepSeek Harness as the first two real Runtime
    integrations.
-   Runtime and model discovery, capability verification and controlled
    launch.
-   Multi-repository project knowledge and task-level workspaces.
-   Goal, plan, task, invocation and run lifecycle management.
-   Four-layer memory and context assembly.
-   Durable local events, schedules, liveness checks and recovery.
-   Feishu, GitHub and GitHub Actions integrations.
-   Local credential protection, authorization, audit and revocation.
-   Extension contracts and a basic non-coding Research Assistant
    workflow.

Enterprise multi-tenancy, cloud Control Plane, distributed queues, large
Worker pools, GitCode adapter, Claude/Codex adapters and enterprise
Vault integration are later work.

## 7. Engineering principle

Build a small, working vertical slice first. Keep the implementation
modular and replaceable at genuine boundaries, but do not build future
infrastructure before there is a current requirement.

The product is complete only when representative real tasks can be
delegated, executed, tracked, recovered and accepted with evidence.
