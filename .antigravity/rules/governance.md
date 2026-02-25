# Governance — Regras de Governança Automática

> **NOTA ARQUITETURAL:** Este arquivo substitui os `PreToolUse Hooks` do Claude Code no ambiente Antigravit.
> Como o Antigravit não possui sistema de interceptação automática de ferramentas, estas regras devem ser
> aplicadas **proativamente** pelo agente antes de cada operação crítica.

---

## Verificações Obrigatórias por Operação

### 🔴 Antes de ESCREVER ou EDITAR código em paths protegidos

**Gatilho:** Qualquer operação `write_to_file` ou `replace_file_content` em:

- `supabase/functions/**`
- `supabase/migrations/**`

**Checklist obrigatório:**

- [ ] Existe documentação aprovada em `docs/architecture/{nome}.md`?
- [ ] OU existe em `docs/approved-plans/{nome}.md`?
- [ ] Se o arquivo já existir — operação de edição é permitida

**Ação se documentação NÃO existe:**

1. Criar documentação de arquitetura primeiro
2. Obter aprovação (notify_user para revisão)
3. ENTÃO retornar à operação de código

**Paths ALWAYS ALLOWED (não requerem doc):**

```
.antigravity/  |  docs/  |  outputs/  |  squads/
.aios-core/    |  .aios-custom/  |  node_modules/  |  .git/
package.json   |  package-lock.json  |  tsconfig.json
.env           |  README.md
```

---

### 🔴 Antes de CRIAR arquivo em `squads/*/agents/*.md`

**Gatilho:** Qualquer `write_to_file` criando novo agente em squads

**Checklist obrigatório:**

- [ ] O agent-id tem sufixo funcional? (`-chief`, `-orchestrator`, `-chair`, `-validator`, `-calculator`, `-generator`, `-extractor`, `-analyzer`, `-architect`, `-mapper`, `-designer`, `-engineer`)
- [ ] OU o agent-id começa com `tools-`, `process-`, `workflow-`?

Se SIM → ✅ Agente funcional, criar normalmente

Se NÃO → VERIFICAR DNA:

- [ ] Existe `squads/{pack}/data/minds/{agent_id}_dna.yaml`?
- [ ] OU existe `squads/{pack}/data/minds/{agent_id}_dna.md`?
- [ ] OU existe `squads/{pack}/data/{agent_id}-dna.yaml`?
- [ ] OU existe `outputs/minds/{agent_id}/`?

Se DNA NÃO existe → **BLOQUEAR. Executar pipeline:**

```
*collect-sources → *extract-voice-dna → *extract-thinking-dna → *create-agent
```

---

### 🔴 Antes de EXECUTAR SQL via run_command

**Gatilho:** Qualquer `run_command` contendo SQL DDL

**Comandos BLOQUEADOS (nunca executar diretamente):**

```sql
CREATE TABLE ...
CREATE VIEW ...
CREATE FUNCTION ...
CREATE TRIGGER ...
ALTER TABLE ...
DROP TABLE ...
DROP VIEW ...
DROP FUNCTION ...
CREATE TABLE AS SELECT ...  -- backup proibido
```

**Comandos PERMITIDOS:**

```bash
supabase migration new {nome}   # Via CLI oficial
supabase db push                # Via CLI oficial
pg_dump ...                     # Backup/export
```

**Ação:** Usar SEMPRE o Supabase CLI para operações de schema.

---

### 🔴 Antes de GIT PUSH

**Gatilho:** Qualquer `run_command` com `git push`

**Verificação:**

- [ ] O agente ativo é `@devops` (Gage)?

Se NÃO → **BLOQUEAR.** Delegar para @devops via:

```
→ @devops *push
```

Se SIM → Verificar quality gates antes de push:

- [ ] `npm run lint` passou?
- [ ] `npm run typecheck` passou?
- [ ] `npm test` passou?

---

### 🟡 Antes de SALVAR documentos (warnings)

**Verificar paths corretos:**

| Tipo de Documento | Path Correto             |
| ----------------- | ------------------------ |
| Sessions/handoffs | `docs/sessions/YYYY-MM/` |
| Arquitetura       | `docs/architecture/`     |
| Guias             | `docs/guides/`           |
| Stories           | `docs/stories/`          |
| Approved plans    | `docs/approved-plans/`   |

**Ação:** Se o documento parece estar no path errado, alertar antes de salvar.

---

### 🟡 Formato de Slugs — Validação Obrigatória

**Regra:** Todos os slugs e IDs DEVEM ser snake_case

```
Padrão válido: ^[a-z0-9]+(_[a-z0-9]+)*$
```

| Exemplo              | Status                  |
| -------------------- | ----------------------- |
| `jose_carlos_amorim` | ✅ VÁLIDO               |
| `workflow_execution` | ✅ VÁLIDO               |
| `jose-carlos-amorim` | ❌ INVÁLIDO (hífen)     |
| `JoseAmorim`         | ❌ INVÁLIDO (camelCase) |
| `jose.carlos`        | ❌ INVÁLIDO (ponto)     |

---

### 🔵 Leitura Parcial de Arquivos Protegidos

**Arquivos que NUNCA devem ser lidos parcialmente (com StartLine/EndLine em contextos críticos):**

- `.antigravity/ANTIGRAVITY.md`
- `.antigravity/rules/*.md`
- `.aios-core/development/agents/*.md`
- `supabase/docs/SCHEMA.md`
- `package.json`, `tsconfig.json`

**Regra:** Sempre ler o arquivo completo quando estiver tomando decisões sobre estes arquivos.

---

## Exit Codes de Referência (Claude Code → Antigravit)

> Para referência histórica. No Antigravit, a governança é instrucional, não via exit codes.

| Exit Code Claude | Equivalente Antigravit                          |
| ---------------- | ----------------------------------------------- |
| 0 — Permitido    | Prosseguir normalmente                          |
| 2 — Bloqueado    | Auto-bloquear + alertar usuário via notify_user |
| Outro — Aviso    | Log interno + continuar                         |

---

## Scripts de Validação Manual

Os scripts Python originais em `.claude/hooks/` podem ser executados manualmente para validação:

```bash
# Validar architecture-first
echo '{"tool_name": "Write", "tool_input": {"file_path": "supabase/functions/minha-funcao/index.ts"}}' | python3 .claude/hooks/enforce-architecture-first.py

# Validar mind-clone governance
echo '{"tool_name": "Write", "tool_input": {"file_path": "squads/meu-squad/agents/gary-halbert.md"}}' | python3 .claude/hooks/mind-clone-governance.py

# Validar slug
echo '{"tool_name": "Bash", "tool_input": {"command": "criar slug jose-carlos"}}' | python3 .claude/hooks/slug-validation.py
```
