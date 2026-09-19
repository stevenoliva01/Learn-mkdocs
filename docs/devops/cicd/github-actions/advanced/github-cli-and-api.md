---
title: GitHub CLI y API
description: Controlar GitHub desde workflows mediante GITHUB_TOKEN y permisos mínimos.
tags: [GitHub Actions, GitHub CLI, API, DevOps]
---

# GitHub CLI y API

Un workflow puede necesitar consultar un repositorio, crear una release o registrar información sobre un issue. Antes de añadir una credencial permanente, pregunta si el token temporal del run alcanza.

```text
Workflow → GITHUB_TOKEN → gh / REST API → GitHub API
```

`GITHUB_TOKEN` se emite para el run y sus capacidades dependen de `permissions`. Expónlo como `GH_TOKEN` para `gh`, no como un valor impreso en logs:

```yaml
permissions:
  contents: read
jobs:
  inspect-repository:
    runs-on: ubuntu-latest
    steps:
      - name: Read repository metadata
        env:
          GH_TOKEN: ${{ github.token }}
        run: gh api repos/${{ github.repository }} --jq '.full_name'
```

`gh api` llama la API REST; `gh issue` y `gh release` dan interfaces de alto nivel para sus recursos. Para crear o modificar algo, concede solo el permiso específico requerido y limita el trigger a código confiable. Por ejemplo, una release necesita `contents: write`; no conviertas esa necesidad en `write-all`, ni uses un PAT si `GITHUB_TOKEN` cumple el caso.

## Diseñar una llamada segura

1. Define el recurso y la operación: leer metadata es distinto de publicar una release.
2. Declara `permissions` mínimas al workflow o job.
3. Pasa datos no confiables por `env` y valida listas permitidas; no interpolar títulos de PR o payloads como shell.
4. Haz la operación idempotente o detecta la condición previa.
5. Añade un summary seguro y conserva el enlace al run para auditoría.

La API tiene límites de tasa y políticas que cambian; diseña paginación, reintento acotado ante fallos transitorios y evita loops de workflows que se disparan mutuamente. Usa `gh api --method GET` para inspección y consulta la referencia del endpoint antes de una escritura. El [Lab 22](../labs/lab-22-github-cli-and-api.md) se limita por defecto a una lectura.
