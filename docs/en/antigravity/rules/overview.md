# Rules — Behavioral Rule System

The **8 rules** in `.antigravity/rules/` define how agents behave. They are loaded automatically or by context (via the `paths:` frontmatter).

> **Updated:** 2026-02-26 — `workflow-execution.md` now covers all **14 workflows**, and `governance.md` now implements the AIOS Governance Pipeline (AGP).

---

## What are Rules in Antigravity?

Rules are `.md` files that Antigravity injects into the agents' context. Unlike Claude Code (where they were all always loaded), in Antigravity rules can be **conditional by path** — they only become active when the agent is working on related files.

---

## Rules Table

| File                      | Type     | Description                                                |
| ------------------------- | -------- | ---------------------------------------------------------- |
| `agent-authority.md`      | Migrated | Hierarchy of authority, Framework/Project Boundary L1-L4   |
| `agent-handoff.md`        | Migrated | Handoff protocol between agents, integrated with KI System |
| `agent-memory-imports.md` | Migrated | When and how to load MEMORY.md, KI summaries               |
| `governance.md`           | **NEW**  | Replaces 6 Claude Python hooks — AGP pre-operation checks  |
| `ids-principles.md`       | Migrated | IDS: REUSE > ADAPT > CREATE                                |
| `story-lifecycle.md`      | Migrated | Full lifecycle of stories                                  |
| `tool-usage.md`           | **NEW**  | Rules for tool usage + MCP                                 |
| `workflow-execution.md`   | Migrated | 14 complete workflows + selection guide                    |

---

## Critical Rules by Category

### 🏛️ Agent Authority (`agent-authority.md`)

Defines **who can do what** and the hierarchy of authority between agents.

**Framework vs Project Boundary:**

| Layer        | Path                                    | Rule                   |
| ------------ | --------------------------------------- | ---------------------- |
| L1 Core      | `.aios-core/core/`, `bin/aios.js`       | NEVER modify           |
| L2 Templates | `.aios-core/development/`               | NEVER modify           |
| L3 Config    | `.aios-core/data/`, `MEMORY.md`         | Modify with care       |
| L4 Runtime   | `docs/stories/`, `packages/`, `squads/` | ALWAYS can be modified |

**Exclusive authority:** Only `@devops` can execute `git push`.

---

### 🔄 Agent Handoff (`agent-handoff.md`)

Formal protocol for passing context between agents.

**Handoff format:**

```
→ @{agent-id} — {reason}
Context: {summary of current state}
Next step: {what must be done}
Artifacts: {links to KIs or relevant artifacts}
```

**KI System Integration:** Upon handoff, the originating agent documents critical decisions in Knowledge Items for retrieval in future sessions.

---

### 🧠 Agent Memory Imports (`agent-memory-imports.md`)

Defines when and how to load persistent memory.

**Loading priority:**

1. KI Summaries (Knowledge System summaries)
2. Relevant conversation logs
3. Specific agent's `MEMORY.md`

**Rule:** Always verify KI summaries **before** any deep research. Prevents duplicate work across sessions.

---

### 🛡️ Governance (`governance.md`)

> This is the most critical operational file. See full documentation: [Governance](./governance.md)

Replaces the **6 Python hooks** from Claude Code with an instructional system of mandatory checks using the AIOS Governance Pipeline (AGP).

---

### ♻️ IDS Principles (`ids-principles.md`)

**Incremental Development Strategy:** REUSE > ADAPT > CREATE

```
1. REUSE   → Is there something that solves this in the codebase?
2. ADAPT   → Can I adapt something existing?
3. CREATE  → Only then create from scratch
```

This rule forces agents to consult the codebase before creating any new structures.

---

### 📖 Story Lifecycle (`story-lifecycle.md`)

Defines the complete lifecycle of a story:

```
draft → validated → in_progress → review → done → archived
```

**States and owners:**

- `draft` — `@sm` or `@po`
- `validated` — `@po` (acceptance criteria defined)
- `in_progress` — `@dev`
- `review` — `@qa`
- `done` — `@devops` (after push)
- `archived` — move to `docs/stories/completed/`

---

### 🔧 Tool Usage (`tool-usage.md`)

Tool usage priority:

| Task            | Preferred Tool                                        | Avoid             |
| --------------- | ----------------------------------------------------- | ----------------- |
| Read file       | `view_file`                                           | Equivalent MCPs   |
| Edit file       | `replace_file_content` / `multi_replace_file_content` | Bash alternatives |
| Create file     | `write_to_file`                                       | —                 |
| Search content  | `grep_search`                                         | —                 |
| Search files    | `find_by_name`                                        | —                 |
| Shell commands  | `run_command`                                         | —                 |
| Browser         | `browser_subagent`                                    | —                 |
| Generate images | `generate_image`                                      | —                 |

**Available MCPs:**

- `stitch` — 8 UI design tools

---

### 🔄 Workflow Execution (`workflow-execution.md`)

The 14 workflows and when to trigger them — now with a full selection guide:

| Category              | Workflows                                                                             |
| --------------------- | ------------------------------------------------------------------------------------- |
| Greenfield (New)      | `greenfield-fullstack`, `greenfield-service`, `greenfield-ui`                         |
| Brownfield (Existing) | `brownfield-discovery`, `brownfield-fullstack`, `brownfield-service`, `brownfield-ui` |
| Development           | `story-development-cycle`, `spec-pipeline`, `epic-orchestration`, `qa-loop`           |
| Specials              | `design-system-build`, `create-squad`, `auto-worktree`                                |

> Selection Index: [`.antigravity/workflows/README.md`](../../../../.antigravity/workflows/README.md)

---

## How Rules Are Loaded

In Antigravity, the rules system works like this:

1. **Global rules** — Always loaded (no `paths:` in frontmatter)
2. **Conditional rules** — Loaded only when working in specific paths
3. **ANTIGRAVITY.md** — Always loaded as the master document

```yaml
# Example of a rule with paths (conditional loading)
---
paths:
  - 'supabase/**'
  - 'packages/db/**'
---
```

> **Note:** Path-conditional rules optimize context usage — the agent only loads rules relevant to what they are currently doing.

---

## Related Documentation

- [Governance (detailed)](./governance.md)
- [Tool Mapping](../tools/tool-mapping.md)
- [Workflows](../workflows/overview.md)
