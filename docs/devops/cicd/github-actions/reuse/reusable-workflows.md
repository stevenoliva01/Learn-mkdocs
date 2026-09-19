---
title: Reusable Workflows
description: Reutilización de jobs completos con workflow_call, contratos y versionado.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Reusable Workflows

Un reusable workflow comparte uno o varios jobs completos. Es el mecanismo correcto cuando varios repositorios deben aplicar el mismo estándar de CI, calidad o despliegue sin copiar la orquestación.

## Modelo y contrato

```text
Caller workflow → workflow_call → called workflow → jobs
```

El workflow llamado vive normalmente en `.github/workflows/java-ci.yml`, declara `on.workflow_call` y define inputs, secretos y outputs explícitos. No es una Action llamada desde un step: el caller lo usa como un job.

```yaml
# .github/workflows/java-ci.yml
name: Java CI reutilizable
on:
  workflow_call:
    inputs:
      java-version:
        required: true
        type: string
    secrets:
      sonar-token:
        required: false
    outputs:
      version:
        value: ${{ jobs.verify.outputs.version }}
jobs:
  verify:
    runs-on: ubuntu-latest
    outputs:
      version: ${{ steps.version.outputs.value }}
    steps:
      - uses: actions/checkout@v7
      - uses: actions/setup-java@v6
        with: {distribution: temurin, java-version: ${{ inputs.java-version }}, cache: maven}
      - id: version
        run: echo "value=$(./mvnw -q help:evaluate -Dexpression=project.version -DforceStdout)" >> "$GITHUB_OUTPUT"
      - run: ./mvnw -B verify
```

El caller declara el job y consume el resultado mediante `needs`:

```yaml
jobs:
  java-ci:
    uses: platform-engineering/workflows/.github/workflows/java-ci.yml@<sha>
    with:
      java-version: "21"
    secrets: inherit
  publish:
    needs: java-ci
    runs-on: ubuntu-latest
    steps:
      - run: echo "Versión: ${{ needs.java-ci.outputs.version }}"
```

`secrets: inherit` puede simplificar un caller del mismo ámbito, pero también amplía qué secretos llegan al workflow llamado. Prefiere declarar secretos concretos cuando el contrato lo permita.

`inputs` y `outputs` forman el contrato tipado del reusable workflow: el llamado declara los inputs permitidos en `on.workflow_call.inputs`, y el caller los entrega mediante `jobs.<job_id>.with`. Los `outputs` del workflow apuntan a outputs de uno de sus jobs y el caller los lee con `needs.<job_id>.outputs`. `secrets` declara secretos permitidos por el workflow llamado; `secrets: inherit` reenvía todos los secretos a los que el caller tiene acceso y solo debe usarse tras revisar ese alcance. `with` de un job que llama un workflow no es el mismo mapa que `steps[*].with`: dentro del called workflow sus valores se leen como `inputs.*`.

## Versionado y seguridad

Se puede llamar dentro del mismo repositorio o desde otro mediante owner/repo, ruta y ref. Un branch ofrece cambios rápidos; un tag versionado da un contrato legible; un SHA completo fija exactamente el contenido y es preferible para una dependencia externa con requisitos estrictos de supply chain. Actualizar el reusable workflow central puede afectar muchos consumidores: documenta cambios compatibles e incompatibles.

```text
Repositorio de plataforma
└── .github/workflows/java-ci.yml
        ↑        ↑        ↑
      API A    API B    API C
```

Este patrón permite a Platform Engineering mantener los controles comunes, mientras los equipos de producto conservan triggers, configuración y responsabilidad sobre su repositorio. No lo uses para una secuencia de dos steps que solo vive en un workflow: una Composite Action o un script será más simple.

## Relación con otros conceptos

`workflow_call` conserva el contexto del caller; no es equivalente a `workflow_run`. Los secrets, permisos y environments deben diseñarse por contrato, y los outputs se propagan como outputs del job llamado. Para operaciones pequeñas ve a [Composite Actions](composite-actions.md).
