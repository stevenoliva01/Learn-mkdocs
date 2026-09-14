---
title: Estrategia y control de ejecución
description: Matrices, concurrencia, límites y tolerancia a fallos.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Estrategia y control de ejecución

Una matriz ejecuta el mismo job para varias combinaciones de valores.

```yaml
strategy:
  fail-fast: false
  max-parallel: 2
  matrix:
    os: [ubuntu-latest, windows-latest]
    java: ["17", "21"]
    exclude:
      - os: windows-latest
        java: "17"
    include:
      - os: ubuntu-latest
        java: "25"
        experimental: true
```

`include` añade combinaciones y `exclude` elimina combinaciones. `fail-fast: false` deja finalizar las demás ejecuciones de una matriz tras un fallo; `max-parallel` limita simultaneidad.

## Concurrencia y límites

```yaml
concurrency:
  group: ci-${{ github.ref }}
  cancel-in-progress: true

jobs:
  test:
    timeout-minutes: 20
```

Cancelar ejecuciones obsoletas ahorra recursos en CI. En producción, evalúa cuidadosamente si una ejecución en curso debe cancelarse.

## Errores y reintentos

`continue-on-error` es apropiado para una comprobación experimental o no bloqueante, no para ocultar un fallo de calidad, seguridad o despliegue. Para errores transitorios, el workflow puede aplicar un reintento explícito.

```bash
for i in 1 2 3; do
  curl -f https://example.com/health && exit 0
  sleep 5
done
exit 1
```
