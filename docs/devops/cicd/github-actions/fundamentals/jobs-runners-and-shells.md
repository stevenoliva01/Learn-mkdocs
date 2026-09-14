---
title: Jobs, runners y shells
description: Ejecución, dependencias y entornos de trabajo.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Jobs, runners y shells

Un workflow contiene jobs. Cada job ejecuta steps en orden; los jobs sin dependencias pueden correr en paralelo.

## Dependencias

`needs` modela el orden entre jobs.

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - run: echo test
  deploy:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - run: echo deploy
```

## Runners

Los runners administrados por GitHub usan etiquetas como `ubuntu-latest`, `windows-latest` y `macos-latest`. Aportan máquinas limpias por job y poca administración. Un runner self-hosted usa `runs-on: self-hosted` y puede combinar labels como `linux` o `azure`; su operación avanzada se trata en [self-hosted runners](../security/self-hosted-runners.md).

## Shell y directorio

En Linux se usa normalmente Bash; Windows también puede ejecutar `pwsh` o `powershell`.

```yaml
defaults:
  run:
    shell: bash
    working-directory: backend

jobs:
  inspect:
    runs-on: windows-latest
    steps:
      - shell: pwsh
        run: Get-ChildItem
```

!!! note
    Define `working-directory` cuando el repositorio contiene varias aplicaciones; muchos fallos aparentes de build provienen de ejecutar el comando en la raíz equivocada.
