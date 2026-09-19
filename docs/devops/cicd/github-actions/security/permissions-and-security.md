---
title: Permisos y seguridad
description: Mínimo privilegio, secretos, OIDC y tratamiento de código no confiable.
tags: [GitHub Actions, CI/CD, DevOps, Security]
---

# Permisos y seguridad

Un workflow es código que recibe eventos y ejecuta instrucciones con identidades. La seguridad consiste en decidir qué identidad, datos y privilegios necesita cada job, no en añadir secretos hasta que funcione.

## `GITHUB_TOKEN` y mínimo privilegio

GitHub genera un `GITHUB_TOKEN` temporal para el run. Declara permisos explícitos al nivel más pequeño posible: workflow como línea base y job cuando una tarea necesita más.

```yaml
permissions:
  contents: read
jobs:
  publish-image:
    permissions:
      contents: read
      packages: write
  azure-deploy:
    permissions:
      contents: read
      id-token: write
```

`contents: read` permite obtener el código; `packages: write` permite publicar en GHCR; `id-token: write` permite solicitar un token OIDC. Ninguno equivale a acceso ilimitado a GitHub o Azure. Evita `write-all` y revisa qué operación concreta requiere cada permiso.

## Secrets y OIDC

Los secrets se configuran en repository, environment u organization. Elige el alcance mínimo y expón cada secreto solo al step que lo necesita. Un environment permite asociar secretos a una promoción protegida. Para proveedores compatibles, OIDC evita guardar un client secret de larga duración; la guía de [Azure OIDC](../pipelines/azure-oidc-and-aks.md) muestra que el proveedor todavía debe validar claims y RBAC.

No imprimas secrets ni contexts completos; un secreto puede no estar disponible en runs de forks. Ese comportamiento es una protección, no un error que deba sortearse.

## PRs, forks e inputs no confiables

`pull_request` está diseñado para validar cambios sin conceder secretos a código de forks. `pull_request_target` se ejecuta en el contexto de la rama base y puede acceder a privilegios: es útil para automatización cuidadosamente diseñada sobre metadatos del PR, pero peligroso si checkout o ejecuta código aportado por el fork.

!!! danger
    No combines `pull_request_target`, checkout de código no confiable y secretos o permisos de escritura. Tampoco interpoles títulos, branches, inputs o payloads directamente en `run`.

Pasa datos por `env`, entrecomilla en el shell, valida opciones permitidas y mantén operaciones de despliegue fuera de eventos no confiables.

## Actions de terceros y supply chain

`uses: owner/action@tag` es legible pero el tag puede cambiar; `uses: owner/action@<SHA completo>` fija el contenido exacto. Para una dependencia sensible, fija SHA y anota la versión revisada. Evalúa procedencia, mantenimiento, permisos y Dependabot para Actions. Las Actions oficiales no eliminan la necesidad de mínimo privilegio.

## Fronteras de confianza y gobierno

Modela explícitamente código confiable/no confiable, token y secrets, runner y datos de evento. Un PR de un fork puede controlar archivos, títulos y mensajes; `pull_request` sirve para validarlo con privilegios restringidos. `pull_request_target` usa el contexto de la rama base: úsalo únicamente para automatización de metadatos que no haga checkout ni ejecute código del fork. Hacer que el ejemplo funcione con un token de escritura no es una corrección de seguridad.

CODEOWNERS dirige revisión de rutas sensibles; protected branches/tags, rulesets y required status checks ayudan a bloquear integración sin señales acordadas. Cuando la disponibilidad del producto lo permita, un ruleset puede requerir workflows antes del merge. Documenta esa regla por su comportamiento —workflow/check requerido, alcance y excepción— y no como la terminología histórica “Required Workflows”. Dependency, code y secret scanning aportan señales adicionales, pero no reemplazan revisión, pinning ni protección de ramas.
