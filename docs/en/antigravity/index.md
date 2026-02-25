# Antigravity Environment

Technical documentation for the Antigravity configuration environment (`.antigravity/`).

> **Migration Date:** 2026-02-25  
> **Status:** ✅ COMPLETED (Parts 1-5)  
> **Portuguese Version:** [Português](../pt/antigravity/index.md)

---

## What is Antigravity?

**Antigravity** is the native configuration and agent orchestration system for the **Synkra AIOS**. It replaces the legacy `.claude/` environment with a more robust, instructional, and tool-enriched structure centered around Gemini models.

While Claude Code relied on automatic Python hooks and external persona files, Antigravity uses **Instructional Governance** and **Inline Personas**, prioritizing portability and agent autonomy.

---

## Architecture Overview

The `.antigravity/` directory is organized into 6 core layers:

1.  **Constitutional (`ANTIGRAVITY.md`)**: The project's master configuration and ground rules.
2.  **Governance (`rules/`)**: Behavioral rules and safety checks (replacing Python hooks).
3.  **Agents (`agents/`)**: Specialized AI personas (Dex, Quinn, Aria, Morgan, Pax, etc.).
4.  **Skills (`skills/`)**: Modular capabilities (Mind Cloning, Architecture Verification, Synapse Context).
5.  **Workflows (`workflows/`)**: Structured execution sequences (replacing slash commands).
6.  **Templates (`templates/`)**: Standardized document formats (PRDs, Stories, Specs).

---

## Documentation Index

### [1. Getting Started](./getting-started.md)

Quick guide on how to activate agents, use commands, and follow the development cycle.

### [2. Agents System](./agents/overview.md)

Detailed list of all 28+ agents, including Core AIOS roles and the Squad Chief 🎨 orchestrator.

### [3. Rules & Governance](./rules/overview.md)

How Antigravity ensures quality and security without automatic tool interception.

### [4. Skills & Capabilities](./skills/overview.md)

Modular powers like `clone-mind` and the `synapse` context engine.

### [5. Squads & Scalability](./squads/overview.md)

The Packs model and the `squad-creator` orchestrator.

### [6. Native Workflows](./workflows/overview.md)

Standard sequences for Story development, Squad creation, and System discovery.

### [7. Tool Mapping](./tools/tool-mapping.md)

Full translation table from Claude Code tools to Antigravit native tools.

### [8. Migration Guide](./migration/from-claude.md)

Lessons learned and technical mapping for those transitioning from the Claude environment.

---

## Core Principles

- **CLI First**: Optimized for terminal-based agent interaction.
- **Minds First**: Real frameworks from real experts > generic bot instructions.
- **Story Driven**: All code must originate from a validated Story.
- **Governance Inline**: Safety checks are built into the agent's instructions.
- **Reuse > Adapt > Create**: Always search for existing solutions before implementing.
