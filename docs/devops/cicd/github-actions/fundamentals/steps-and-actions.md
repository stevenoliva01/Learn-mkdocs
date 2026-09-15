---
title: Steps y Actions
description: Instrucciones de un job, acciones reutilizables y checkout seguro del código.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Steps y Actions

Los steps son las unidades secuenciales de un job. Cada uno ejecuta un comando con `run` o invoca una Action con `uses`; elegir bien evita repetir lógica, esconder dependencias o confiar ciegamente en código externo.

## Propiedades de un step

| Propiedad | Para qué sirve |
| --- | --- |
| `name` | Nombre visible en los logs. |
| `id` | Identificador para consumir outputs del step. |
| `run` | Ejecuta un comando en el shell del runner. |
| `uses` | Invoca una Action o una Action local. |
| `with` | Envía inputs a una Action. |
| `env` | Define variables para ese step. |
| `if` | Decide declarativamente si se ejecuta. |
| `shell` | Selecciona Bash, PowerShell u otro shell. |
| `working-directory` | Cambia la carpeta para `run`. |

```yaml
- name: Ejecutar pruebas del backend
  id: tests
  working-directory: backend
  env:
    PROFILE: ci
  if: ${{ github.event_name == 'pull_request' }}
  shell: bash
  run: ./mvnw -B test
```

## `run` frente a `uses`

`run` ejecuta instrucciones que el equipo posee y revisa, como `npm ci` o un script del repositorio. `uses` incorpora una Action: una unidad empaquetada que puede ser oficial, de Marketplace, propia, JavaScript, Docker o composite. Una Action reduce repetición, pero también introduce una dependencia que debe mantenerse y evaluarse.

```yaml
- uses: actions/setup-java@v6
  with:
    distribution: temurin
    java-version: "21"
- run: ./mvnw -B verify
```

## Checkout: el paso que crea el workspace

El runner no contiene automáticamente el repositorio que disparó el workflow. `actions/checkout` obtiene el commit del run y lo deja disponible para los steps siguientes:

```text
runner vacío → actions/checkout → workspace → compilación y pruebas
```

```yaml
- uses: actions/checkout@v7
- run: git rev-parse --short HEAD
```

Sin checkout, un comando que espera `pom.xml`, `package.json` o `./deploy.sh` fallará porque esos archivos no están presentes. Una Action externa debe provenir de una fuente confiable; para controles de alto riesgo, fija un SHA completo y conserva el comentario de versión para revisión humana. Amplía el tema en [seguridad de dependencias](../security/permissions-and-security.md).

## Cuándo usar cada mecanismo

Usa una Action oficial para una integración repetida y bien definida, como preparar Java o subir artifacts. Usa scripts del repositorio para reglas propias complejas. Cuando repites steps entre workflows, evalúa una [Composite Action](../reuse/composite-actions.md); si repites jobs completos, usa un [Reusable Workflow](../reuse/reusable-workflows.md).
