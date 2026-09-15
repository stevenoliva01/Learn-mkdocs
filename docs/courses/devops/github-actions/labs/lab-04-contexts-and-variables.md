---
title: Lab 04 - Contexts y variables
description: Usar inputs, env y contexts sin exponer secretos.
tags: [GitHub Actions, Laboratorios]
---

# Lab 04 — Contexts, variables e inputs

## Objetivo

Lanzar una operación simulada parametrizada y distinguir expresión de Actions y variable de shell. Consulta [Variables, contextos y expresiones](../../../../devops/cicd/github-actions/fundamentals/variables-contexts-and-expressions.md).

## Archivo a crear

```yaml
name: Lab 04 - Datos del workflow
on:
  workflow_dispatch:
    inputs:
      environment: {description: Ambiente, required: true, type: choice, options: [dev, pre]}
      version: {description: Versión, required: true, type: string}
      dry_run: {description: Simular, type: boolean, default: true}
env:
  LAB_NAME: contexts-and-variables
jobs:
  inspect:
    runs-on: ubuntu-latest
    env:
      ENVIRONMENT: ${{ inputs.environment }}
    steps:
      - run: |
          echo "input=${{ inputs.environment }}"
          echo "shell=$ENVIRONMENT"
          echo "version=${{ inputs.version }} dry_run=${{ inputs.dry_run }}"
          echo "event=${{ github.event_name }} runner=${{ runner.os }} lab=$LAB_NAME"
```

Guárdalo como `.github/workflows/lab-04.yml`, publícalo y ejecútalo con `pre`, `1.0.0` y `dry_run=true`.

## Validación y seguridad

El log muestra inputs, contexts y `env`; no muestra secretos. `${{ inputs.environment }}` se resuelve antes de ejecutar el step; `$ENVIRONMENT` lo expande Bash. Nunca reemplaces los `echo` por secrets para comprobarlos. Prueba `dev` y compara el log. No hay cleanup. Continúa con [Lab 05](lab-05-artifacts.md).
