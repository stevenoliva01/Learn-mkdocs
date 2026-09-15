---
title: Composite Actions
description: Reutilización de secuencias de steps mediante action.yml.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Composite Actions

Una Composite Action empaqueta steps repetidos como una Action que se invoca desde un step. Resuelve el problema de repetir preparación y comandos coherentes en muchos workflows sin imponer un runner, un job ni un trigger.

## Estructura y ejemplo

```text
.github/actions/setup-java-build/action.yml
```

```yaml
name: Setup Java and build
description: Prepara Java y ejecuta la verificación Maven
inputs:
  java-version:
    description: Versión de Java
    required: true
outputs:
  artifact-name:
    description: Nombre lógico del resultado
    value: ${{ steps.metadata.outputs.artifact-name }}
runs:
  using: composite
  steps:
    - uses: actions/setup-java@v6
      with:
        distribution: temurin
        java-version: ${{ inputs.java-version }}
        cache: maven
    - id: metadata
      shell: bash
      run: |
        ./mvnw -B verify
        echo "artifact-name=payments-api" >> "$GITHUB_OUTPUT"
```

El workflow sigue siendo responsable de checkout, runner y permisos:

```yaml
steps:
  - uses: actions/checkout@v7
  - id: build
    uses: ./.github/actions/setup-java-build
    with:
      java-version: "21"
  - run: echo "${{ steps.build.outputs.artifact-name }}"
```

## Cuándo elegirla

Elígela para una operación de steps que necesita ejecutarse dentro del job del caller: instalar herramientas, aplicar convenciones de build o preparar credenciales ya autorizadas. Elige un [Reusable Workflow](reusable-workflows.md) cuando debe imponer jobs, `needs`, permisos o una política de CI completa. Una Action JavaScript/Docker encaja cuando la lógica necesita empaquetarse o no es razonable como shell.

No escondas una política compleja y crítica dentro de una composite action sin documentar inputs, outputs, shells y efectos. Declara `shell` en cada step `run`: es obligatorio para scripts de una Composite Action y evita depender del sistema del caller.
