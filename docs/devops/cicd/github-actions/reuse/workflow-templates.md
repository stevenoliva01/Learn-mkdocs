---
title: Workflow Templates
description: Plantillas de inicio para estandarizar workflows en repositorios nuevos.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Workflow Templates

Un Workflow Template es una plantilla que ayuda a crear un archivo nuevo desde la interfaz de Actions. Resuelve el inicio consistente de un repositorio; no llama código central en runtime y no recibe cambios posteriores automáticamente.

## Cómo funciona

```text
Organización .github → workflow-templates/ → New workflow → YAML copiado al repositorio
```

Un repositorio especial `.github` puede publicar `workflow-templates/java-ci.yml` y su metadata `java-ci.properties.json`. El YAML puede usar `$default-branch` y la metadata describe el nombre, categorías y patrones que ayudan a recomendarla.

```yaml
# workflow-templates/java-ci.yml
name: Java CI
on:
  push:
    branches: [$default-branch]
  pull_request:
    branches: [$default-branch]
jobs:
  ci:
    uses: platform-engineering/workflows/.github/workflows/java-ci.yml@<sha>
    with: {java-version: "21"}
```

```json
{
  "name": "Java CI corporativo",
  "description": "Validación Maven estándar",
  "categories": ["Java"],
  "filePatterns": ["pom.xml"]
}
```

## Template frente a reutilización

La plantilla da un punto de partida que el repositorio pasa a poseer; un reusable workflow mantiene una implementación central invocada por cada run. Una organización grande puede combinar ambos: el template crea un caller pequeño y ese caller llama al reusable workflow central. Así estandariza adopción y mantiene la política sin duplicar jobs.

No uses un template si necesitas corregir centralmente todos los consumidores mañana. No uses un reusable workflow cuando el objetivo es enseñar o generar una configuración inicial que cada equipo debe adaptar conscientemente.

GitHub-provided workflow templates aparecen como punto de inicio en **Actions → New workflow**. Una custom organization workflow template es una capacidad de configuración organizacional; un reusable workflow y una Composite/Custom Action son mecanismos de reutilización durante el run. Antes de adoptar una template revisa triggers, `permissions`, editor, Actions de terceros y versiones. Practica esta diferencia en [Lab 18](../labs/lab-18-workflow-templates.md).

## YAML anchors

Los anchors reducen repetición dentro de **un mismo YAML**. No cruzan archivos ni repositorios, no sustituyen versionado y pueden ocultar el flujo a lectores nuevos. Úsalos con moderación para configuración local realmente idéntica.
