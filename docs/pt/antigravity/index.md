# Ambiente Antigravit — Visão Geral

> **Versão:** 1.0 | **Idioma:** PT-BR | **Status:** ✅ Produção

O **Antigravit** é o ambiente de configuração do AIOS para o **Google Gemini** (via IDE Antigravity). Ele reside em `.antigravity/` e define agentes, regras, skills, workflows e templates que controlam o comportamento do assistente neste repositório.

---

## O que é o `.antigravity/`?

O `.antigravity/` é para o **Antigravit** o que o `.claude/` é para o Claude Code: o cérebro de configuração do sistema. Ele foi migrado do Claude Code em **2026-02-25**, preservando toda a arquitetura AIOS e aproveitando as capacidades exclusivas do Antigravit.

```
.antigravity/
├── ANTIGRAVITY.md          ← Documento master do ambiente (17KB)
├── agents/                 ← 28 agentes (core, chiefs, mind clones, especiais)
├── rules/                  ← 8 regras de comportamento
├── skills/                 ← 6 skills SKILL.md
├── workflows/              ← 4 workflows equivalentes a slash commands
├── templates/              ← 16 templates de documentação
└── agent-memory/           ← Memória persistente de 7 agentes
```

---

## Por que Antigravit?

| Dimensão              | Claude Code                   | Antigravit                             |
| --------------------- | ----------------------------- | -------------------------------------- |
| Motor de IA           | Claude (Anthropic)            | Gemini (Google DeepMind)               |
| Arquivo master        | `.claude/CLAUDE.md`           | `.antigravity/ANTIGRAVITY.md`          |
| Hooks automáticos     | 6 scripts Python (PreToolUse) | `governance.md` instrucional           |
| Geração de imagens    | ❌ N/A                        | ✅ `generate_image`                    |
| Design de UI          | ❌ N/A                        | ✅ `mcp_stitch_*` (8 ferramentas)      |
| Automação de browser  | ❌ N/A                        | ✅ `browser_subagent` (com vídeo WebP) |
| Memória cross-session | MEMORY.md por agente          | KI System + MEMORY.md                  |

---

## Princípio Fundamental: CLI First

O Antigravit segue a mais importante regra constitucional do AIOS:

```
CLI First → Observability Second → UI Third
```

Toda implementação começa pela CLI. A UI jamais é pré-requisito para operação.

---

## Índice da Documentação

### 🚀 Início Rápido

- [Getting Started](./getting-started.md) — Primeiros passos com o Antigravit

### 🤖 Agentes

- [Visão Geral dos Agentes](./agents/overview.md) — Sistema de agentes: ativação, hierarquia, memória
- [Agentes Core AIOS](./agents/core-agents.md) — Os 11 agentes fundamentais
- [Chiefs](./agents/chiefs.md) — 8 Chiefs especializados por domínio
- [Mind Clones](./agents/mind-clones.md) — 5 Clones de mentes do Design Squad
- [Agentes Especiais](./agents/special-agents.md) — 4 Agentes com capacidades únicas

### 📋 Rules

- [Visão Geral das Rules](./rules/overview.md) — Como as regras de comportamento funcionam
- [Governance](./rules/governance.md) — Governança automática e substituição de hooks

### 🛠️ Skills

- [Visão Geral das Skills](./skills/overview.md) — As 6 skills e como usá-las

### 🏗️ Squads (Packs)

- [Squads & Escalabilidade](./squads/overview.md) — O modelo de packs e o orquestrador `squad-creator`

### 🔄 Workflows

- [Visão Geral dos Workflows](./workflows/overview.md) — Os 4 workflows nativos

### 📄 Templates

- [Catálogo de Templates](./templates/overview.md) — Os 16 templates disponíveis

### 🔧 Ferramentas

- [Mapeamento de Ferramentas](./tools/tool-mapping.md) — Claude → Antigravit + ferramentas exclusivas

### 🔀 Migração

- [Migração do Claude](./migration/from-claude.md) — Do `.claude/` para `.antigravity/`

---

## Referência Rápida

### Ativar um agente

```
@dev       → Dex (Desenvolvimento)
@qa        → Quinn (Qualidade)
@architect → Aria (Arquitetura)
@devops    → Gage (DevOps + git push)
@squad-chief → Squad Architect 🎨
```

### Workflow de desenvolvimento mais comum

```
@po *create-story → @dev implementa → @qa testa → @devops *push
```

### Ferramentas exclusivas do Antigravit

```typescript
generate_image()         // Gerar imagens e mockups UI
browser_subagent()       // Automação de browser com vídeo WebP
mcp_stitch_*()           // Design de interfaces (8 ferramentas)
task_boundary()          // Comunicação de progresso estruturada
```

---

_Synkra AIOS — Antigravit Configuration v1.0_
_Migrado de `.claude/` para `.antigravity/` em 2026-02-25_
