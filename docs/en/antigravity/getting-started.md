# Getting Started with Antigravity

A quick guide to navigating and using the Antigravity environment within Synkra AIOS.

---

## 1. Environment Activation

Antigravity is automatically active within the project root via the `.antigravity/` folder. All agents and workflows are loaded at session startup.

## 2. Interacting with Agents

Use the `@` symbol to call specific agents for tasks:

- **Code:** `@dev implement the logout feature`
- **Quality:** `@qa run all tests for the payments module`
- **Planning:** `@pm create a PRD for the new dashboard`
- **Squads:** `@squad-chief I need a marketing specialist squad`

## 3. Using Star Commands (`*`)

Specialized actions can be triggered using the `*` prefix:

- `@po *create-story` — Starts the story creation template.
- `@squad-chief *create-squad` — Initiates the mind-research-cloning pipeline.
- `@qa *qa-gate` — Executes the full suite of quality checks.

## 4. The Development Cycle

We follow the **Story-Driven Development** (SDD) model:

1.  **Draft**: Use `@sm *draft` to outline a new feature.
2.  **Validate**: `@po` reviews and defines Acceptance Criteria (AC).
3.  **Develop**: `@dev` implements the changes in a new branch.
4.  **Verify**: `@qa` runs gated tests.
5.  **Push**: `@devops` (exclusive authority) performs the `git push`.

## 5. Locating Artifacts

- **Stories**: `docs/stories/active/`
- **Architecture**: `docs/architecture/`
- **Sprints/Tasks**: `.synapse/antigravit/`
- **Agent Memory**: `.antigravity/agent-memory/`

---

## First Steps Checklist

- [ ] Check `ANTIGRAVITY.md` for project-specific ground rules.
- [ ] List available squads using `@squad-chief *list-squads`.
- [ ] Review the `governance.md` rules to understand the safety checkpoints.
