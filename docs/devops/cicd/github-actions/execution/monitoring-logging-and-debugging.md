---
title: Monitoring, logs y debugging
description: Observar, interpretar y diagnosticar workflow runs sin exponer datos sensibles.
tags: [GitHub Actions, CI/CD, DevOps, Observability]
---

# Monitoring, logs y debugging

Un pipeline no termina cuando el YAML es aceptado: debe dejar evidencia suficiente para saber qué ocurrió, dónde ocurrió y qué decisión tomar. Monitoring convierte una ejecución opaca en una secuencia investigable; debugging añade detalle de forma temporal cuando los logs normales no bastan.

```text
Workflow → Workflow run → Job → Step → Log
```

Un **workflow** es la definición. Cada evento crea un **workflow run**; GitHub crea un check suite y cada job se refleja como un check run. Dentro de un job, los steps dejan sus logs secuenciales. No confundas el nombre visible con los IDs que usa la API: la UI muestra ambos en distintos lugares.

## Leer la UI antes de cambiar YAML

En **Actions**, el historial permite seleccionar el workflow y después un run. El resumen muestra estado, conclusión, duración y el grafo de visualización. El grafo revela dependencias `needs`, jobs paralelos y el punto exacto donde se detuvo el flujo. Abre un job para ver los steps: GitHub también registra *Set up job* y *Complete job*. En runners hospedados, el primero enlaza la imagen y sus herramientas preinstaladas.

| Resultado del run | Lectura operativa |
| --- | --- |
| `success` | El trabajo configurado terminó correctamente. No prueba por sí solo que una entrega sea segura. |
| `failure` | Un step, job o configuración falló; comienza por el primer error accionable. |
| `cancelled` | Una persona, concurrencia o plataforma canceló el run. |
| `skipped` | Una condición o dependencia impidió ejecutar un job/step. |
| `neutral` | El check terminó sin una conclusión de éxito o fallo bloqueante cuando el contexto lo soporta. |
| `timed_out` | El límite de tiempo terminó la ejecución. |

Estos valores no son intercambiables: la disponibilidad exacta depende de si se consulta una conclusión de run, check o job. Documenta la señal que utiliza tu ruleset o required status check.

## Logs, búsqueda, descarga y anotaciones

Un step fallido se abre automáticamente. Busca en el log el primer `error`, el comando ejecutado y el exit code; el último mensaje suele ser una consecuencia. La UI permite buscar dentro de los logs, enlazar una línea y descargar el archivo del run para una investigación reproducible. Con GitHub CLI se puede inspeccionar un run y sus logs, por ejemplo `gh run view RUN_ID --log-failed`; no pegues logs que contengan configuración sensible en issues públicos.

Las workflow commands añaden señales estructuradas sin ocultar el fallo:

```bash
echo "::notice title=Build::Se usará la versión calculada"
echo "::warning file=src/app.js,line=12::Configuración obsoleta"
echo "::error file=src/app.js,line=12::Validación fallida"
echo "::group::Diagnóstico de dependencias"
npm ls --depth=0
echo "::endgroup::"
```

`notice`, `warning` y `error` producen anotaciones visibles; `group` y `endgroup` pliegan ruido relacionado. Una anotación comunica contexto, pero `::error` no reemplaza `exit 1` cuando el control debe fallar. Para una conclusión legible, escribe Markdown al archivo que GitHub proporciona:

```bash
{
  echo "## Resultado de validación"
  echo "- Commit: \`$GITHUB_SHA\`"
  echo "- Estado: correcto"
} >> "$GITHUB_STEP_SUMMARY"
```

`GITHUB_STEP_SUMMARY` reúne por job una síntesis de la ejecución; no es un artifact ni una variable global. Nunca vuelques contexts completos, tokens, headers, archivos `.env` o respuestas de APIs solo para depurar.

## Reintentar con intención

Reintenta un workflow, sus jobs fallidos o un job específico cuando la causa parezca transitoria y ya tengas una hipótesis: caída de servicio, saturación del runner o descarga interrumpida. La UI ofrece **Re-run all jobs**, **Re-run failed jobs** y reejecución por job. La CLI expone `gh run rerun RUN_ID`, `gh run rerun RUN_ID --failed` y `gh run rerun --job JOB_ID`; `--debug` habilita depuración para ese reintento. No uses rerun para maquillar una prueba determinísticamente rota: corrige el código o la configuración y genera evidencia nueva.

Un badge comunica el estado de un workflow en el README, por ejemplo:

```markdown
[![CI](https://github.com/OWNER/REPOSITORY/actions/workflows/ci.yml/badge.svg)](https://github.com/OWNER/REPOSITORY/actions/workflows/ci.yml)
```

Sustituye `OWNER`, `REPOSITORY` y `ci.yml`; un badge informa un estado, no reemplaza el historial ni las protecciones de merge.

## Debug logging: más señal, más cuidado

Establece el secreto o variable `ACTIONS_STEP_DEBUG` en `true` para ampliar los eventos de los steps. `ACTIONS_RUNNER_DEBUG=true` agrega al archivo descargable logs de diagnóstico del proceso runner y worker, en `runner-diagnostic-logs`. Si ambos existen como secreto y variable, el secreto tiene prioridad. `runner.debug` permite condicionar una comprobación adicional:

```yaml
- name: Show safe diagnostics only when debug is enabled
  if: ${{ runner.debug == '1' }}
  run: node --version
```

Habilita estos controles por tiempo acotado o para un reintento; su mayor verbosidad puede revelar rutas, versiones y detalles operativos. La secuencia sana es: reproducir → localizar job/step → leer el primer error → habilitar debug si falta evidencia → corregir → volver a ejecutar → retirar diagnóstico temporal.

## Relación con costos y seguridad

La duración del job y sus minutos facturables ayudan a detectar matrices excesivas, esperas y reruns repetidos. No deduzcas precios desde un run: consulta los límites y precios vigentes. Para reducir investigación y costo, usa `timeout-minutes`, `concurrency`, filtros y logs con mensajes accionables. Practica el flujo completo en [Lab 17 — Monitoring, logs y debugging](../labs/lab-17-monitoring-logging-and-debugging.md).
