---
title: Eventos e inputs
description: Triggers, filtros y ejecución manual de workflows.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Eventos e inputs

La clave `on` define qué evento inicia un workflow. Los eventos habituales son `push`, `pull_request`, `workflow_dispatch`, `schedule`, `release`, `workflow_run` y `repository_dispatch`.

## Filtros

Limita la ejecución mediante ramas, tags y rutas para evitar trabajo innecesario.

```yaml
on:
  push:
    branches: [main, "release/**"]
    tags: ["v*"]
    paths:
      - "backend/**"
      - ".github/workflows/backend.yml"
  pull_request:
    branches: [main]
```

`branches-ignore` excluye ramas y `paths-ignore` excluye cambios que no deben activar el flujo; no los combines con sus filtros inclusivos equivalentes para el mismo evento. `schedule` usa cron y normalmente se interpreta en UTC.

## Ejecución manual

`workflow_dispatch` ofrece una ejecución desde GitHub con entradas tipadas.

```yaml
on:
  workflow_dispatch:
    inputs:
      environment:
        description: Ambiente
        required: true
        type: choice
        options: [dev, pre, prod]
      dry_run:
        description: Solo simular
        default: true
        type: boolean
```

Úsalas como `${{ inputs.environment }}`. `workflow_run` encadena workflows terminados y `repository_dispatch` permite que un sistema externo active el flujo mediante API.

!!! warning
    Un input no sustituye controles de autorización ni validación. Evita permitir que una entrada no confiable decida comandos de shell.
