---
title: Lab 06 - Cache
description: Observar un cache miss y cache hit con dependencias npm.
tags: [GitHub Actions, Laboratorios]
---

# Lab 06 — Cache

## Objetivo y prerrequisitos

Observar que la cache acelera dependencias, pero no transporta entregables. Necesitas Node y npm localmente para crear `package-lock.json`; consulta la [teoría](../execution/artifacts-cache-and-environments.md).

`npm-cache` es un **job_id** elegido por nosotros. `actions/checkout` descarga el código en el runner; `actions/setup-node` instala Node y, con `with.cache: npm`, restaura/guarda la cache global de npm usando el lockfile. `node-version` y `cache` son inputs de esa Action; `npm ci` sigue instalando de forma reproducible y no convierte la cache en un artifact.

## Archivos a crear

En la raíz del repositorio de pruebas ejecuta:

```bash
npm init -y
npm install --save-dev eslint
```

Crea `.github/workflows/lab-06.yml`:

```yaml
name: Lab 06 - Cache
on: {workflow_dispatch: {}}
jobs:
  npm-cache:
    name: Install dependencies with npm cache
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - uses: actions/setup-node@v7
        with: {node-version: "24", cache: npm}
      - run: npm ci
      - run: npm --version
```

## Ejecutar y validar

Publica `package.json`, `package-lock.json` y el workflow. La primera ejecución suele indicar cache miss y llena la cache; ejecuta otra vez el mismo workflow y busca cache hit o restauración en logs. El resultado final sigue siendo `npm ci` correcto, no un archivo descargable en Artifacts.

## Reto, troubleshooting y limpieza

Modifica una dependencia y regenera el lockfile: cambia la key automática y espera otro miss. Si falla, confirma que `package-lock.json` fue publicado y que `npm ci` puede resolverlo localmente. Elimina el workflow y, opcionalmente, la cache desde Actions → Caches; elimina `node_modules` local, nunca lo subas a Git. Siguiente: [Lab 07](lab-07-matrix.md).
