---
title: Jobs, runners y shells
description: Dependencias, plataformas de ejecución y directorios de trabajo.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Jobs, runners y shells

Un job agrupa steps que deben ejecutarse en el mismo entorno y en orden. Separa jobs cuando necesitas paralelismo, otra plataforma, otro límite de permisos o una frontera clara entre construir y desplegar.

## Dependencias con `needs`

Los jobs sin `needs` pueden empezar en paralelo. `needs` construye un grafo y evita que un deploy empiece si no terminó la validación requerida.

```text
        build
       /     \
    test    scan
       \     /
       deploy
```

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps: [{run: ./mvnw -B package}]
  test:
    needs: build
    runs-on: ubuntu-latest
    steps: [{run: ./mvnw -B test}]
  scan:
    needs: build
    runs-on: ubuntu-latest
    steps: [{run: ./scan.sh}]
  deploy:
    needs: [test, scan]
    runs-on: ubuntu-latest
    steps: [{run: ./deploy.sh pre}]
```

`needs` expresa dependencia entre jobs, no transporta archivos: usa [artifacts](../execution/artifacts-cache-and-environments.md) o outputs para ello.

## Runners: elegir el lugar de ejecución

Los runners GitHub-hosted (`ubuntu-latest`, `windows-latest`, `macos-latest`) simplifican la operación y normalmente son efímeros. Úsalos para CI común. Los self-hosted permiten red privada, hardware o software especializado, pero trasladan a la organización el parcheo, aislamiento y control de credenciales; se desarrollan en [Self-hosted runners](../security/self-hosted-runners.md).

```yaml
runs-on: ubuntu-latest
# o, cuando el workload está autorizado para infraestructura propia:
runs-on: [self-hosted, linux, private-network]
```

No elijas self-hosted solo para acelerar una ejecución: primero mide caché, paralelismo y filtros. Nunca asumas que un runner persistente es seguro para código de un fork.

## Shell y `working-directory`

En Linux el shell habitual es Bash; Windows puede usar `pwsh` o `cmd`. La misma instrucción no tiene siempre la misma sintaxis ni rutas. `working-directory` evita ejecutar comandos de un monorepo desde la raíz equivocada.

```text
repo/
├── backend/
├── frontend/
└── infrastructure/
```

```yaml
defaults:
  run:
    shell: bash
    working-directory: backend

jobs:
  frontend:
    runs-on: windows-latest
    defaults:
      run:
        working-directory: frontend
    steps:
      - uses: actions/checkout@v7
      - shell: pwsh
        run: npm ci; npm run build
```

El valor de `defaults.run` se aplica a `run`, no a `uses`. Configúralo a nivel de workflow para una norma común o de job cuando cada aplicación necesita un directorio distinto.

## Relación con Azure DevOps

Un runner se parece a un agent y `needs` a `dependsOn`; la comparación completa explica por qué no toda equivalencia es literal en [Azure DevOps y GitHub Actions](../advanced/azure-devops-comparison.md).
