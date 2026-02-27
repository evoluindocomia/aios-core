---
description: UI Generation Pipeline with Google Stitch
---

# 🎨 Stitch UI Generation Pipeline

This workflow describes the mandatory process for creating screens (Web or Mobile) using the MCP integration with Google Stitch. It ensures extreme visual fidelity and architectural consistency without requiring the agent to "guess" the UI code.

## 📋 Prerequisites (One-Time Setup)

Before generating any screen, the project must have its visual identity configuration file in place. This information is required **only once** and serves as a guide for the entire application lifecycle.

1. Verify the existence of the visual identity configuration file (e.g., `docs/architecture/ui-guidelines.yaml`).
2. If it does not exist, the agent or the user **must** create it containing the following minimum definitions:
   ```yaml
   ui_guidelines:
     platform: 'web' # or "mobile"
     script_language: 'TypeScript / React 18' # or Flutter, Svelte, Vue, etc.
     css_framework: 'TailwindCSS v3' # or pure CSS, Bootstrap, etc.
     design_system:
       primary_color: '#0F172A'
       secondary_color: '#38BDF8'
       font_family: 'Inter, sans-serif'
       border_radius: '0.5rem'
     component_framework: 'Shadcn UI' # or DaisyUI, Radix, Cupertino, etc.
   ```

## 🚀 Generation Step-by-Step (Story Development)

Whenever the story in progress has the scope of "Frontend" or "Screen Creation", the `story-development-cycle.md` Workflow will direct the task to the **`@ui-builder`**, the specialist agent with native knowledge of Stitch.

1. **Combined Context Reading:**
   - The `@ui-builder` reads the functional requirements outlined in the selected Story.
   - The `@ui-builder` compulsorily reads the `ui-guidelines.yaml` file, loading the technological and design constraints.

2. **Orchestration with Stitch (MCP Integration):**
   - The agent consolidates the data and interacts with the **Google Stitch** MCP Server tool.
   - **Prompt Format:** The agent delegates the visual creation via a detailed prompt, e.g., _"Generate a [Screen Objective] screen that strictly obeys the following parameters: Framework [Language], Styles via [CSS Framework], using [Component Framework]. Apply the design system [colors and fonts]. The functional data and behaviors are: [Story Requirements]."_

3. **Injected Generated Code:**
   - The code (or multiple components) returned by Stitch is integrated and saved in the corresponding directories of the codebase (e.g., `src/app/pages/login/page.tsx`, `lib/screens/login_screen.dart`, etc.).

4. **Logical Refinement (Business Logic Binding):**
   - With the perfect visual shell already generated using the right technology, the `@ui-builder` handles the final refactoring by implementing the business logic.
   - The agent injects State Hooks, route management, and real API calls (Services/BFFs) into the components drawn by Stitch.

5. **Compliance Validation (@QA):**
   - The QA agent checks the Pull Request/Commit, ensuring that the technology adopted by Stitch actually respected the `ui-guidelines.yaml`, preventing, for example, Stitch from inserting Bootstrap into a project configured for TailwindCSS.
