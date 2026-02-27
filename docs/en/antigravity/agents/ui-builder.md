# Agent @ui-builder (Stitch Architect)

The **`@ui-builder`** is the Frontend Visual specialist agent and the certified orchestrator of Google Stitch. In the Antigravity ecosystem, it exists to solve the classic problem of AIs manually writing CSS or creating poor layouts due to lack of visual context.

## 🎯 Objective

Its objective is to generate beautiful interfaces, rich components, and to ensure that the project strictly complies with the informed Design System definitions by outsourcing the "heavy lifting" design work to the Google Stitch MCP server.

## 🚀 Where Does It Act?

While `@dev` focuses on backend evolution, generic scripts, and pure structural logic, `@ui-builder` takes on the challenge during Workflows where the primary target is **Screens, Layout Positioning, and Complex Styling**.

- Triggered via the Story Development Cycle (`story-development-cycle.md`) for features labeled as "Front-End".
- Triggered in `greenfield-ui.md` and `brownfield-ui.md`.

## 🧠 Heuristics and Behavioral Patterns

The intrinsic "DNA" of this agent forces it to work following 3 golden rules:

1. **The Guidelines Rule:**
   The agent will NEVER initiate the code for a screen without first looking for the constraint entity in the repository. It initially looks for `docs/architecture/ui-guidelines.yaml`.

2. **No Manual CSS Hallucination:**
   Once the guidelines are read, `@ui-builder` is instructed not to try writing HTML/CSS from scratch. Instead, it uses the `mcp_stitch_generate_screen_from_text` MCP tool (or its variants) to generate the visual shell first.

3. **The Binding Duty (Post-Stitch Focus):**
   For the `@ui-builder`, the primary visual work has been handled by Stitch. Its manual coding comes into play only in the state and interaction implementation part: adding React Hooks, event handlers, Axios/Fetch calls to the backend API, Routings, and State Management.

## 🛠 Native Tools

The `@ui-builder` carries the permission to invoke commands from the Google Stitch Model Context Protocol:

- `mcp_stitch_create_project`
- `mcp_stitch_generate_screen_from_text`
- `mcp_stitch_edit_screens`
- `mcp_stitch_generate_variants`
- Standard filesystem tools for saving the components (`view_file`, `write_to_file`, etc).

> **Remember:** Segregation of duties is essential for AIOS to maintain the quality and direction of user stories without inflating prompts unnecessarily.
