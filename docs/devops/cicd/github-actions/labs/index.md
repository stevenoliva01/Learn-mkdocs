---
title: Prácticas guiadas de GitHub Actions
description: 24 laboratorios reproducibles para ejecutar, observar y depurar GitHub Actions.
tags: [GitHub Actions, CI/CD, DevOps, Laboratorios]
---

# Laboratorios de GitHub Actions

Estos 24 laboratorios se ejecutan en un repositorio GitHub que crea el alumno. Este sitio no contiene workflows activos: cada página indica qué crear, por qué existe y qué observar en Actions. Los identificadores como `build`, `validate` o `use-artifact` los elige quien escribe el YAML; no son keywords.

## Antes de empezar

Necesitas una cuenta GitHub, Git instalado y un repositorio vacío propio. Crea una carpeta local, inicialízala, añade un `README.md` y publica la rama `main`. Los Labs 12 y 14 requieren activar características externas; los Labs 14 y 15 son avanzados. Nunca pegues secretos reales en YAML.

## Ruta recomendada

```text
Básico:      01 → 02 → 03 → 04
Intermedio:  05 → 06 → 07 → 08 → 09 → 10
CI/CD:       11 → 12 → 13
Cloud:       14 → 15
Integración: 16
Operación:   17 → 18 → 19 → 20 → 21 → 22 → 23 → 24
```

| Lab | Tema | Nivel | Servicio externo |
| --- | --- | --- | --- |
| [Lab 01](lab-01-first-workflow.md) | Primer workflow | Básico | No |
| [Lab 02](lab-02-events.md) | Eventos | Básico | No |
| [Lab 03](lab-03-jobs-and-needs.md) | Jobs y `needs` | Básico | No |
| [Lab 04](lab-04-contexts-and-variables.md) | Contextos y variables | Básico | No |
| [Lab 05](lab-05-artifacts.md) | Artifacts | Intermedio | No |
| [Lab 06](lab-06-cache.md) | Cache | Intermedio | No |
| [Lab 07](lab-07-matrix.md) | Matrix | Intermedio | No |
| [Lab 08](lab-08-concurrency.md) | Concurrency | Intermedio | No |
| [Lab 09](lab-09-reusable-workflows.md) | Reusable workflows | Intermedio | No |
| [Lab 10](lab-10-composite-actions.md) | Composite actions | Intermedio | No |
| [Lab 11](lab-11-environments.md) | Environments | Intermedio | No |
| [Lab 12](lab-12-docker-ghcr.md) | Docker y GHCR | Intermedio | GitHub Packages |
| [Lab 13](lab-13-quality-gates.md) | Quality gates | Intermedio | No |
| [Lab 14](lab-14-azure-oidc.md) | Azure OIDC | Avanzado | Azure |
| [Lab 15](lab-15-terraform.md) | Terraform | Avanzado | Terraform CLI; cloud opcional |
| [Lab 16](lab-16-complete-pipeline.md) | Pipeline completo | Avanzado | No; GHCR opcional |
| [Lab 17](lab-17-monitoring-logging-and-debugging.md) | Monitoring y debugging | Intermedio | No |
| [Lab 18](lab-18-workflow-templates.md) | Workflow Templates | Intermedio | Organización opcional |
| [Lab 19](lab-19-javascript-custom-action.md) | JavaScript Custom Action | Avanzado | Node local |
| [Lab 20](lab-20-docker-container-action.md) | Docker Container Action | Avanzado | Docker en runner |
| [Lab 21](lab-21-self-hosted-runners.md) | Self-hosted runner | Avanzado/optativo | Host temporal |
| [Lab 22](lab-22-github-cli-and-api.md) | GitHub CLI y API | Intermedio | No |
| [Lab 23](lab-23-migrating-from-azure-devops.md) | Migración Azure DevOps | Avanzado | Docker/CLI/ADO opcionales |
| [Lab 24](lab-24-rulesets-and-required-workflows.md) | Rulesets y workflows requeridos | Avanzado/optativo | Organización/plan |

!!! tip
    Trabaja en un repositorio de pruebas. Haz cada laboratorio en una rama o elimina su workflow antes de iniciar el siguiente si quieres observar solo un run a la vez.

Continúa con los [ejercicios adicionales](exercises.md), prepara un [repositorio de práctica](practice-repository.md) y cierra el recorrido con el [proyecto final](final-project.md).
