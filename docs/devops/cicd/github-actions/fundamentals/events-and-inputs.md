---
title: Eventos e inputs
description: Triggers, filtros, ejecución manual e integración externa de workflows.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Eventos e inputs

`on` responde a la pregunta «¿por qué se creó este workflow run?». Elegir el evento correcto evita validar demasiado tarde, desplegar por accidente o usar un mecanismo de encadenamiento donde basta una dependencia entre jobs.

| Evento | Cuándo ocurre | Uso típico |
| --- | --- | --- |
| `push` | Se envía un commit o tag | CI tras merge, release por tag |
| `pull_request` | Hay actividad en un PR | Validar antes del merge |
| `workflow_dispatch` | Una persona o API lo solicita | Deploy, rollback, operación controlada |
| `schedule` | Coincide una expresión cron | Mantenimiento periódico |
| `release` | Cambia una GitHub Release | Publicar paquete o imagen |
| `repository_dispatch` | Un sistema externo llama la API | Integración entre sistemas |
| `workflow_run` | Otro workflow inicia o termina | Separar flujos con una frontera real |
| `workflow_call` | Otro workflow lo invoca | Reutilizar jobs |

## `push`: validar el cambio integrado

`push` se dispara cuando llegan commits o tags al repositorio. Es adecuado para CI en una rama integrada; no sustituye la validación previa de un pull request. `github.ref` identifica la ref completa, `github.ref_name` su nombre y `github.sha` el commit del run.

```yaml
on:
  push:
    branches: [main, "feature/**"]
    branches-ignore: ["legacy/**"] # no combinar con branches en el mismo trigger
    tags: ["v*"]
    paths: ["backend/**", ".github/workflows/backend.yml"]
```

En un trigger real usa inclusiones **o** exclusiones equivalentes por tipo de filtro; por ejemplo, no `branches` y `branches-ignore` juntos. Los filtros por `paths` ahorran tiempo en monorepos, pero no sustituyen una estrategia de pruebas que detecte cambios compartidos. No uses `push` para pedir confirmación humana: usa un environment o `workflow_dispatch`.

## `pull_request`: decidir antes de integrar

Un PR separa base y head: `github.base_ref` es la rama destino, `github.head_ref` la rama que propone cambios. `github.ref` y `github.sha` representan la referencia que GitHub prepara para el run, por lo que no deben confundirse con una rama de despliegue.

```text
Developer → Pull request → compile + tests + scan → required check → merge
```

```yaml
on:
  pull_request:
    branches: [main]
    types: [opened, synchronize, reopened]
```

Úsalo para checks que deben informar la decisión de merge. `synchronize` cubre commits nuevos en el PR. No des a este evento secretos ni privilegios de producción cuando el código pueda venir de un fork; el tratamiento de `pull_request_target` está en [Seguridad](../security/permissions-and-security.md).

## `workflow_dispatch`: operación manual parametrizada

Resuelve casos que no deben ocurrir por cada commit: elegir un ambiente, hacer rollback, repetir un proceso o lanzar una tarea administrativa. En la interfaz muestra **Run workflow** y permite elegir la ref; también se invoca mediante GitHub CLI o API. El archivo debe existir en la rama predeterminada para que el trigger reciba el evento.

```yaml
name: Deploy manual
run-name: Deploy ${{ inputs.environment }} por @${{ github.actor }}
on:
  workflow_dispatch:
    inputs:
      environment:
        description: Ambiente de destino
        type: choice
        required: true
        options: [dev, pre, prod]
      dry_run:
        description: Solo simular
        type: boolean
        default: true
      release_version:
        description: Versión que se desplegará
        type: string
        required: true
```

Los tipos `string`, `boolean`, `choice` y `environment` expresan intención y `inputs` conserva booleanos como booleanos; `github.event.inputs` los representa como strings. Un input no es autorización: `environment: production`, permisos, validación de valores y controles de rama deben proteger una operación sensible.

```text
Actions → Run workflow → environment=pre, dry_run=false → workflow run
```

No uses una entrada no validada directamente dentro de un comando de shell. Usa `workflow_dispatch` cuando una persona inicia la operación; para una integración externa usa `repository_dispatch`.

## `schedule`: mantenimiento periódico

`schedule` declara cron en UTC. Sirve para verificar dependencias, renovar datos no sensibles o ejecutar una comprobación diaria; no es una garantía de ejecución exacta y puede retrasarse durante alta carga.

```yaml
on:
  schedule:
    - cron: "0 5 * * 1-5" # minuto, hora, día del mes, mes, día de semana
```

El ejemplo corre a las 05:00 UTC de lunes a viernes. Evita programar una operación destructiva sin idempotencia y locks. Azure DevOps también tiene triggers programados, pero la zona horaria, filtros y reglas se deben comprobar en cada plataforma; no copies el cron sin revisarlo.

## `release`: reaccionar a una release publicada

Un tag es una referencia Git; una GitHub Release es una entidad de publicación asociable a un tag. `release` con `published` permite publicar un paquete o una imagen cuando la release queda visible.

```yaml
on:
  release:
    types: [published]
```

Úsalo cuando la publicación formal sea el contrato. Si el contrato es simplemente «se creó un tag `v*`», `push.tags` es más directo. No confundas crear un borrador de release con publicar una versión.

## `repository_dispatch`: una llamada desde fuera

```text
Sistema externo → GitHub API → repository_dispatch → workflow
```

```yaml
on:
  repository_dispatch:
    types: [upstream-release]
```

El emisor manda `event_type: upstream-release` y puede incluir `client_payload`, disponible como `github.event.client_payload`. Úsalo para integrar sistemas externos o repositorios que no pueden llamar un reusable workflow. A diferencia de `workflow_dispatch`, la intención proviene de un cliente autenticado y no de un botón; valida el payload y limita el token del emisor.

## `workflow_run` y `workflow_call`: encadenar o reutilizar

`workflow_run` observa el inicio o final de otro workflow. Puede separar CI y CD cuando esa frontera simplifica permisos o responsabilidades:

```yaml
on:
  workflow_run:
    workflows: ["CI"]
    types: [completed]
```

No lo uses para ordenar jobs del mismo proceso: `needs` es más claro y conserva un único run. Revisa con cuidado permisos y artifacts al elevar privilegios entre workflows.

`workflow_call` declara que un workflow puede ser invocado por otro. No es un trigger de usuario ni un encadenamiento asíncrono: es un contrato de reutilización que conserva el evento del caller. Su explicación y ejemplos completos están en [Reusable Workflows](../reuse/reusable-workflows.md).

## Relación con Azure DevOps

`push`, `pull_request`, `schedule` y ejecución manual tienen equivalentes funcionales en triggers de Azure Pipelines. El mapeo no es uno a uno para `workflow_run`, `workflow_call` o templates; consulta la [comparación razonada](../advanced/azure-devops-comparison.md).

## Práctica recomendada

Reproduce los contexts y diferencias observables entre ejecución manual, push y pull request en el [Lab 02 — Eventos](../labs/lab-02-events.md).
