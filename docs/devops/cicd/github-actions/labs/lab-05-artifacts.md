---
title: Lab 05 - Artifacts
description: Transportar un archivo entre runners mediante un artifact explícito.
tags: [GitHub Actions, Laboratorios]
---

# Lab 05 — Artifacts

## Objetivo y modelo mental

En el Lab 03 `build` creó `app.txt`, pero el archivo desapareció al pasar a otro job: cada job normalmente obtiene otro runner. Aquí haremos explícito el puente: `upload-artifact` guarda el archivo en GitHub y `download-artifact` lo recupera en el runner siguiente. Lee antes [Artefactos, caché y entornos](../execution/artifacts-cache-and-environments.md) y [Jobs, runners y shells](../fundamentals/jobs-runners-and-shells.md).

```text
Job build / Runner A → upload-artifact → GitHub → download-artifact → Job use-artifact / Runner B
```

`build` y `use-artifact` son **job_id** elegidos por nosotros; no son keywords. `name:` es solo el texto amigable visible en la interfaz. `needs: build` espera el job cuyo id es `build`; no transporta archivos por sí mismo.

## Paso 1 — Crear el workflow y el job que genera el archivo

Crea `.github/workflows/lab-05.yml` con el trigger manual y un job llamado `build`:

```yaml
name: Lab 05 - Artifacts
on: {workflow_dispatch: {}}
jobs:
  build:
    name: Generate artifact
    runs-on: ubuntu-latest
    steps:
      - name: Generate app.txt
        run: echo "version=1.0.0" > app.txt
```

`runs-on` reserva el runner. `run` ejecuta el comando en su shell y genera el archivo dentro de ese runner. Publica el archivo, ejecútalo manualmente y confirma en el log que el step terminó verde.

## Paso 2 — Subir el resultado como artifact

Añade este step dentro de `build.steps`, después de crear el archivo:

```yaml
      - name: Upload application artifact
        uses: actions/upload-artifact@v7
        with:
          name: application
          path: app.txt
          retention-days: 1
```

`uses` invoca la Action oficial `actions/upload-artifact`; empaqueta y guarda los archivos seleccionados. `with` entrega sus parámetros: `name` es el identificador lógico del artifact, `path` es el archivo que se sube y `retention-days` fija una retención de un día, sin superar el límite del repositorio. Ejecuta otra vez: en el resumen del run debe aparecer **Artifacts → application**. Al terminar este job, también termina su runner.

## Paso 3 — Crear el job que descarga y usa el artifact

Ahora añade un segundo job al mismo nivel que `build`:

```yaml
  use-artifact:
    name: Download and use artifact
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Download application artifact
        uses: actions/download-artifact@v8
        with:
          name: application
          path: downloaded
      - name: Read the downloaded file
        run: cat downloaded/app.txt
```

`use-artifact` expresa mejor la intención que `consume`: ambos serían nombres válidos de autor, pero ninguno es parte de GitHub Actions. `actions/download-artifact` recupera el artifact cuyo `name` coincide exactamente con el de subida y lo coloca en el directorio `path` del nuevo runner. El último `run` demuestra que el archivo fue transportado, no compartido implícitamente.

## Paso 4 — Ejecutar, observar y diagnosticar

Haz commit, push y selecciona **Actions → Lab 05 - Artifacts → Run workflow**. Debes observar:

- `Generate artifact` verde; sus logs muestran la creación y subida.
- `Download and use artifact` iniciado solo después de `build` y con `version=1.0.0` en el log.
- **Artifacts → application** en el resumen del run, descargable desde la UI.

Rompe el contrato cambiando el `name` de descarga a `missing-application`. El segundo job debe fallar porque no existe ese artifact. Restaura `application`; no intentes arreglarlo eliminando `needs`, porque eso solo quitaría el orden y empeoraría el problema.

## Cleanup y siguiente paso

El artifact expira tras un día o puedes borrarlo antes desde GitHub. El workflow no crea infraestructura. Continúa con [Lab 06 — Cache](lab-06-cache.md): allí reutilizarás dependencias para acelerar un run, que es un objetivo distinto de trasladar un entregable.
