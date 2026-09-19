---
title: Lab 08 - Concurrency
description: Cancelar un despliegue simulado obsoleto para un mismo ambiente.
tags: [GitHub Actions, Laboratorios]
---

# Lab 08 — Concurrency

## Objetivo

Simular dos despliegues hacia el mismo recurso lógico y observar la cancelación del anterior. Consulta [Estrategia y control](../execution/strategy-and-control.md).

`concurrency` controla runs que comparten una clave. `group` es la clave de exclusión mutua; aquí concatena el texto elegido `lab-deploy-` con el input `environment`, de modo que `pre` y `dev` no compiten. `cancel-in-progress` decide si el run anterior se cancela. `deploy-simulation` es solo un job_id pedagógico: no realiza un deploy.

## Archivo a crear

```yaml
name: Lab 08 - Concurrency
on:
  workflow_dispatch:
    inputs:
      environment: {description: Ambiente, type: choice, options: [pre, dev], required: true}
concurrency:
  group: lab-deploy-${{ inputs.environment }}
  cancel-in-progress: true
jobs:
  deploy-simulation:
    name: Simulate deployment
    runs-on: ubuntu-latest
    steps:
      - run: |
          echo "Simulando deploy a ${{ inputs.environment }}"
          sleep 90
          echo "Simulación terminada"
```

## Ejecutar y validar

Inicia manualmente dos runs hacia `pre` con pocos segundos de diferencia. El segundo queda activo y el primero aparece cancelado. Lanza un tercero hacia `dev`: puede ejecutarse en paralelo porque el grupo es distinto.

## Reto y limpieza

Cambia `cancel-in-progress` a `false` y observa que los dos runs se serializan en vez de cancelarse. En producción no decidas cancelar un apply de Terraform sin diseñar recuperación. Elimina o desactiva el workflow al terminar. Siguiente: [Lab 09](lab-09-reusable-workflows.md).
