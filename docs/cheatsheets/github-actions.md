---
title: Cheatsheet de GitHub Actions
description: Referencia rápida para workflows de GitHub Actions.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Cheatsheet de GitHub Actions

## Workflow e inputs

```yaml
name: CI
on:
  workflow_dispatch:
    inputs:
      environment:
        required: true
        type: choice
        options: [dev, prod]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
```

## Datos y control

```yaml
env: { APP_NAME: demo }
run: echo "${{ vars.APP_NAME }} ${{ github.sha }}"
if: ${{ github.ref_name == 'main' }}
needs: build
strategy:
  matrix: { java: ["17", "21"] }
```

```bash
echo "version=1.2.3" >> "$GITHUB_OUTPUT"
echo "APP_VERSION=1.2.3" >> "$GITHUB_ENV"
echo "# Resultado" >> "$GITHUB_STEP_SUMMARY"
```

## Plataforma

```yaml
permissions: { contents: read, id-token: write }
steps:
  - uses: azure/login@v3
  - uses: actions/setup-java@v6
    with: { distribution: temurin, java-version: "21", cache: maven }
  - uses: actions/setup-node@v7
    with: { node-version: "24", cache: npm }
  - uses: docker/build-push-action@v7
    with: { push: true, tags: ghcr.io/org/app:${{ github.sha }} }
  - uses: actions/upload-artifact@v7
    with: { name: reports, path: target/ }
```

Los secretos se consumen con `${{ secrets.NAME }}`; no los imprimas. Consulta la [guía completa](../devops/cicd/github-actions/index.md) para contexto y seguridad.
