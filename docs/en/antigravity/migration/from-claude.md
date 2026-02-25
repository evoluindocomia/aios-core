# From Claude to Antigravit

Technical guide on what was migrated, what changed, and what is exclusive to Antigravity.

> **Migration Date:** 2026-02-25  
> **Status:** ✅ COMPLETED (Parts 1-5)  
> **Full Walkthrough:** `.synapse/antigravit/walkthrough-migracao-claude.md`

---

## What was Migrated

The legacy `.claude/` environment had **8 layers** that were mapped to the new `.antigravity/` structure:

| Claude Layer  | Files                               | Antigravit Destination                   |
| ------------- | ----------------------------------- | ---------------------------------------- |
| Configuration | `CLAUDE.md` (379L), `settings.json` | `ANTIGRAVITY.md` (17KB)                  |
| Rules         | 10 `.md` files                      | `.antigravity/rules/` (8 files)          |
| Hooks         | 12 files (6 Python scripts)         | `governance.md` (instructional)          |
| Agents        | 29 `.md` files                      | `.antigravity/agents/` (28 files)        |
| Skills        | 12 skills + subfolders              | `.antigravity/skills/` (6 skills)        |
| Commands      | 4 slash command namespaces          | `.antigravity/workflows/` (4 workflows)  |
| Templates     | 18 YAML/MD templates                | `.antigravity/templates/` (16 templates) |
| Agent Memory  | 8 subfolders with `MEMORY.md`       | `.antigravity/agent-memory/` (7 agents)  |

---

## Critical Changes

### 1. Hooks → Governance (Main Difference)

**In Claude Code:**
Automatic `PreToolUse` hooks blocked operations via exit codes (exit 2 = blocked).

**In Antigravit:**
Instructional checklists in `governance.md`. The agent proactively verifies rules before each operation. This relies on agent autonomy and covers ~95% of use cases.

| Original Python Hook            | Antigravit Governance Equivalent                |
| ------------------------------- | ----------------------------------------------- |
| `enforce-architecture-first.py` | 🔴 "Before WRITING to protected paths"          |
| `mind-clone-governance.py`      | 🔴 "Before CREATING agent in squads/\*/agents/" |
| `sql-governance.py`             | 🔴 "Before EXECUTING SQL DDL"                   |
| `enforce-git-push-authority.sh` | 🔴 "Before GIT PUSH"                            |
| `write-path-validation.py`      | 🟡 "Before SAVING documents"                    |

---

### 2. Inline Persona vs. External Files

Antigravit agents have their persona **INLINE** within the agent file (e.g., `.antigravity/agents/aios-dev.md`). No more external file dependencies, making agents more portable and specific.

### 3. Slash Commands → Workflows

| Claude Command         | Antigravit Workflow           |
| ---------------------- | ----------------------------- |
| `/AIOS:story`          | `story-development-cycle.md`  |
| `/AIOS:spec-pipeline`  | `spec-pipeline.md`            |
| `/cohort-squad:create` | `create-squad.md` (via packs) |
| `/cohort-squad:sync`   | `sync-squads.md` (via packs)  |
| Analysis commands      | `brownfield-discovery.md`     |

---

## Antigravit Advantages (Exclusives)

- **AI Image Generation**: Native `generate_image` tool.
- **Interactive UI Design**: 8 dedicated tools via `mcp_stitch`.
- **Browser Recording**: `browser_subagent` with WebP video capture.
- **Advanced Context**: The **SYNAPSE Context Engine** and **KI System**.
- **Progress Visibility**: Native `task_boundary` and `notify_user` mechanics.

---

## Lessons Learned

1. **Hooks are not 1:1 replicable**: Instructional governance is the best path for agent-centric systems.
2. **Squad Management Adaptation**: The "Broken Migration" was fixed by moving orchestration from core to project-level packs (`squads/squad-creator/`).
3. **Minds First approach**: Consolidating research specialists (`@research-specialists`) improved high-fidelity mind cloning.
