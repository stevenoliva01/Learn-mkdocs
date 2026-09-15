---
title: Lab 10 - Composite Actions
description: Crear e invocar una Action local que agrupa steps.
tags: [GitHub Actions, Laboratorios]
---

# Lab 10 — Composite Actions

## Objetivo

Reutilizar steps dentro de un job y distinguirlo de reutilizar jobs. Consulta [Composite Actions](../../../../devops/cicd/github-actions/reuse/composite-actions.md).

## Archivos a crear

Crea `.github/actions/project-info/action.yml`:

```yaml
name: Project info
description: Muestra datos controlados del proyecto
inputs:
  application: {required: true, description: Nombre de aplicación}
outputs:
  label: {description: Etiqueta generada, value: ${{ steps.label.outputs.value }}}
runs:
  using: composite
  steps:
    - id: label
      shell: bash
      run: |
        echo "value=${{ inputs.application }}-${GITHUB_SHA::7}" >> "$GITHUB_OUTPUT"
        echo "Aplicación: ${{ inputs.application }}"
```

Crea `.github/workflows/lab-10.yml`:

```yaml
name: Lab 10 - Composite Action
on: {workflow_dispatch: {}}
jobs:
  use-action:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - id: info
        uses: ./.github/actions/project-info
        with: {application: payments-api}
      - run: echo "Etiqueta=${{ steps.info.outputs.label }}"
```

## Validación y troubleshooting

El run imprime nombre y etiqueta con SHA corto. Si falla `uses: ./.github/...`, comprueba que hubo checkout: una Action local está en el repositorio y el runner empieza vacío. Reusable Workflow reutiliza jobs; Composite Action reutiliza steps. Elimina ambos archivos si no deseas conservarlos. Siguiente: [Lab 11](lab-11-environments.md).
