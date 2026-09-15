---
title: Lab 03 - Jobs y needs
description: Observar paralelismo, dependencias y aislamiento entre jobs.
tags: [GitHub Actions, Laboratorios]
---

# Lab 03 — Jobs, aislamiento y `needs`

## Objetivo

Demostrar que los jobs independientes pueden correr en paralelo y no comparten filesystem. Lee [Modelo de ejecución y aislamiento](../../../../devops/cicd/github-actions/fundamentals/execution-model-and-isolation.md).

## Archivo a crear

Crea `.github/workflows/lab-03.yml`:

```yaml
name: Lab 03 - Jobs y needs
on: {workflow_dispatch: {}}
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "artifact" > app.txt
      - run: cat app.txt
  independent:
    runs-on: ubuntu-latest
    steps:
      - run: sleep 10; echo "Puede correr junto a build"
  deploy-simulation:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: test ! -f app.txt
      - run: echo "El archivo no existe: otro job tiene otro runner"
```

## Ejecutar y observar

Haz commit, push y ejecútalo manualmente. En el gráfico del run, `build` e `independent` comienzan sin esperar; `deploy-simulation` inicia tras `build`. El resultado debe ser verde y confirmar que `app.txt` no existe en el job final.

## Reto, troubleshooting y limpieza

Sustituye `test ! -f app.txt` por `cat app.txt`: el job debe fallar con archivo inexistente. No es un problema de `needs`; `needs` ordena jobs, no mueve archivos. Restaura el YAML. El Lab 05 resolverá este problema con artifact. Elimina el workflow si no quieres conservarlo y continúa con [Lab 04](lab-04-contexts-and-variables.md).
