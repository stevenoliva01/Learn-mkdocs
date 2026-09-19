---
title: Jobs, runners y shells
description: Dependencias, plataformas de ejecución y directorios de trabajo.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Jobs, runners y shells

Un job agrupa steps que deben ejecutarse en el mismo entorno y en orden. Separa jobs cuando necesitas paralelismo, otra plataforma, otro límite de permisos o una frontera clara entre construir y desplegar.

## `job_id` frente a `name`: identificador y etiqueta

En `jobs`, la clave que inventa el autor es el **`job_id`**. No es una keyword de GitHub Actions ni un comando: es el identificador técnico, único dentro de ese workflow, que se usa en `needs` y en expresiones como `needs.build.outputs.version`. Debe empezar por letra o `_` y solo puede contener letras, números, `-` o `_`. Elige un nombre que describa la intención (`use-artifact`, `integration-tests`, `publish-image`), no un verbo ambiguo como `consume`.

`jobs.<job_id>.name`, en cambio, es una etiqueta opcional y legible que GitHub muestra en el gráfico y los logs. Puede contener espacios y no cambia las referencias técnicas.

```yaml
jobs:
  build:                         # job_id elegido por quien escribe el workflow
    name: Generate artifact       # etiqueta mostrada en la UI
    runs-on: ubuntu-latest
    steps: [{run: echo "build"}]
  use-artifact:                  # otro job_id, no una keyword
    name: Download and use artifact
    needs: build                 # referencia al job_id, no al name
    runs-on: ubuntu-latest
    steps: [{run: echo "after build"}]
```

La misma distinción aplica a `id` de un step: es una clave elegida por el autor para leer sus outputs, mientras que `steps[*].name` solo hace el log comprensible. Escribir ambos cuando aclaran la intención reduce errores al copiar un ejemplo.

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

`needs` expresa dependencia entre jobs, no transporta archivos: usa [artifacts](../execution/artifacts-cache-and-environments.md) o outputs para ello. Si el job requerido falla o se omite, los jobs que dependen de él se omiten normalmente; no conviertas esa protección en una suposición de que un archivo apareció en el runner siguiente.

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
