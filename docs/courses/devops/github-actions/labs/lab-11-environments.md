---
title: Lab 11 - Environments
description: Registrar despliegues simulados en dev, pre y production.
tags: [GitHub Actions, Laboratorios]
---

# Lab 11 — Environments

## Objetivo

Crear historial de deployments sin infraestructura real. Revisa [Environments](../../../../devops/cicd/github-actions/execution/artifacts-cache-and-environments.md).

## Archivo a crear

```yaml
name: Lab 11 - Environments
on:
  workflow_dispatch:
    inputs:
      environment: {description: Destino, type: environment, required: true}
jobs:
  deploy-simulation:
    runs-on: ubuntu-latest
    environment:
      name: ${{ inputs.environment }}
    steps:
      - run: |
          echo "Deploying to ${{ inputs.environment }}"
          echo "No se creó infraestructura"
```

Guárdalo como `.github/workflows/lab-11.yml`. Ejecuta primero con `dev`, luego crea/elige `pre` y `production` desde el selector.

## Validación, límites y limpieza

En Actions debe aparecer el job y en **Settings → Environments** el historial de deployment para cada destino. Protecciones, reviewers y reglas dependen del plan y configuración: no asumas que están disponibles. El input no autoriza un deploy real. Borra los environments de prueba desde Settings si no quieres conservar el historial. Continúa con [Lab 12](lab-12-docker-ghcr.md).
