# Tool Mapping

Comparison between Claude Code tools and native Antigravity tools.

---

## Direct Tool Mapping

| Claude Tool | Antigravity Tool                                      | Notes                                     |
| ----------- | ----------------------------------------------------- | ----------------------------------------- |
| `Read`      | `view_file`                                           | Supports line range and file outline      |
| `Write`     | `write_to_file`                                       | Includes artifact metadata support        |
| `Edit`      | `replace_file_content` / `multi_replace_file_content` | Multi-replace is exclusive to Antigravit  |
| `Grep`      | `grep_search`                                         | High-speed ripgrep searching              |
| `Glob`      | `find_by_name`                                        | File and directory discovery              |
| `Bash`      | `run_command`                                         | Command execution with status tracking    |
| `WebSearch` | `search_web`                                          | Real-time web browsing                    |
| `WebFetch`  | `read_url_content` / `read_browser_page`              | `read_browser_page` supports JS execution |

---

## Antigravity Exclusive Tools

| Tool               | Capability                                              |
| ------------------ | ------------------------------------------------------- |
| `generate_image`   | Creating UI assets, mockups, and illustrations.         |
| `mcp_stitch_*`     | Professional UI design and screen generation (8 tools). |
| `browser_subagent` | Complex browser automation with video recording.        |
| `task_boundary`    | Visual progress tracking and task state management.     |
| `notify_user`      | Structured communication and artifact review requests.  |
| `view_code_item`   | Advanced semantic node inspection (Functions/Classes).  |
| `command_status`   | Monitoring long-running background tasks.               |

---

## Core Framework Integrations

### 1. KI System (Knowledge Items)

Unlike Claude's simple session memory, Antigravity uses **Knowledge Items (KIs)** for persistent, cross-session architecture and decision memory. Use `list_dir` on `knowledge/` to explore.

### 2. SYNAPSE Context Engine

The engine that organizes rules into 8 layers and adaptively injects them into the agent's prompt based on token availability.

### 3. Task Management Artifacts

Antigravit uses standard artifacts like `task.md` and `implementation_plan.md` to coordinate complex development cycles, visible via the UI.
