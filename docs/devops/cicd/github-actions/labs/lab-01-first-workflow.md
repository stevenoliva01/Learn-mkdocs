---
title: Lab 01 - Primer workflow
description: Ejecutar manualmente el primer workflow y leer sus logs.
tags: [GitHub Actions, Laboratorios]
---

# Lab 01 — Primer workflow

## Objetivo

Crear un workflow manual y comprobar que GitHub reserva un runner y publica sus logs. Antes de empezar, lee [Conceptos y sintaxis](../../../../devops/cicd/github-actions/fundamentals/concepts-and-syntax.md).

## Prerrequisitos y archivos

En tu repositorio GitHub de pruebas crea `.github/workflows/lab-01.yml`:

```yaml
name: Lab 01 - Primer workflow
run-name: Primer workflow por @${{ github.actor }}
on:
  workflow_dispatch:
jobs:
  hello:
    runs-on: ubuntu-latest
    steps:
      - name: Mostrar contexto mínimo
        run: |
          echo "Hola desde GitHub Actions"
          echo "Runner: $RUNNER_OS"
          echo "Commit: $GITHUB_SHA"
```

`workflow_dispatch` evita que cada commit cree un run mientras aprendes. Publica el archivo:

```bash
git add .github/workflows/lab-01.yml
git commit -m "lab: add first workflow"
git push origin main
```

## Ejecutar y validar

Abre **Actions → Lab 01 - Primer workflow → Run workflow → Run workflow**. Debes observar un run verde con el job `hello` y tres líneas de log; el nombre incluye tu usuario. Cambia `Hola` por otro texto, vuelve a hacer push y ejecútalo de nuevo: el segundo run debe mostrar el texto nuevo.

## Reto y troubleshooting

Rompe la indentación de `steps`, haz push y observa que GitHub no puede cargar el workflow o muestra un error YAML. Corrige la indentación. Si no aparece **Run workflow**, comprueba ruta, extensión y que el archivo exista en la rama predeterminada.

## Limpieza y siguiente paso

Conserva el archivo para comparar después o elimínalo con un commit. No crea recursos persistentes. Continúa con [Lab 02 — Eventos](lab-02-events.md).
