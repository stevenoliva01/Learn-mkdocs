---
title: Lab 17 — Monitoring, logs y debugging
description: Crear evidencia de ejecución, provocar un fallo y diagnosticarlo con seguridad.
tags: [GitHub Actions, Laboratorios, Observability]
---

# Lab 17 — Monitoring, logs y debugging

## Objetivo

Construir un workflow manual que deje annotations, grupos y summary, falle de forma intencional y se inspeccione desde la UI. Lee antes [Monitoring, logs y debugging](../execution/monitoring-logging-and-debugging.md).

## Flujo y prerrequisitos

```text
workflow_dispatch → observe job → logs/groups/summary → failure → rerun → diagnosis
```

Usa un repositorio de pruebas y crea `.github/workflows/lab-17-monitoring.yml`. `observe` es un job_id creado por ti; `workflow_dispatch`, `jobs`, `runs-on`, `steps` y `run` ya se explican en los Labs 01–04.

## Construcción incremental

1. Añade el trigger manual y el job `observe` en `ubuntu-latest`.
2. Añade un step que emita `::notice`, `::warning`, `::group` y `::endgroup`.
3. Añade un step que escriba una conclusión sin secretos en `$GITHUB_STEP_SUMMARY`.
4. Añade al final `exit 1` con el mensaje `intentional diagnostic failure`.

```yaml
name: Lab 17 - Monitoring
on: {workflow_dispatch:}
jobs:
  observe:
    name: Emit useful evidence
    runs-on: ubuntu-latest
    steps:
      - name: Create annotations and grouped logs
        run: |
          echo "::notice title=Lab::Run started"
          echo "::warning::This warning is intentional"
          echo "::group::Safe diagnostics"
          node --version || true
          echo "::endgroup::"
      - name: Write summary
        run: echo "## Lab 17`nEvidence created safely." >> "$GITHUB_STEP_SUMMARY"
      - name: Fail intentionally
        run: exit 1
```

## Ejecutar, observar y corregir

En **Actions**, ejecuta el workflow. Observa el run fallido, el grafo, el job, el step expandido, annotations y summary. Busca `intentional`, descarga los logs si necesitas archivarlos y prueba **Re-run failed jobs**: debe fallar igual porque es determinista. Cambia `exit 1` por `echo "fixed"`, ejecuta de nuevo y observa éxito. Añade opcionalmente el badge descrito en teoría. Para un rerun de diagnóstico puedes habilitar debug, pero no imprimas contexts ni secrets; `ACTIONS_STEP_DEBUG` es configuración temporal, no YAML del job.

## Limpieza y aprendizaje

Elimina el workflow o conserva la versión corregida. Aprendiste a distinguir run, job, step y log, y a usar rerun como prueba de hipótesis, no como sustituto de una corrección. Continúa con [Lab 18](lab-18-workflow-templates.md).
