---
title: Lab 20 — Docker Container Action
description: Empaquetar un step en Docker y diferenciarlo de job y service containers.
tags: [GitHub Actions, Laboratorios, Docker]
---

# Lab 20 — Docker Container Action

## Objetivo

Crear una Docker Container Action mínima con input y salida exitosa/fallida. Antes, lee [Custom Actions](../reuse/custom-actions.md) y [Contenedores y servicios](../execution/containers-and-services.md).

## Construcción incremental

Crea `.github/actions/greeting/action.yml`, `Dockerfile` y `entrypoint.sh` con el ejemplo de teoría. Haz ejecutable el script en el contexto de tu repositorio. Añade un workflow manual con checkout y un step `uses: ./.github/actions/greeting` con `with.name: Ada`. El step utiliza una Action Docker; no es `container:` de job ni `services:` para una base de datos auxiliar.

Ejecuta, abre logs y confirma `Hello Ada`. Después cambia el script para validar argumento y terminar con `exit 2` si está vacío; invócalo sin input o con una variante que active el fallo, observa la conclusión y restaura la ruta feliz. La limpieza consiste en eliminar workflow e imágenes/caches locales que hayas creado al probar. Continúa con [Lab 21](lab-21-self-hosted-runners.md).
