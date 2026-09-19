---
title: Lab 07 - Matrix
description: Ejecutar el mismo job con varias versiones de Node.
tags: [GitHub Actions, Laboratorios]
---

# Lab 07 — Matrix

## Objetivo

Ver cómo una matrix crea varios jobs visibles para validar compatibilidad útil, sin multiplicar combinaciones sin razón. Lee [Estrategia y control](../execution/strategy-and-control.md).

`node` es el **job_id**; `matrix.node` es una variable creada por la estrategia, no una versión fija ni una keyword global. `include` añade datos a una combinación, `exclude` la elimina, `fail-fast` decide qué ocurre con otras copias después de un fallo y `max-parallel` limita cuántas copias se ejecutan a la vez. `label` es un campo elegido por este lab.

## Archivo a crear

```yaml
name: Lab 07 - Matrix
on: {workflow_dispatch: {}}
jobs:
  node:
    name: Test Node ${{ matrix.node }}
    runs-on: ubuntu-latest
    strategy:
      fail-fast: false
      max-parallel: 1
      matrix:
        node: [22, 24]
        include:
          - node: 24
            label: current
    steps:
      - uses: actions/setup-node@v7
        with: {node-version: ${{ matrix.node }}}
      - run: node --version
      - run: echo "label=${{ matrix.label || 'supported' }}"
```

## Ejecutar, validar y experimentar

Ejecuta el workflow y observa dos jobs, uno por versión, en secuencia por `max-parallel: 1`. Añade `exclude: [{node: 22}]` y observa que solo queda el job 24. No uses `include` para repetir accidentalmente una combinación: revisa el número de jobs antes de ejecutar. No crea recursos persistentes. Sigue con [Lab 08](lab-08-concurrency.md).
