---
title: Comparación con Azure DevOps
description: Equivalencias conceptuales entre Azure DevOps y GitHub Actions.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Comparación con Azure DevOps

| Azure DevOps | GitHub Actions |
| --- | --- |
| Pipeline | Workflow |
| Job | Job |
| Task | Action |
| Agent | Runner |
| Variable | `env` / `vars` |
| Secret variable | `secrets` |
| `dependsOn` | `needs` |
| `condition` | `if` |
| Pipeline template | Reusable workflow |
| Task group | Composite Action |

La equivalencia sirve para migrar el modelo mental, no para replicar literalmente cada concepto. GitHub Actions no necesita copiar el modelo de Stages de Azure DevOps: jobs, `needs`, environments y workflows reutilizables permiten modelar el flujo según la necesidad.
