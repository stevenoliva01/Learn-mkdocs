---
title: Diagnóstico, costos y anti-patrones
description: Guía práctica para investigar fallos y operar workflows eficientemente.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Diagnóstico, costos y anti-patrones

Empieza por el job y step fallido, no por reintentar a ciegas. Los logs, la ref, el evento y el directorio de trabajo suelen revelar qué supuesto fue incorrecto.

| Síntoma | Comprobación y relación |
| --- | --- |
| Workflow no aparece | Ruta `.github/workflows`, YAML, rama predeterminada y `workflow_dispatch` |
| No se ejecuta | `on`, filtros de branch/path y `types` del evento |
| Código no encontrado | Falta `actions/checkout` |
| Archivo desapareció | Cambió de job/runner; usa artifact |
| Permission denied | Revisa `permissions` y la identidad del proveedor |
| Secret vacío | Scope, environment y ejecución desde fork |
| Shell/ruta incorrectos | SO, `shell` y `working-directory` |
| Deploy duplicado | Define `concurrency` por ambiente |
| Matrix excesiva | Cuenta combinaciones, `include` y `exclude` |

`ACTIONS_STEP_DEBUG` puede ampliar logs cuando está habilitado, pero no uses depuración para imprimir secretos o contextos completos. Para errores transitorios, identifica la dependencia y aplica reintento acotado; no escondas fallos deterministas.

## Costos y anti-patrones

Usa filtros de paths, caches, timeouts, concurrencia y retención razonable de artifacts. Verifica límites y cuotas vigentes en la documentación oficial antes de afirmar números. Evita `continue-on-error` para gates, secretos en YAML, dependencias solo por `latest`, operaciones privilegiadas sobre forks y duplicar pipelines que deberían compartir un contrato reutilizable.

El consumo depende de tipo de runner, duración, matrices, reintentos, artifacts, retención, caches, triggers excesivos y de la infraestructura de self-hosted runners. Controla el crecimiento con `paths`, `concurrency`, cache con claves correctas, `timeout-minutes`, `max-parallel`, matrices pequeñas y retención ajustada. Una optimización no debe retirar validación necesaria ni convertir un deploy compartido en carreras paralelas.
