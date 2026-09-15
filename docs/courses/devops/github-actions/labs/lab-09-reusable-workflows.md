---
title: Lab 09 - Reusable Workflows
description: Llamar el mismo workflow desde dos callers.
tags: [GitHub Actions, Laboratorios]
---

# Lab 09 — Reusable Workflows

## Objetivo

Reutilizar un job completo mediante `workflow_call`, sin secretos reales. Lee [Reusable Workflows](../../../../devops/cicd/github-actions/reuse/reusable-workflows.md).

## Archivos a crear

Crea `.github/workflows/lab-09-reusable.yml`:

```yaml
name: Lab 09 - Reusable
on:
  workflow_call:
    inputs:
      application: {required: true, type: string}
    outputs:
      artifact-name: {value: ${{ jobs.report.outputs.artifact-name }} }
jobs:
  report:
    runs-on: ubuntu-latest
    outputs:
      artifact-name: ${{ steps.name.outputs.value }}
    steps:
      - id: name
        run: echo "value=${{ inputs.application }}-report" >> "$GITHUB_OUTPUT"
      - run: echo "Validando ${{ inputs.application }}"
```

Crea dos callers, `.github/workflows/lab-09-api.yml` y `lab-09-web.yml`, cambiando solo `application`:

```yaml
name: Lab 09 - API caller
on: {workflow_dispatch: {}}
jobs:
  reusable:
    uses: ./.github/workflows/lab-09-reusable.yml
    with: {application: payments-api}
  show-output:
    needs: reusable
    runs-on: ubuntu-latest
    steps:
      - run: echo "${{ needs.reusable.outputs.artifact-name }}"
```

Para `web`, usa el mismo contenido con `name: Lab 09 - Web caller` y `application: payments-web`.

## Validación y reto

Ejecuta ambos callers: cada uno invoca el workflow común y muestra un output diferente. Rompe el nombre de input en un caller para observar el fallo de contrato; restáuralo. No hay secretos ni cleanup. Sigue con [Lab 10](lab-10-composite-actions.md).

## Opcional — Workflow Template organizacional

No requiere crear una organización para entenderlo: prepara un futuro `workflow-templates/java-ci.yml` y su `.properties.json` solo cuando una organización disponga de repositorio `.github`. Un template genera un caller inicial; no reemplaza este reusable workflow en runtime. Consulta [Workflow Templates](../../../../devops/cicd/github-actions/reuse/workflow-templates.md).
