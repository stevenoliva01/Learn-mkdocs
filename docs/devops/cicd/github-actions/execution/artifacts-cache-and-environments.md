---
title: Artefactos, caché y entornos
description: Resultados persistentes, aceleración de dependencias y promoción controlada.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Artefactos, caché y entornos

Artifacts, cache y environments resuelven problemas distintos. Confundirlos produce pipelines lentos o despliegues con controles implícitos.

## Artifacts: mover un resultado, no una suposición

Un job normalmente termina en un runner distinto del siguiente. Por eso un archivo creado por `build` —un JAR, ZIP, `dist/`, cobertura, resultados de tests, una SBOM, reportes de seguridad o un `tfplan`— no aparece por arte de magia en `use-artifact`. Un **artifact** es uno o varios archivos que GitHub almacena para el workflow run: persiste el resultado y permite que otro job lo descargue.

```text
Runner A
Job build
└── app.txt
     │ upload-artifact
     ▼
GitHub Artifact Storage
     │ download-artifact
     ▼
Runner B
Job use-artifact
└── app.txt
```

El flujo es `upload-artifact` → almacenamiento de GitHub → `download-artifact`. `needs` espera a `build`, pero no mueve nada; ambos mecanismos son necesarios cuando el segundo job debe utilizar un archivo generado por el primero.

```yaml
jobs:
  build:                           # job_id elegido por el autor
    name: Generate artifact         # texto visible en la UI
    runs-on: ubuntu-latest
    steps:
      - name: Generate the file
        run: echo "version=1.0.0" > app.txt
      - name: Upload application artifact
        uses: actions/upload-artifact@v7
        with:
          name: application         # nombre lógico del artifact; lo elige el autor
          path: app.txt              # archivo o patrón que se sube desde este runner
          retention-days: 1          # días que GitHub lo conserva, sujeto al límite configurado
  use-artifact:                     # job_id; no es una keyword
    name: Download and use artifact
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Download application artifact
        uses: actions/download-artifact@v8
        with:
          name: application         # debe coincidir con el nombre usado al subirlo
          path: downloaded           # directorio de destino en el runner actual
      - name: Read the downloaded file
        run: cat downloaded/app.txt
```

`uses` invoca una Action reutilizable. `actions/upload-artifact` empaqueta y guarda los archivos indicados; `actions/download-artifact` recupera un artifact del run. `with` es el mapa de parámetros de esa Action: consulta siempre sus inputs, no supongas que `name` o `path` significan lo mismo para otra Action. “Consumir un artifact” es lenguaje de arquitectura, no una keyword: significa descargar y usar un resultado que un job anterior produjo.

### Artifact, cache u output

| Necesidad | Mecanismo | Por qué |
| --- | --- | --- |
| Entregar JAR, `dist/`, informe o `tfplan` a otro job o a una persona | Artifact | Conserva archivos identificables de un run. |
| Reutilizar dependencias descargadas entre runs para ahorrar tiempo | Cache | Es una optimización con claves; no es un entregable. |
| Pasar versión, URL o booleano pequeño entre jobs | Job output | Es un valor de contrato, no un sistema de archivos. |

No uses cache para promover un binario ni un artifact para una simple versión. Para valores pequeños, consulta [Outputs y comandos de workflow](outputs-and-workflow-commands.md).

| | Artifact | Cache |
| --- | --- | --- |
| Propósito | Conservar resultado | Acelerar ejecuciones |
| Ejemplo | JAR, cobertura, SBOM, plan | Maven `.m2`, npm |
| Consumo humano | Frecuente | No es el objetivo |
| Entre jobs | Sí, download explícito | Posible por clave, no contrato |
| Persistencia | Retención configurada | Gestionada como caché |

```text
build → app.jar → upload-artifact → deploy → download-artifact
```

```yaml
- uses: actions/upload-artifact@v7
  with:
    name: payments-api
    path: target/*.jar
    retention-days: 7
```

Un artifact conserva un resultado concreto de este run. Una cache se identifica por una key y puede restaurarse desde keys parecidas; úsala para dependencias reproducibles, no para publicar el entregable. `actions/setup-java` y `actions/setup-node` pueden gestionar caches de Maven y npm. Cuando necesites una caché propia, define ruta, key basada en lockfiles y `restore-keys` con cuidado.

### Cache: una búsqueda por clave, no un canal entre jobs

Una cache guarda una ruta reutilizable entre ejecuciones. `key` identifica exactamente el contenido esperado; debe cambiar cuando cambie el lockfile. `restore-keys` son prefijos de respaldo: pueden recuperar una cache cercana, pero el gestor de paquetes sigue siendo responsable de verificar e instalar lo necesario. En `actions/setup-node`, `cache: npm` pide a esa Action que gestione la cache global de npm a partir del lockfile; no guarda `node_modules` como si fuera un artifact.

```yaml
- name: Restore npm cache
  uses: actions/cache@v4
  with:
    path: ~/.npm
    key: npm-${{ runner.os }}-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      npm-${{ runner.os }}-
```

`actions/cache` busca primero `key` y después los prefijos de `restore-keys`. Un *cache hit* solo indica que se restauró una entrada; no demuestra que el build sea correcto. Ejecuta todavía `npm ci` o el comando equivalente.

`path` puede señalar un archivo, directorio o patrón wildcard. Configura `if-no-files-found: error` cuando la ausencia de JAR, informe o SBOM haga inválido el run; el valor por defecto solo advierte. `retention-days` controla cuánto se conserva, sujeto al límite de repositorio. Un artifact es adecuado para JAR, ZIP, `dist/`, `coverage.xml`, JUnit XML, SBOM, reportes de seguridad o `tfplan`; evalúa sensibilidad antes de publicar un plan. Un release asset pertenece a una release publicada y su ciclo de vida es distinto del artifact de un run.

Practica el movimiento de archivos entre runners en [Lab 05 — Artifacts](../labs/lab-05-artifacts.md) y la optimización por clave en [Lab 06 — Cache](../labs/lab-06-cache.md).

## Environments

Un GitHub Environment (`dev`, `pre`, `production`) representa un destino de despliegue con historial, variables, secretos y, según disponibilidad, reglas de protección. No es simplemente una variable llamada `environment`.

```yaml
deploy-production:
  environment:
    name: production
  permissions:
    contents: read
    id-token: write
  runs-on: ubuntu-latest
```

Los controles del environment se aplican al job que lo referencia, por lo que es una frontera adecuada para una promoción. Un input que dice `prod` no concede acceso por sí solo. Mantén un artifact trazable desde build hasta deploy y valida reglas, approvals y plan de GitHub antes de depender de ellos.

## Práctica recomendada

El [Lab 05](../labs/lab-05-artifacts.md) transporta un archivo entre jobs, el [Lab 06](../labs/lab-06-cache.md) observa cache hit/miss y el [Lab 11](../labs/lab-11-environments.md) registra promociones simuladas.
