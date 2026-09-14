---
title: Variables, contextos y expresiones
description: Datos de configuración y lógica declarativa en workflows.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Variables, contextos y expresiones

`env` define variables a nivel de workflow, job o step. `vars` contiene configuración de organización, repositorio o environment. `secrets` expone datos sensibles configurados en GitHub; su protección se amplía en [seguridad](../security/permissions-and-security.md).

```yaml
env:
  APP_NAME: payments-api

jobs:
  build:
    env:
      JAVA_VERSION: "21"
    steps:
      - run: echo "${{ vars.APP_NAME }}"
```

## Contextos

Las expresiones usan `${{ ... }}`. Los contextos frecuentes son `github`, `env`, `vars`, `job`, `steps`, `runner`, `strategy`, `matrix`, `needs` e `inputs`.

```yaml
${{ github.repository }}
${{ github.sha }}
${{ runner.os }}
${{ matrix.java }}
${{ needs.build.outputs.version }}
```

## Condiciones y funciones

```yaml
if: ${{ github.ref_name == 'main' && github.event_name == 'push' }}
if: ${{ contains(github.event.head_commit.message, '[deploy]') }}
```

También están disponibles `startsWith`, `endsWith`, `format` y `toJSON`, además de las funciones de estado `success`, `failure`, `cancelled` y `always`.

!!! warning
    `toJSON` ayuda a diagnosticar contexts, pero no imprimas indiscriminadamente datos que puedan ser sensibles. Para comunicar resultados entre steps o jobs, usa [outputs y archivos especiales](../execution/outputs-and-workflow-commands.md).
