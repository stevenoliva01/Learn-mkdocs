---
title: Comparación con Azure DevOps
description: Equivalencias conceptuales y diferencias de diseño entre Azure DevOps y GitHub Actions.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Comparación con Azure DevOps

La tabla ayuda a trasladar un modelo mental, no a traducir YAML de forma literal. La pregunta útil es qué se reutiliza, qué identidad ejecuta y dónde se aplican controles.

| Azure DevOps | GitHub Actions | Matiz importante |
| --- | --- | --- |
| Pipeline | Workflow | Un workflow puede tener varios jobs |
| Pipeline run | Workflow run | Ejecución concreta de un evento |
| Agent / pool | Runner / group o labels | El aislamiento y administración difieren |
| Task | Action | `uses` invoca una dependencia |
| Script step | `run` | El shell depende del runner |
| `dependsOn` | `needs` | Modelan dependencias entre jobs |
| `condition` | `if` | Ambos usan expresiones propias |
| Variables | `env` / `vars` | Separa proceso y configuración |
| Secret variables | `secrets` | Alcance repository/environment/organization |
| Artifact / cache | Artifact / cache | Misma intención, detalles distintos |
| Environment | Environment | Protecciones dependen de configuración y plan |
| Service Connection | OIDC o secrets/credenciales | OIDC evita secreto persistente cuando aplica |
| Scheduled trigger | `schedule` | Revisa zona horaria y comportamiento |
| Manual run | `workflow_dispatch` | Inputs no equivalen a autorización |

Un YAML template de Azure DevOps no tiene una equivalencia única: un template de jobs se parece a un Reusable Workflow; uno de steps a una Composite Action; un scaffold organizacional a un Workflow Template. `workflow_run` no es el reemplazo normal de `needs`; úsalo solo cuando workflows separados aportan una frontera real.
