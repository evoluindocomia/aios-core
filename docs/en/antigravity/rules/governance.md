# Governance — Automatic Governance Pipeline

> **Origin:** This file replaces the **6 Python hooks** (`PreToolUse`) from Claude Code.  
> As the Antigravity environment does not have an automatic tool interception system, these validations are applied **proactively** by the agent before each critical operation via the AGP (AIOS Governance Pipeline).

---

## AIOS Governance Pipeline (AGP)

> **EXECUTION MECHANISM:** The rules below are implemented as active tasks.
> Instead of manually checking item by item, use the governance pipeline:
>
> ```text
> → skill: governance  (.antigravity/skills/governance/SKILL.md)
> → The corresponding task must be executed BEFORE the operation
> ```
>
> The sections below serve as a **reference of the policies** (source of truth).
> The AGP is the **execution mechanism** for these policies. Configurations are set in `.antigravity/rules/governance-config.md`.

---

## How It Works

In Claude Code, there were Python scripts that **automatically blocked** unwanted operations before they were executed:

```
enforce-architecture-first.py  → blocked writes without approved architecture doc
mind-clone-governance.py       → blocked functional agent creation if identified as human without DNA
sql-governance.py              → blocked direct SQL DDL commands
enforce-git-push-authority.sh  → blocked git push from non-devops agents
write-path-validation.py       → validated semantic paths of documents
slug-validation.py             → validated snake_case formatting for slugs
```

In Antigravity, **these rules live in `governance.md`** and are checked instructionally by the agent through specific Tasks within the governance skill. The pipeline can result in three outcomes: `ALLOWED`, `REQUIRES_APPROVAL`, or `BLOCKED`.

---

## Mandatory Checks

### 🔴 Before WRITING or EDITING code in protected paths

**AGP Task:** `check-architecture-first` → `.antigravity/skills/governance/`

**Trigger:** Any `write_to_file` or `replace_file_content` operation in:

- `supabase/functions/**`
- `supabase/migrations/**`

**Checklist:**

- [ ] Does an approved `docs/architecture/{name}.md` exist?
- [ ] OR does an approved `docs/approved-plans/{name}.md` exist?
- [ ] If the file already exists → edit operation is **allowed**

**If documentation DOES NOT exist:**

1. Create architectural documentation first
2. Obtain approval via `notify_user`
3. THEN return to the code operation

**ALWAYS ALLOWED paths** (no doc required):

```
.antigravity/  |  docs/  |  outputs/  |  squads/
.aios-core/    |  .aios-custom/  |  node_modules/  |  .git/
package.json   |  package-lock.json  |  tsconfig.json
.env           |  README.md
```

---

### 🔴 Before CREATING an agent in `squads/*/agents/*.md`

**AGP Task:** `check-mind-clone-dna` → `.antigravity/skills/governance/`

**Trigger:** `write_to_file` creating a new `.md` inside `squads/*/agents/`

**Checklist — verify if it's a functional agent:**

- [ ] Does the agent-id have a functional suffix?  
       (`-chief`, `-orchestrator`, `-chair`, `-validator`, `-calculator`, `-generator`, `-extractor`, `-analyzer`, `-architect`, `-mapper`, `-designer`, `-engineer`)
- [ ] OR does it start with `tools-`, `process-`, `workflow-`?

**If YES** → ✅ Functional agent. Create normally.

**If NO** (likely a real person) → Check DNA:

- [ ] Does `squads/{pack}/data/minds/{agent_id}_dna.yaml` exist?
- [ ] OR `squads/{pack}/data/minds/{agent_id}_dna.md` exist?
- [ ] OR `squads/{pack}/data/{agent_id}-dna.yaml` exist?
- [ ] OR `outputs/minds/{agent_id}/` exist?

**If DNA DOES NOT exist** → **BLOCK.** Execute pipeline:

```
*collect-sources → *extract-voice-dna → *extract-thinking-dna → *create-agent
```

---

### 🔴 Before EXECUTING SQL DDL via `run_command`

**AGP Task:** `check-sql-governance` → `.antigravity/skills/governance/`

**Trigger:** `run_command` containing any SQL DDL

**BLOCKED Commands** (never execute directly):

```sql
CREATE TABLE ...
CREATE VIEW ...
CREATE FUNCTION ...
CREATE TRIGGER ...
ALTER TABLE ...
DROP TABLE ...
DROP VIEW ...
DROP FUNCTION ...
CREATE TABLE AS SELECT ...  -- backup prohibited
```

**ALLOWED Commands:**

```bash
supabase migration new {name}   # Via official CLI
supabase db push                # Migration deploy
pg_dump ...                     # Backup/export
SELECT / INSERT / UPDATE        # DML is allowed
```

---

### 🔴 Before GIT PUSH

**AGP Task:** `check-git-push-authority` → `.antigravity/skills/governance/`

**Trigger:** `run_command` with `git push`

**Verification:**

- [ ] Is the active agent `@devops` (Gage)?

**If NO** → **BLOCK.** Delegate:

```
→ @devops *push
```

**If YES** → Execute quality gates before pushing:

```bash
npm run lint         # ✅ must pass
npm run typecheck    # ✅ must pass
npm test             # ✅ must pass
```

---

### 🟡 Before CREATING or REMOVING Git Worktrees

**AGP Task:** `check-git-push-authority` → `.antigravity/skills/governance/`

**Trigger:** `run_command` with `git worktree add` or `git worktree remove`

**Mandatory verification:**

- [ ] Is the active agent `@devops` (Gage)?

**If NO** → **BLOCK.** Worktrees must be managed solely by @devops:

```
→ @devops *create-worktree {branch}
→ @devops *remove-worktree {branch}
→ @devops *list-worktrees
```

**If YES** → Follow the `auto-worktree.md` workflow protocol.

---

### 🟡 Before SAVING documents (warnings)

**AGP Task:** `check-write-path` → `.antigravity/skills/governance/`

**Verify correct paths:**

| Document Type       | Correct Path             |
| ------------------- | ------------------------ |
| Sessions / handoffs | `docs/sessions/YYYY-MM/` |
| Architecture        | `docs/architecture/`     |
| Guides              | `docs/guides/`           |
| Stories             | `docs/stories/`          |
| Approved plans      | `docs/approved-plans/`   |

**Action:** If the document appears to be in the wrong path, alert before saving.

---

### 🟡 Before using `mcp_stitch_*`

**Trigger:** Any invocation of Stitch MCP tools

**Verification:**

- [ ] Is the context Design System or UI? (workflows: `design-system-build.md`, `greenfield-ui.md`, `brownfield-ui.md`)
- [ ] Will the output be saved/referenced in `docs/design-system/` or `docs/`?

**If YES** → Proceed normally.
**If NO** → Alert: "Use Stitch MCP only in the context of UI/Design System workflows."

---

### 🟡 Slug & ID Format — Mandatory Validation

**AGP Task:** `check-slug-format` → `.antigravity/skills/governance/`

**Rule:** All slugs and IDs MUST be `snake_case`

```
Valid pattern: ^[a-z0-9]+(_[a-z0-9]+)*$
```

| Example              | Status                 |
| -------------------- | ---------------------- |
| `jose_carlos_amorim` | ✅ VALID               |
| `workflow_execution` | ✅ VALID               |
| `jose-carlos-amorim` | ❌ INVALID (hyphen)    |
| `JoseAmorim`         | ❌ INVALID (camelCase) |
| `jose.carlos`        | ❌ INVALID (dot)       |

---

### 🔵 Partial Reading of Protected Files

**Files that should NEVER be read partially** (with StartLine/EndLine when making critical decisions):

- `.antigravity/ANTIGRAVITY.md`
- `.antigravity/rules/*.md`
- `.aios-core/development/agents/*.md`
- `supabase/docs/SCHEMA.md`
- `package.json`, `tsconfig.json`

**Rule:** Always read the full file when making decisions about these files.

---

## Manual Validation Scripts

> **Note:** For integrated use, prefer the AGP via `skill governance`.

The original Python scripts in `.claude/hooks/` still exist and can be used for manual validation if needed:

```bash
# Validate architecture-first
echo '{"tool_name": "Write", "tool_input": {"file_path": "supabase/functions/my-function/index.ts"}}' \
  | python3 .claude/hooks/enforce-architecture-first.py

# Validate mind-clone governance
echo '{"tool_name": "Write", "tool_input": {"file_path": "squads/my-squad/agents/gary-halbert.md"}}' \
  | python3 .claude/hooks/mind-clone-governance.py

# Validate slug
echo '{"tool_name": "Bash", "tool_input": {"command": "create slug jose-carlos"}}' \
  | python3 .claude/hooks/slug-validation.py
```
