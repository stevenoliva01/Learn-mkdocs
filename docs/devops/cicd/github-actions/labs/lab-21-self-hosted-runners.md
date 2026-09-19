---
title: Lab 21 — Self-hosted runners
description: Registrar, seleccionar y retirar un runner de pruebas de forma segura.
tags: [GitHub Actions, Laboratorios, Security]
---

# Lab 21 — Self-hosted runners

!!! warning
    **Avanzado y opcional.** Usa solo un host de pruebas temporal, sin secretos, sin datos productivos y sin PRs de forks. Lee [Self-hosted runners](../security/self-hosted-runners.md).

## Objetivo y flujo

```text
GitHub settings → registration token → temporary runner → labeled job → logs → removal
```

Registra el runner siguiendo la UI y las instrucciones oficiales del alcance elegido. Comprueba el host, red y espacio antes de hacerlo; instala el servicio solo si entiendes cómo detenerlo. Añade un label de pruebas como `lab-temporary`, que es una etiqueta elegida por ti, no un permiso.

## Workflow mínimo

```yaml
name: Lab 21 - Temporary runner
on: {workflow_dispatch:}
jobs:
  inspect-runner:                 # job_id elegido por el alumno
    runs-on: [self-hosted, lab-temporary]
    steps:
      - run: echo "Runner $RUNNER_NAME on $RUNNER_OS"
```

Ejecuta manualmente, verifica en la UI qué runner recibió el job y revisa los logs. Si se queda en cola, revisa labels, disponibilidad, runner group y conectividad antes de cambiar YAML. No ejecutes checkout de código no confiable ni agregues secretos para “probar acceso”.

## Cleanup obligatorio

Detén el servicio, elimina el registro desde el host y confirma en GitHub que el runner desapareció. Borra el workflow si era exclusivo de la práctica. Contrasta el host persistente usado aquí con un runner `--ephemeral`, JIT o ARC: esos patrones reducen estado residual y son preferibles para autoscaling, pero no se configuran en este lab. Sigue con [Lab 22](lab-22-github-cli-and-api.md).
