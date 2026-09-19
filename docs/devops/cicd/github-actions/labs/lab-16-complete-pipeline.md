---
title: Lab 16 - Pipeline completo
description: Integrar CI, artifact y promoción simulada en un workflow final.
tags: [GitHub Actions, Laboratorios, CI/CD]
---

# Lab 16 — Pipeline completo

## Objetivo

Unir lo aprendido en una pipeline reproducible: un PR valida, `main` construye un artifact y un trigger manual simula promoción. No publica imágenes ni crea cloud. Repasa [Pipelines](../pipelines/index.md).

Este cierre combina conceptos ya introducidos: triggers, `permissions`, `needs`, artifact, `if` y Environment. `ci`, `deploy-dev-simulation` y `deploy-manual-simulation` son job_id elegidos; sus `name` visibles aclaran la intención. `if` filtra cada job según `github.event_name`; `needs: ci` impide la promoción si CI no termina correctamente. `environment: dev` o `${{ inputs.environment }}` registra el destino, pero no crea infraestructura.

## Archivos a crear

Reutiliza los archivos de Node del Lab 13 y crea `.github/workflows/lab-16.yml`:

```yaml
name: Lab 16 - Pipeline completo
on:
  pull_request: {branches: [main]}
  push: {branches: [main]}
  workflow_dispatch:
    inputs:
      environment: {description: Destino simulado, type: choice, options: [dev, pre], required: true}
permissions: {contents: read}
jobs:
  ci:
    name: Validate and package
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - uses: actions/setup-node@v7
        with: {node-version: "24", package-manager-cache: false}
      - run: npm run lint && npm test && npm run build
      - uses: actions/upload-artifact@v7
        with: {name: quality-lab, path: "index.js\ntest.js", retention-days: 1}
  deploy-dev-simulation:
    name: Simulate dev deployment
    if: ${{ github.event_name == 'push' }}
    needs: ci
    runs-on: ubuntu-latest
    environment: dev
    steps: [{run: echo "Deploy simulado a dev desde $GITHUB_SHA"}]
  deploy-manual-simulation:
    name: Simulate selected environment promotion
    if: ${{ github.event_name == 'workflow_dispatch' }}
    needs: ci
    runs-on: ubuntu-latest
    environment: ${{ inputs.environment }}
    steps: [{run: echo "Promoción manual simulada a ${{ inputs.environment }}"}]
```

## Ejecutar y validar

Abre un PR que cambie `index.js`: observa `ci`. Haz merge a `main`: observa `ci`, artifact `quality-lab` y `deploy-dev-simulation`. Ejecuta manualmente con `pre`: observa `deploy-manual-simulation`. Si un test falla, ningún deploy comienza porque depende de `ci`.

## Reto, cleanup y cierre

Elimina `needs: ci` temporalmente para observar por qué un deploy no debe depender de una suposición de éxito; restáuralo. Borra artifacts, environments de prueba y workflows cuando termine la práctica. Un siguiente repositorio `github-actions-labs` puede extraer estos archivos, añadir Docker/GHCR y convertir los Labs 14–15 en variantes opcionales con credenciales externas, sin convertir este MkDocs en un repositorio ejecutable.
