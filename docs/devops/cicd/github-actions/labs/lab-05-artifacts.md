---
title: Lab 05 - Artifacts
description: Transportar un archivo entre jobs con upload y download artifact.
tags: [GitHub Actions, Laboratorios]
---

# Lab 05 — Artifacts

## Objetivo

Resolver de forma explícita el aislamiento observado en Lab 03. Revisa [Artifacts, caché y entornos](../../../../devops/cicd/github-actions/execution/artifacts-cache-and-environments.md).

## Archivo a crear

```yaml
name: Lab 05 - Artifacts
on: {workflow_dispatch: {}}
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "version=1.0.0" > app.txt
      - uses: actions/upload-artifact@v7
        with: {name: application, path: app.txt, retention-days: 1}
  consume:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/download-artifact@v8
        with: {name: application, path: downloaded}
      - run: cat downloaded/app.txt
```

Guarda como `.github/workflows/lab-05.yml`, haz commit/push y ejecútalo manualmente.

## Validación

El run muestra `build` y luego `consume` en verde; el segundo imprime `version=1.0.0`. En la vista del run aparece **Artifacts → application**, descargable desde la UI. Un artifact conserva un resultado de este run; no acelera instalaciones futuras como una cache.

## Reto y limpieza

Escribe un nombre inexistente en `download-artifact`; el job falla porque no encuentra el artifact. Restaura `application`. El artifact expira tras un día; puedes borrarlo desde la UI antes si no lo necesitas. Siguiente: [Lab 06](lab-06-cache.md).
