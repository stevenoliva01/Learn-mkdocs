---
title: Lab 11 - Environments
description: Registrar despliegues simulados en dev, pre y production.
tags: [GitHub Actions, Laboratorios]
---

# Lab 11 — Environments

## Objetivo

Crear historial de deployments sin infraestructura real. Revisa [Environments](../execution/artifacts-cache-and-environments.md).

Un GitHub **Environment** es un destino con historial y posibles reglas, secretos y variables; no es simplemente el input `environment`. Ese input de tipo `environment` permite seleccionar un nombre, y `jobs.deploy-simulation.environment.name` asocia el job a ese destino. `deploy-simulation` es un job_id creado por el ejemplo: su `name` deja claro que no despliega infraestructura.

## Archivo a crear

```yaml
name: Lab 11 - Environments
on:
  workflow_dispatch:
    inputs:
      environment: {description: Destino, type: environment, required: true}
jobs:
  deploy-simulation:
    name: Record simulated deployment
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
