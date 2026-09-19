---
title: Lab 03 - Jobs y needs
description: Observar paralelismo, dependencias y aislamiento entre jobs.
tags: [GitHub Actions, Laboratorios]
---

# Lab 03 — Jobs, aislamiento y `needs`

## Objetivo

Demostrar que los jobs independientes pueden correr en paralelo y no comparten filesystem. Lee [Modelo de ejecución y aislamiento](../fundamentals/execution-model-and-isolation.md).

`build`, `independent` y `deploy-simulation` son **job_id** elegidos por el autor; sus `name` opcionales hacen el gráfico legible. `needs: build` declara una dependencia de orden hacia ese id: no comparte su disco ni mueve `app.txt`. Esta distinción es el punto de partida del Lab 05.

## Archivo a crear

Crea `.github/workflows/lab-03.yml`:

```yaml
name: Lab 03 - Jobs y needs
on: {workflow_dispatch: {}}
jobs:
  build:
    name: Create isolated file
    runs-on: ubuntu-latest
    steps:
      - run: echo "artifact" > app.txt
      - run: cat app.txt
  independent:
    name: Run independently
    runs-on: ubuntu-latest
    steps:
      - run: sleep 10; echo "Puede correr junto a build"
  deploy-simulation:
    name: Verify a different runner
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
