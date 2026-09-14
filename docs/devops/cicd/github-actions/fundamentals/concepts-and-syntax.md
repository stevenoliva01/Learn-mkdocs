---
title: Conceptos y sintaxis de GitHub Actions
description: Modelo mental y estructura de un workflow.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Conceptos y sintaxis de GitHub Actions

GitHub Actions reacciona a eventos del repositorio para ejecutar automatizaciones declaradas en YAML.

## Modelo mental

```text
Evento
  |
  +-- Workflow
       |
       +-- Job
            |
            +-- Step
```

- **Workflow**: automatización completa en `.github/workflows/`.
- **Event**: hecho que inicia el workflow, como `push` o `pull_request`.
- **Job**: unidad que corre en un runner; los jobs independientes pueden ser paralelos.
- **Step**: instrucción secuencial de un job, con `run` o `uses`.
- **Action**: componente reutilizable, por ejemplo `actions/checkout@v7`.
- **Runner**: máquina que ejecuta un job.

## Workflow mínimo

```yaml
name: CI

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - run: echo "Hola GitHub Actions"
```

Los elementos de nivel superior más habituales son `name`, `run-name`, `on`, `permissions`, `env`, `defaults`, `concurrency` y `jobs`.

## YAML necesario

Los mapas agrupan claves y valores; las listas se indican con guiones. Usa `|` para varios comandos y comillas cuando un valor pueda interpretarse de forma inesperada.

```yaml
env:
  VERSION: "1.0"
  ENABLED: "true"

run: |
  echo "compilar"
  echo "probar"
```

!!! tip
    Mantén un workflow con una responsabilidad clara. El YAML debe describir el flujo, no sustituir una aplicación completa de scripts.
