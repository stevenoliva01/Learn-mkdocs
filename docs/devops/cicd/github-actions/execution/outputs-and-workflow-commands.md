---
title: Outputs y comandos de workflow
description: Comunicación explícita entre steps, jobs y la interfaz de ejecución.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Outputs y comandos de workflow

Un resultado calculado debe viajar por un contrato explícito, no por una suposición sobre archivos, variables o runners. Hay tres niveles: `step output` → `job output` → otro job mediante `needs`.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    outputs:
      version: ${{ steps.version.outputs.value }}
    steps:
      - id: version
        run: echo "value=1.4.0" >> "$GITHUB_OUTPUT"
      - run: echo "Versión local: ${{ steps.version.outputs.value }}"
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: echo "Desplegar ${{ needs.build.outputs.version }}"
```

`GITHUB_OUTPUT` crea un output del step; exponerlo en `jobs.<id>.outputs` lo hace disponible al job dependiente. Para un binario no uses output: usa [artifact](artifacts-cache-and-environments.md).

## Archivos especiales

| Archivo | Resultado |
| --- | --- |
| `GITHUB_ENV` | Variable disponible en steps posteriores del mismo job |
| `GITHUB_OUTPUT` | Output de un step identificado |
| `GITHUB_PATH` | Añade una ruta al `PATH` posterior |
| `GITHUB_STEP_SUMMARY` | Publica Markdown en el resumen del run |

```bash
echo "APP_VERSION=1.4.0" >> "$GITHUB_ENV"
echo "$HOME/bin" >> "$GITHUB_PATH"
echo "## Verificación aprobada" >> "$GITHUB_STEP_SUMMARY"
```

El step que escribe `GITHUB_ENV` no ve todavía el valor; lo ve el siguiente. No uses estos archivos para mover secretos entre jobs ni para reemplazar los mecanismos de autorización.

!!! warning
    `::add-mask::` puede reducir exposición accidental, pero no convierte un secreto en seguro para imprimirlo. No lo escribas en logs ni summaries.
