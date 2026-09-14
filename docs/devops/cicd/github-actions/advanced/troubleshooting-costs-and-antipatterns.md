---
title: Diagnóstico, costos y anti-patrones
description: Depuración segura y operación eficiente de workflows.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Diagnóstico, costos y anti-patrones

Revisa logs por job y step, el directorio de trabajo, `PATH`, permisos y secretos vacíos. `ACTIONS_STEP_DEBUG` aporta detalle adicional; evita imprimir secretos o contexts completos sin necesidad.

Errores frecuentes incluyen YAML inválido, no hacer checkout antes de usar el código, permisos insuficientes, shell incorrecto y rutas equivocadas. Reintenta un job cuando el fallo sea transitorio y usa `GITHUB_STEP_SUMMARY` para dejar resultados visibles.

## Costos y eficiencia

Usa filtros de paths, caché, concurrencia, `timeout-minutes` y retención razonable de artifacts. Elimina logs o artifacts innecesarios. Consulta la documentación oficial de GitHub para los límites y cuotas actuales.

## Anti-patrones

- `continue-on-error` para ocultar controles críticos.
- Dependencia exclusiva de `latest` para imágenes.
- Secretos en YAML o impresos en logs.
- Ejecutar código de forks con privilegios elevados.
- Duplicar workflows en vez de reutilizar componentes.
