# Getting Started — Ambiente Antigravit

Guia rápido para começar a usar o AIOS no Antigravit (Gemini IDE).

---

## Pré-requisitos

- AIOS instalado (`npx aios-core install`)
- Ambiente **Antigravity** (IDE Google Gemini) configurado
- Repositório com `.antigravity/` presente

---

## 1. Entendendo o documento master

Toda sessão começa com o `ANTIGRAVITY.md` sendo carregado automaticamente. Ele define:

- **Constitution** — 6 princípios fundamentais (CLI First, Agent Authority, etc.)
- **Idioma** — PT-BR por padrão
- **Sistema de agentes** — quem faz o quê
- **Governança** — verificações obrigatórias antes de operações críticas

> Arquivo: [`.antigravity/ANTIGRAVITY.md`](../../../../.antigravity/ANTIGRAVITY.md)

---

## 2. Ativando agentes

Use o prefixo `@` para ativar qualquer agente:

```
@dev          → Dex, desenvolvedor principal
@qa           → Quinn, testes e qualidade
@architect    → Aria, arquitetura de sistema
@pm           → Morgan, product management
@po           → Pax, product owner e stories
@sm           → River, scrum master
@analyst      → Alex, pesquisa e análise
@data-engineer → Dara, banco de dados
@ux-design-expert → Uma, UX/UI design
@devops       → Gage, CI/CD e git push
@squad-chief  → Squad Architect 🎨
```

**Exemplo:**

```
@dev implemente o endpoint de autenticação conforme a story 3.2
```

---

## 3. Comandos de agente (`*`)

Dentro de um agente ativo, use `*` para executar comandos:

| Comando         | O que faz                              |
| --------------- | -------------------------------------- |
| `*help`         | Lista todos os comandos disponíveis    |
| `*create-story` | Cria uma nova story de desenvolvimento |
| `*task {nome}`  | Executa uma task específica            |
| `*exit`         | Sai do modo agente                     |

**Exemplo com @po:**

```
@po *create-story
```

---

## 4. Workflow de desenvolvimento (Story Development Cycle)

O ciclo completo recomendado:

```
1. @sm *draft         → Rascunha a story
2. @po *validate      → Valida e refina
3. @dev *develop      → Implementa
4. @qa *qa-gate       → Quality gate
5. @devops *push      → Push para remote
```

**Atalho rápido:**

```
@po *create-story → @dev implementa → @qa testa → @devops *push
```

> ⚠️ **Importante:** Apenas `@devops` pode fazer `git push`. Todos os outros agentes devem delegar essa operação.

---

## 5. Diagnosticando o sistema

```bash
npx aios-core doctor    # Diagnóstico completo
npx aios-core info      # Informações do sistema
npx aios-core install   # Reinstalar se necessário
```

---

## 6. Governança automática — o que verificar

Antes de cada operação crítica, o agente ativo verifica automaticamente:

| Operação                          | Verificação                           |
| --------------------------------- | ------------------------------------- |
| Escrever em `supabase/functions/` | Documentação de arquitetura aprovada? |
| Criar `squads/*/agents/*.md`      | DNA extraído para mind-clones?        |
| Executar SQL DDL                  | Usar `supabase migration` CLI?        |
| `git push`                        | Agente ativo é `@devops`?             |
| Slugs/IDs                         | Formato `snake_case` válido?          |

Para detalhes completos: [Governance Rules](./rules/governance.md)

---

## 7. Usando ferramentas exclusivas

O Antigravit tem capacidades que o Claude Code **não possui**:

```
@ux-design-expert → generate_image (mockups e assets visuais)
@ux-design-expert → mcp_stitch_* (design de UI com Stitch)
@dev → browser_subagent (automação de browser com gravação de vídeo)
```

**Exemplo — gerar mockup:**

```
@ux-design-expert crie um mockup da tela de login do produto X
```

---

## 8. Criando um squad personalizado

Use `@squad-chief` para criar squads especializados para qualquer domínio:

```
@squad-chief quero um squad de marketing digital
```

O squad-chief irá:

1. Pesquisar as mentes de elite do domínio
2. Extrair DNA de cada especialista
3. Criar agentes baseados nas mentes reais
4. Gerar toda a estrutura do squad

> Veja: [Workflow create-squad](./workflows/overview.md#create-squad)

---

## Próximos passos

- [Sistema de Agentes](./agents/overview.md) — Entenda a hierarquia completa
- [Rules e Governance](./rules/overview.md) — Regras de comportamento
- [Mapeamento de Ferramentas](./tools/tool-mapping.md) — Todas as ferramentas disponíveis
- [Templates](./templates/overview.md) — Templates prontos para usar
