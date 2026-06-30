# .github

Configurações e workflows compartilhados da organização ligue-lead-tech.

## Workflows reutilizáveis

### Project Sync (`project-sync-reusable.yml`)

Automatiza a movimentação de cards no [Project Tech](https://github.com/orgs/ligue-lead-tech/projects/12) baseado em eventos de PR.

**O que faz:**

| Evento | Resultado no board |
|--------|--------------------|
| PR marcada Ready for Review | Card → **In Review** + assign do autor na issue |
| Owner aprova ou 2+ devs aprovam | Card → **Testing** + deploy na branch `homolog` (se existir) |
| PR mergeada | Card → **Done** |

**Conflitos:** se o merge na homolog falhar, o bot comenta na PR com instruções pra resolver manualmente.

## Como adicionar a um novo repositório

1. Crie o arquivo `.github/workflows/project-sync.yml` no repositório:

```yaml
name: Sync PR to Project Board

on:
  pull_request:
    types: [ready_for_review, closed]
  pull_request_review:
    types: [submitted]

jobs:
  sync:
    uses: ligue-lead-tech/.github/.github/workflows/project-sync-reusable.yml@main
    secrets: inherit
```

2. Pronto. O repositório já está integrado ao board.

## Pré-requisitos

Os secrets `PROJECT_SYNC_APP_ID` e `PROJECT_SYNC_APP_PRIVATE_KEY` devem estar configurados a nível de **organização** (Settings → Secrets → Actions). Novos repos herdam automaticamente.

## Convenções

Veja o [CONTRIBUTING.md](https://github.com/ligue-lead-tech/areadocliente/blob/master/CONTRIBUTING.md) para padrões de cards, branches e PRs.
