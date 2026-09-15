---
title: Conceptos y sintaxis de GitHub Actions
description: Modelo mental, workflow y YAML esencial para comenzar con GitHub Actions.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Conceptos y sintaxis de GitHub Actions

GitHub Actions convierte sucesos del repositorio en trabajo automatizado. Su valor no es el YAML: es que una misma definición deja visible, repetible y auditable cómo se valida, publica o despliega un cambio.

## ¿Qué problema resuelve?

Sin automatización, compilar, probar y desplegar depende de que una persona recuerde los pasos y tenga el entorno correcto. Un workflow declara ese proceso junto al código, de modo que cada cambio se puede comprobar de forma consistente.

## Modelo mental

```text
Evento
  ↓
Workflow run
  ↓
Job obtiene un runner
  ↓
Steps, en orden
  ↓
run (comando) / uses (Action)
```

Un **workflow** es un archivo YAML en `.github/workflows/` con extensión `.yml` o `.yaml`. GitHub lo descubre al existir en esa ruta de la rama pertinente; un **workflow run** es una ejecución concreta creada por un evento. Un workflow puede tener varios jobs y cada job tiene steps. Consulta el [modelo de ejecución y aislamiento](execution-model-and-isolation.md) para entender qué comparte cada job.

## Workflow, nombre y ejecución

`name` identifica la automatización en la pestaña Actions. `run-name` identifica una ejecución concreta y puede usar `github` e `inputs`; es especialmente útil para operaciones manuales.

```yaml
name: Validación de payments-api
run-name: Validación de ${{ github.ref_name }} por @${{ github.actor }}

on:
  push:
    branches: [main]

permissions:
  contents: read

jobs:
  verify:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - run: ./mvnw -B verify
```

El resultado es un run llamado, por ejemplo, `Validación de main por @ana`; reserva un runner, descarga el repositorio y ejecuta Maven. `runs-on` **no** descarga el código: `actions/checkout` lo coloca en el workspace antes de compilar.

## La sintaxis YAML que importa aquí

YAML usa mappings (`clave: valor`) y listas (`- valor`). La indentación expresa pertenencia: una línea desplazada pertenece a la clave anterior. Usa `|` para preservar saltos de línea y `>` para plegarlos en una línea. Las expresiones de Actions se escriben con `${{ }}`; no son variables de shell.

```yaml
env:
  APP_NAME: payments-api
  ENABLE_FEATURE: "true"

steps:
  - name: Verificar
    run: |
      echo "Aplicación: $APP_NAME"
      ./mvnw -B verify
  - if: ${{ github.ref_name == 'main' }}
    run: echo "La condición se evaluó antes de ejecutar el shell"
```

No uses valores ambiguos sin comillas si una herramienta puede interpretarlos de forma inesperada; revisa indentación, listas y comillas antes de culpar al runner. Una expresión se resuelve por Actions; `$APP_NAME` se expande después por Bash o PowerShell.

## Cuándo utilizarlo y cuándo no

Un workflow es apropiado para CI, despliegues controlados, mantenimiento recurrente y tareas que necesitan historial. No es buena opción para lógica de negocio extensa: conserva esa lógica en scripts o herramientas versionadas y deja el workflow como orquestador.

## Relación con otros conceptos

Los [eventos](events-and-inputs.md) deciden cuándo crear un run; los [steps y Actions](steps-and-actions.md) describen el trabajo; las [variables y expresiones](variables-contexts-and-expressions.md) aportan datos; `needs` y artifacts unen jobs separados.

!!! tip
    Un workflow pequeño con una responsabilidad explícita es más fácil de revisar, proteger y reutilizar que un único YAML que intenta hacer todo.
