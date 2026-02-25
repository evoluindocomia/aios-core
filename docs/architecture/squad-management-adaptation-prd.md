# PRD — Adaptação do Sistema de Squads para Antigravit

Este documento detalha o plano para corrigir e adaptar o sistema de criação e gestão de squads, que sofreu uma "migração parcial" do Claude Code para o Antigravit.

## Contexto e Problema

O sistema original (Claude Code) dependia fortemente de scripts Python e comandos slash (`/cohort-squad`) para gerenciar squads. Na migração atual:

1.  O agente `@squad-chief` foi migrado, mas aponta para diretórios inexistentes (`squads/squad-creator/`).
2.  A lógica de criação de squads está "presa" no core do framework (`.aios-core/`), enquanto o design do Antigravit prevê que squads sejam self-contained e orquestrados por um squad mestre.
3.  O pipeline de clonagem de mentes (DNA Mental™) cita agendes core (Victoria, Tim, Daniel) que não existem no ambiente Antigravit atual.
4.  Comandos críticos como sincronização com Synkra API estão faltando no Antigravit.

## Objetivos

- **Consolidar o `squad-creator`**: Transformar a lógica de criação de squads em um squad de projeto real em `squads/squad-creator/`.
- **Desacoplar do Framework**: Permitir que o processo de criação de squads use ferramentas exclusivas do Antigravit (`generate_image`, `browser_subagent`).
- **Restaurar Pipeline DNA**: Implementar os agentes especialistas necessários para a clonagem cognitiva de alta fidelidade.
- **Implementar Sync**: Restaurar a capacidade de sincronização com o Marketplace Synkra via API.

## Mudanças Propostas

### 1. Estrutura do Squad Orquestrador

Criar `squads/squad-creator/` com:

- `squad.yaml`: Manifesto definindo o squad.
- `agents/oalanicolas.md`: Arquiteto de DNA (mind cloning).
- `agents/pedro-valerio.md`: Absolutista de processos (validação).
- `agents/research-specialists.md`: (Novo) Consolidação de Victoria, Tim e Daniel para pesquisa e análise.
- `tasks/create-squad.md`: Baseado na lógica core, mas adaptado para o novo ambiente.
- `tasks/sync-squads.md`: Utiliza `run_command` (curl) ou `read_url_content` para interagir com a API Synkra.

### 2. Atualização de Dependências

- Atualizar `.antigravity/agents/squad-chief.md` para apontar corretamente para `squads/squad-creator/`.
- Atualizar `.antigravity/skills/squad/SKILL.md` e `clone-mind/SKILL.md`.

### 3. Sincronização Synkra (Sync)

Implementar `tasks/sync-squads.md` que:

1. Valida o `squad.yaml` local.
2. Faz POST para `https://api.synkra.ai/api/squads/sync`.
3. Retorna a URL do marketplace.

## Road Map de Execução

1. **Setup**: Criar diretórios de `squad-creator`.
2. **Mind Cloning**: Migrar e adaptar `oalanicolas.md` — o coração da clonagem.
3. **Creation Flow**: Adaptar a lógica de geração de diretórios e arquivos.
4. **Sync Flow**: Implementar a comunicação com a API REST.
5. **Validação**: Testar a criação de um squad de exemplo usando o novo fluxo.

---

> [!IMPORTANT]
> Esta mudança é crítica para que o Antigravit não seja apenas um "bot de chat", mas um sistema capaz de **gerar outros sistemas** (squads especializados).
