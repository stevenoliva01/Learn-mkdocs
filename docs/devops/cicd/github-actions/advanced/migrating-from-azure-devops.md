---
title: Migrar desde Azure DevOps
description: Migración gradual de Azure Pipelines a GitHub Actions con revisión humana e Importer.
tags: [GitHub Actions, Azure DevOps, Migration, DevOps]
---

# Migrar desde Azure DevOps

Migrar no es traducir YAML línea a línea. Es reconstruir un sistema de automatización, identidades y controles de entrega en un modelo distinto.

```text
Inventario → Clasificación → Dependencias → Identidades → Runners → Templates
→ Artifacts → Environments → Migración → Validación → Cutover
```

Empieza por inventariar pipelines, triggers, variables, service connections, agent pools, tasks, artifacts, approvals y dependencias externas. Clasifica cada flujo por criticidad y nivel de automatización posible; migra primero CI reproducible y conserva una vía de rollback durante el cutover. No promociones producción hasta comparar resultados, permisos y evidencias.

| Azure DevOps | GitHub Actions | Matiz |
| --- | --- | --- |
| Pipeline / Pipeline Run | Workflow / Workflow Run | El run nace de un evento. |
| Stage | Sin equivalencia única | Puede ser jobs con `needs`, environments o workflows separados. |
| Job / Step | Job / Step | `needs` expresa el grafo. |
| Task / Script | Action / `run` | Evalúa mantenimiento y permisos de toda Action. |
| Agent / Agent Pool | Runner / runner group y labels | La frontera de confianza puede cambiar. |
| `dependsOn` / `condition` | `needs` / `if` | Revisa semántica de expresiones y fallos. |
| Variables / secret variables | `env` o `vars` / `secrets` | Conserva el menor alcance. |
| Variable group | `vars` y `secrets` según caso | No hay mapeo automático universal. |
| Templates | Reusable workflow, Composite Action o template | Elige según comparta jobs, steps o scaffold. |
| Artifact / Cache | Artifact / Cache | Un artifact no es una cache. |
| Environment / approvals | Environment y reglas de protección | Dependen de configuración y plan. |
| Service connection | OIDC o credenciales | Prefiere identidad federada cuando aplique. |
| Scheduled/manual trigger | `schedule` / `workflow_dispatch` | Revalida cron y controles. |

## GitHub Actions Importer: acelerar, no delegar el diseño

El Importer actual se instala como extensión de GitHub CLI y requiere Docker en ejecución, GitHub CLI y credenciales para Azure DevOps. Sus comandos importantes son `audit`, `forecast`, `dry-run` y `migrate`:

```bash
gh extension install github/gh-actions-importer
gh actions-importer configure
gh actions-importer audit azure-devops --output-dir tmp/audit
gh actions-importer forecast azure-devops --output-dir tmp/forecast
gh actions-importer dry-run azure-devops pipeline --pipeline-id PIPELINE_ID --output-dir tmp/dry-run
```

`audit` mide conversión y complejidad; `forecast` estima uso según históricos; `dry-run` produce archivos para revisar sin abrir un PR. `migrate` puede convertir y abrir un PR en un repositorio objetivo, por lo que debe ocurrir solo tras una revisión explícita. La salida no migra automáticamente secrets, service connections, agentes self-hosted, environments ni aprobaciones; además, gates y tareas desconocidas pueden exigir rediseño manual. Custom transformers pueden ayudar con transformaciones repetidas, pero requieren pruebas y revisión de seguridad.

El [Lab 23](../labs/lab-23-migrating-from-azure-devops.md) practica una conversión manual y deja Importer en modo de análisis/dry-run, sin abrir PR ni cambiar un repositorio remoto.
