# .github

Configurações e workflows compartilhados da organização ligue-lead-tech.

## Workflows reutilizáveis

### Project Sync (`project-sync-reusable.yml`)

Automatiza a movimentação de cards no [Project Tech](https://github.com/orgs/ligue-lead-tech/projects/12) baseado em eventos de PR.

**O que faz:**

| Evento | Resultado no board |
|--------|--------------------|
| PR marcada Ready for Review | Card → **In Review** + assign do autor na issue |
| PR convertida pra Draft | Card → **In Progress** |
| Owner aprova ou 2+ devs aprovam | Card → **Testing** + deploy na branch configurada |
| PR mergeada | Card → **Done** |

## Como adicionar a um novo repositório

### 1. Criar o workflow

Copie o arquivo `workflow-examples/project-sync.yml` para `.github/workflows/project-sync.yml` no seu repositório:

```yaml
name: Sync PR to Project Board

on:
  pull_request:
    types: [ready_for_review, converted_to_draft, closed]
  pull_request_review:
    types: [submitted]

jobs:
  sync:
    uses: ligue-lead-tech/.github/.github/workflows/project-sync-reusable.yml@main
    secrets: inherit
```

### 2. Criar a configuração (opcional)

Crie `.github/project-sync.json` no repositório:

```json
{
  "deploy_branch": "homolog",
  "owners": ["joaosanches", "luizramosdev"]
}
```

**Campos:**

| Campo | Tipo | Default | Descrição |
|-------|------|---------|-----------|
| `deploy_branch` | `string \| null` | `"homolog"` | Branch para deploy automático após approval. Use `null` para desabilitar deploy. |
| `owners` | `string[]` | `["joaosanches", "luizramosdev"]` | Logins do GitHub cujo approval sozinho move o card pra Testing. Sem ser owner, precisa de 2+ approvals. |

**Exemplos:**

```json
// Repo com deploy em homolog (padrão)
{"deploy_branch": "homolog", "owners": ["joaosanches", "luizramosdev"]}

// Repo sem deploy (ex: bot, lib, infra)
{"deploy_branch": null, "owners": ["joaosanches"]}

// Repo com branch de staging diferente
{"deploy_branch": "staging", "owners": ["joaosanches", "luizramosdev"]}
```

Se o arquivo não existir, usa os defaults (`deploy_branch: "homolog"`, `owners` globais).

### 3. Pronto

O repositório já está integrado ao board. PRs com `closes #<numero>` no body movem o card automaticamente.

## Pré-requisitos

Os secrets `PROJECT_SYNC_APP_ID` e `PROJECT_SYNC_APP_PRIVATE_KEY` estão configurados a nível de organização. Novos repos herdam automaticamente.

## Convenções

Veja o [CONTRIBUTING.md](https://github.com/ligue-lead-tech/areadocliente/blob/master/CONTRIBUTING.md) para padrões de cards, branches e PRs.
