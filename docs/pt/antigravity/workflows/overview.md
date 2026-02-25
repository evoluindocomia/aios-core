# Workflows — Os 4 Workflows Nativos

Os **4 workflows** em `.antigravity/workflows/` são equivalentes aos slash commands do Claude Code, traduzidos para o formato nativo do Antigravit.

---

## O que são Workflows no Antigravit?

No Claude Code, os workflows eram slash commands como `/AIOS:story`, `/cohort-squad:create`. No Antigravit, eles são arquivos `.md` em `.antigravity/workflows/` que os agentes executam como sequências de passos estruturados.

**Formato:**

```yaml
---
description: breve descrição do workflow
---
## Passos
1. ...
2. ...
```

---

## Tabela de Workflows

| Workflow                  | Origem (Claude)       | Responsável                        | Quando Usar                    |
| ------------------------- | --------------------- | ---------------------------------- | ------------------------------ |
| `story-development-cycle` | `/AIOS:story`         | `@sm → @po → @dev → @qa → @devops` | Ciclo completo de feature      |
| `create-squad`            | `/cohort-squad:*`     | `@squad-chief`                     | Criar novo squad especializado |
| `brownfield-discovery`    | Commands de análise   | `@architect → @analyst`            | Analisar projeto legado        |
| `spec-pipeline`           | `/AIOS:spec-pipeline` | `@pm → @architect → @dev`          | Pipeline de especificação      |

---

## Detalhes dos Workflows

### `story-development-cycle` — Ciclo Completo de Story

**Arquivo:** `.antigravity/workflows/story-development-cycle.md`

O workflow mais usado — governa o ciclo completo de desenvolvimento de uma feature.

**Sequência:**

```
@sm *draft
  ↓
@po *validate
  ↓
@dev *develop
  ↓
@qa *qa-gate
  ↓
@devops *push
```

**Detalhes por etapa:**

| Etapa    | Agente    | Artefato                                | Critério de Saída                 |
| -------- | --------- | --------------------------------------- | --------------------------------- |
| Draft    | `@sm`     | Story rascunhada em `docs/stories/`     | Story com critérios iniciais      |
| Validate | `@po`     | Story com acceptance criteria definidos | ACs mensuráveis e completos       |
| Develop  | `@dev`    | Código implementado                     | Todos os ACs cobertos             |
| QA Gate  | `@qa`     | Relatório de qualidade                  | lint ✅ + typecheck ✅ + tests ✅ |
| Push     | `@devops` | PR criado / branch pushed               | Remote atualizado                 |

**Como acionar:**

```
@sm *draft {nome-da-feature}
```

---

### `create-squad` — Criação de Squad

**Arquivo:** `.antigravity/workflows/create-squad.md`

Workflow completo para criar um squad especializado do zero para qualquer domínio.

**Sequência:**

```
1. @squad-chief recebe request de domínio
2. → IMEDIATAMENTE pesquisa mentes de elite (search_web)
3. → Executa 3-5 iterações de pesquisa com devil's advocate
4. → Apresenta lista curada de mentes reais com frameworks
5. → Usuário aprova
6. → @squad-chief *clone-mind para cada mente (via @oalanicolas)
7. → @squad-chief *create-agent usando DNA extraído
8. → Gera estrutura de diretórios do squad
9. → Gera README e config do squad
```

**Regra crítica — MINDS FIRST:**

```
❌ ERRADO: criar agentes genéricos sem pesquisa
✅ CORRETO: pesquisar mentes → extrair DNA → criar agentes baseados em especialistas reais
```

**Output:**

```
squads/{nome-do-squad}/
├── agents/           ← Agentes com DNA de especialistas reais
├── tasks/            ← Tasks do squad
├── data/minds/       ← DNA YAML de cada especialista
└── README.md         ← Documentação do squad
```

**Como acionar:**

```
@squad-chief quero um squad de [domínio]
```

---

### `brownfield-discovery` — Descoberta de Projetos Legados

**Arquivo:** `.antigravity/workflows/brownfield-discovery.md`

Para analisar e entender projetos existentes antes de fazer mudanças. Evita a síndrome de "ver o código pela primeira vez e querer reescrever tudo".

**Sequência:**

```
1. @architect mapeia estrutura existente (find_by_name, list_dir)
2. @analyst identifica padrões e decisões técnicas (grep_search)
3. → Mapeamento de dependências
4. → Identificação de gaps e dívidas técnicas
5. → Criar docs/architecture/brownfield-map.md
6. → Propor strategy: Reuse > Adapt > Create
```

**Artefatos gerados:**

- `docs/architecture/brownfield-map.md` — mapa do sistema existente
- `docs/architecture/tech-debt.md` — dívida técnica identificada
- `docs/approved-plans/migration-strategy.md` — estratégia de migração

**Como acionar:**

```
@architect analise a estrutura existente do projeto antes de começarmos
```

---

### `spec-pipeline` — Pipeline de Especificação

**Arquivo:** `.antigravity/workflows/spec-pipeline.md`

Para features complexas que precisam de especificação detalhada antes de desenvolvimento.

**Sequência:**

```
@pm → Project Brief (project-brief-tmpl.yaml)
  ↓
@analyst → Pesquisa de mercado / competidores
  ↓
@architect → Especificação técnica + ADRs
  ↓
@po → PRD completo (prd-tmpl.yaml)
  ↓
@po → Stories breakdown
  ↓
@dev → Implementação (via story-development-cycle)
```

**Templates utilizados:**

| Etapa           | Template                                                  |
| --------------- | --------------------------------------------------------- |
| Project Brief   | `.antigravity/templates/project-brief-tmpl.yaml`          |
| Market Research | `.antigravity/templates/market-research-tmpl.yaml`        |
| Frontend Spec   | `.antigravity/templates/front-end-spec-tmpl.yaml`         |
| Architecture    | `.antigravity/templates/fullstack-architecture-tmpl.yaml` |
| PRD             | `.antigravity/templates/prd-tmpl.yaml`                    |

**Como acionar:**

```
@pm inicie o pipeline de especificação para a feature X
```

---

## Workflows via Slash Commands (Claude) → Antigravit

| Claude Command         | Antigravit Equivalente                       |
| ---------------------- | -------------------------------------------- |
| `/AIOS:story`          | `@sm *draft` → story-development-cycle       |
| `/AIOS:spec-pipeline`  | `@pm → spec-pipeline workflow`               |
| `/cohort-squad:create` | `@squad-chief *create-squad`                 |
| `/AIOS:brownfield`     | `@architect → brownfield-discovery workflow` |

---

## Documentação Relacionada

- [Agentes Core](../agents/core-agents.md) — Agentes que executam os workflows
- [Templates](../templates/overview.md) — Templates usados nos workflows
- [Skills](../skills/overview.md) — Skills chamadas durante os workflows
