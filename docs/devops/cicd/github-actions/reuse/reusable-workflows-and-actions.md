---
title: Workflows y actions reutilizables
description: Reutilización de jobs, steps y componentes de automatización.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Workflows y actions reutilizables

Un **reusable workflow** reutiliza jobs o workflows completos. Una **composite action** reutiliza una secuencia de steps. Una custom action puede implementarse con JavaScript, Docker o composición.

## Reusable workflow

El workflow llamado expone `workflow_call` con inputs y secretos; el caller lo invoca mediante `uses`.

```yaml
on:
  workflow_call:
    inputs:
      java-version:
        required: true
        type: string
    secrets:
      token:
        required: true
```

## Composite action

Una action local o publicada declara `action.yml` y reúne steps repetidos.

```yaml
name: Build Java
runs:
  using: composite
  steps:
    - run: mvn -B verify
      shell: bash
```

Usa reusable workflows para políticas y flujos completos; actions compuestas para operaciones pequeñas y repetibles. Documenta contratos, limita secretos, mantén versiones y fija actions externas por SHA cuando el requisito de seguridad lo demande.

La reutilización centralizada forma parte del [gobierno empresarial](../advanced/repository-governance-and-enterprise.md).
