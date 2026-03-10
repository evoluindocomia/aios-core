# Step-by-Step Process: New Project (Greenfield)

This document describes the end-to-end operational flow for creating new software (Greenfield Fullstack) using AIOS within the Antigravity environment. This process is orchestrated by a squad of specialized agents.

---

## Phase 0: Environment Bootstrap (@devops)

Before planning begins, we prepare the technical foundation.

- **Action:** The `@devops` agent verifies the local environment (Git, Node, etc.).
- **Artifacts:** Creation of `.aios/config.yaml` and `.aios/environment-report.json`.
- **Result:** Initial project structure (_scaffolding_) is ready.

---

## Phase 1: Discovery and Planning

We transform the initial idea into concrete engineering specifications.

1.  **Project Brief (@analyst)**: Generates a comprehensive scope document (`docs/project-brief.md`) based on the initial description.
2.  **PRD - Product Requirements Document (@pm)**: Defines business rules, epics, and the MVP scope (`docs/prd.md`).
3.  **Front-End / UI Spec (@ux)**: Specifies user experience and visual guidelines (`docs/front-end-spec.md`). May include wireframes generated via _Google Stitch MCP_.
4.  **Full-Stack Architecture (@architect)**: Defines the technological solution, coding standards, and database schema (`docs/fullstack-architecture.md`).
5.  **Quality Gate / Validation (@po)**: The Product Owner validates the coherence of all artifacts. The process only moves forward with explicit approval.

---

## Phase 2: Document Sharding (@po)

To optimize development and prevent context overload:

- **Sharding:** The `@po` fragments the PRD into smaller operational files in the `docs/prd/` folder, including style guides and directory trees.

---

## Phase 3: Development Cycle (SDC)

Iteration over each functionality (Story) in the backlog.

1.  **Story Creation (@sm)**: The Scrum Master generates the task description with acceptance criteria (AC).
2.  **Impact Analysis (@architect)**: Validates that the implementation does not compromise system integrity.
3.  **Implementation (@dev or @ui-builder)**:
    - `@ui-builder`: Focused on visual interfaces and components.
    - `@dev`: Implements logic, backend, and integrations, ensuring all ACs and tests are met.
4.  **QA Review (@qa)**: The QA agent reviews the code. If it doesn't meet the criteria, it enters the `qa-loop` for corrections.
5.  **Commit (@devops)**: After QA approval, `@devops` performs the commit following the _Conventional Commits_ standard.
6.  **Final Push (@devops)**: After the completion of the epic or set of stories, `@devops` pushes to the remote repository.

---

## Handoff Flow Summary

`@analyst` → `@pm` → `@ux` → `@architect` → `@po` → (sharding) → `@sm` → `@dev` → `@qa` → `@devops`

---

_Document generated as a reference guide for executing new projects in AIOS._
