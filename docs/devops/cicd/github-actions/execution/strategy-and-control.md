---
title: Estrategia y control de ejecución
description: Matrices, concurrencia, límites y tolerancia explícita a fallos.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Estrategia y control de ejecución

Estas opciones controlan cuántas veces se ejecuta un job, qué run puede seguir activo y cómo se interpreta un fallo. No son adornos: protegen tiempo de feedback, ambientes compartidos y señales de calidad.

## Matrix

Una matrix resuelve la validación de una misma lógica contra combinaciones relevantes, por ejemplo Java 17/21 y Ubuntu/Windows. No la uses por completitud teórica si cada combinación no aporta una señal útil.

```yaml
strategy:
  fail-fast: false
  max-parallel: 2
  matrix:
    os: [ubuntu-latest, windows-latest]
    java: ["17", "21"]
    exclude: [{os: windows-latest, java: "17"}]
    include: [{os: ubuntu-latest, java: "25", experimental: true}]
```

`include` añade casos fuera del producto cartesiano; `exclude` retira combinaciones inválidas. `fail-fast: false` permite observar los demás resultados después de un fallo; `max-parallel` protege capacidad limitada. Accede con `${{ matrix.java }}`. Una matrix grande multiplica costo y ruido.

En otras palabras, `matrix` es la lista de dimensiones; cada combinación genera una copia del mismo job. `include` puede añadir campos como `label` a una combinación y `exclude` quita una coincidencia. `fail-fast` controla si un fallo cancela las copias pendientes o en curso (por defecto es `true`); `max-parallel` limita cuántas copias ejecuta GitHub a la vez, sin cambiar las combinaciones.

## Concurrencia

La concurrencia evita dos CI obsoletas o dos despliegues hacia PRE al mismo tiempo.

```yaml
concurrency:
  group: deploy-pre-${{ github.repository }}
  cancel-in-progress: false
```

Para CI de una misma rama, `cancel-in-progress: true` normalmente da feedback más reciente. Para Terraform o producción, cancelar un apply en curso puede ser inseguro: serializa y diseña recuperación. El grupo debe representar el recurso compartido, no ser una cadena constante para toda la organización.

`concurrency.group` es esa clave de exclusión mutua: dos runs con la misma clave compiten entre sí, mientras claves distintas pueden avanzar a la vez. `cancel-in-progress: true` cancela el run o job anterior que siga en curso para esa clave; con `false`, GitHub mantiene el anterior y deja el nuevo pendiente. Elige el scope (workflow o job) según el recurso que proteges.

## `continue-on-error` y límites

`continue-on-error: true` conserva el error, pero permite continuar; es útil para una combinación experimental o una observación no bloqueante. Nunca lo uses para silenciar test, SAST, quality gate o deploy crítico. `timeout-minutes` limita un job que quedó bloqueado; los reintentos deben ser explícitos y reservados para fallos transitorios comprobables.
