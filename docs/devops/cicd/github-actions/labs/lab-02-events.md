---
title: Lab 02 - Eventos
description: Comparar workflow_dispatch, push y pull_request mediante contexts observables.
tags: [GitHub Actions, Laboratorios]
---

# Lab 02 — Eventos

## Objetivo

Observar por qué se crea un run y cómo cambian `github.event_name`, `ref`, `ref_name`, `sha` y `actor`. Consulta [Eventos e inputs](../../../../devops/cicd/github-actions/fundamentals/events-and-inputs.md).

## Archivo a crear

Crea `.github/workflows/lab-02.yml`:

```yaml
name: Lab 02 - Eventos
on:
  workflow_dispatch:
  push:
    branches: [main, "lab/**"]
  pull_request:
    branches: [main]
jobs:
  inspect:
    runs-on: ubuntu-latest
    steps:
      - run: |
          echo "event_name=${{ github.event_name }}"
          echo "ref=${{ github.ref }}"
          echo "ref_name=${{ github.ref_name }}"
          echo "sha=${{ github.sha }}"
          echo "actor=${{ github.actor }}"
```

## Ejecutar el laboratorio

Publica el archivo en `main`. Ejecútalo manualmente; después provoca un push y un PR:

```bash
git checkout -b lab/events
echo "evento" >> README.md
git add README.md .github/workflows/lab-02.yml
git commit -m "lab: observe push event"
git push -u origin lab/events
```

Abre un Pull Request de `lab/events` hacia `main` desde GitHub. Completa mentalmente esta tabla desde los logs:

| Ejecución | `event_name` | `ref` | `ref_name` |
| --- | --- | --- | --- |
| Manual | | | |
| Push | | | |
| Pull request | | | |

## Validación, reto y limpieza

Debes ver tres runs con el mismo job y valores de contexto distintos. Cambia el filtro a `branches: [trunk]`, haz push a `lab/events` y observa que no hay run; restaura el filtro. Cierra el PR y elimina la rama remota cuando termines. Continúa con [Lab 03](lab-03-jobs-and-needs.md).
