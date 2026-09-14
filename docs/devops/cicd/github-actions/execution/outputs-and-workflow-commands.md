---
title: Outputs y comandos de workflow
description: Comunicación entre steps, jobs y el runner.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Outputs y comandos de workflow

Los outputs permiten pasar valores sin acoplar pasos a archivos temporales. Un step debe tener `id` para que sus outputs puedan consumirse.

```yaml
- id: version
  run: echo "value=1.2.3" >> "$GITHUB_OUTPUT"
- run: echo "${{ steps.version.outputs.value }}"
```

## Outputs entre jobs

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    outputs:
      version: ${{ steps.version.outputs.value }}
    steps:
      - id: version
        run: echo "value=1.2.3" >> "$GITHUB_OUTPUT"
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: echo "${{ needs.build.outputs.version }}"
```

## Archivos especiales

- `GITHUB_ENV`: variable disponible en steps posteriores del mismo job.
- `GITHUB_OUTPUT`: output de un step.
- `GITHUB_PATH`: agrega una ruta al `PATH` posterior.
- `GITHUB_STEP_SUMMARY`: publica un resumen Markdown de la ejecución.

```bash
echo "APP_VERSION=1.2.3" >> "$GITHUB_ENV"
echo "$HOME/bin" >> "$GITHUB_PATH"
echo "# Resultado" >> "$GITHUB_STEP_SUMMARY"
echo "::add-mask::$TOKEN"
echo "::warning::Cobertura menor a 80%"
echo "::error::Quality Gate falló"
```

!!! warning
    Enmascara valores con `add-mask`, pero no imprimas secretos. El enmascaramiento no reemplaza un manejo correcto de secretos.
