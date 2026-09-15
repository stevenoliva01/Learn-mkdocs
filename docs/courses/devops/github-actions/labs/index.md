---
title: Prácticas guiadas de GitHub Actions
description: 16 laboratorios reproducibles para ejecutar, observar y depurar GitHub Actions.
tags: [GitHub Actions, CI/CD, DevOps, Laboratorios]
---

# Prácticas guiadas de GitHub Actions

Estos 16 laboratorios se ejecutan en un repositorio GitHub que crea el alumno. Este sitio no contiene workflows activos: cada página indica los archivos completos que debes copiar y el resultado que debes observar en Actions.

## Antes de empezar

Necesitas una cuenta GitHub, Git instalado y un repositorio vacío propio. Crea una carpeta local, inicialízala, añade un `README.md` y publica la rama `main`. Los Labs 12 y 14 requieren activar características externas; los Labs 14 y 15 son avanzados. Nunca pegues secretos reales en YAML.

## Ruta recomendada

```text
Básico:      01 → 02 → 03 → 04
Intermedio:  05 → 06 → 07 → 08 → 09 → 10
CI/CD:       11 → 12 → 13
Cloud:       14 → 15
Integración: 16
```

| Lab | Tema | Nivel | Servicio externo |
| --- | --- | --- | --- |
| 01–11 | Workflows, eventos, jobs, datos y reutilización | Básico/Intermedio | No |
| 12 | Docker y GHCR | Intermedio | GitHub Packages |
| 13 | Quality gate local | Intermedio | No |
| 14 | Azure OIDC | Avanzado | Azure |
| 15 | Terraform | Avanzado | Terraform CLI; cloud opcional |
| 16 | Pipeline completo | Avanzado | No; GHCR opcional |

!!! tip
    Trabaja en un repositorio de pruebas. Haz cada laboratorio en una rama o elimina su workflow antes de iniciar el siguiente si quieres observar solo un run a la vez.

El [inventario de 35 ejercicios](../labs.md) permanece como ampliación de esta ruta, y el [repositorio de práctica](../practice-repository.md) deja preparada la futura extracción a `github-actions-labs`.
