---
title: Lab 19 — JavaScript Custom Action
description: Empaquetar input y output mediante @actions/core sin publicar una Action.
tags: [GitHub Actions, Laboratorios, JavaScript]
---

# Lab 19 — JavaScript Custom Action

## Objetivo y estructura

Crear y usar una Action local con un input, output y fallo controlado. Lee [Custom Actions](../reuse/custom-actions.md). Crea en tu repositorio de prueba:

```text
.github/actions/validate-version/
├── action.yml
├── package.json
├── src/index.js
└── dist/index.js
```

`dist/` representa el resultado empaquetado según la estrategia actual de la Action; no incluyas `node_modules`. Usa el `action.yml` y `src/index.js` de la guía, instala `@actions/core`, compila/empaqueta con la herramienta elegida y versiona solo el resultado necesario para que el runner ejecute la Action.

## Invocación y observación

Construye `.github/workflows/lab-19.yml` paso a paso: trigger manual, checkout, step `uses: ./.github/actions/validate-version`, `with.version: "1.2.3"` y un step que lea `steps.validate.outputs.normalized-version`. `validate` es un `id` elegido por ti; `normalized-version` es output definido por la Action. Ejecuta y confirma el output en log.

Después pasa `not-a-version`. La Action debe usar `core.setFailed`, devolver un resultado no exitoso y detener el job. Corrige el input y vuelve a ejecutar. No publiques en Marketplace: el aprendizaje es contrato, empaquetado, exit code y consumo local. Continúa con [Lab 20](lab-20-docker-container-action.md).
