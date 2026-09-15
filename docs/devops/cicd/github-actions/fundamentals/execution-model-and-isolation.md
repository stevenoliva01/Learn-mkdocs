---
title: Modelo de ejecución y aislamiento
description: Qué comparte un job, qué se aísla entre jobs y cómo transportar resultados.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Modelo de ejecución y aislamiento

Entender el aislamiento evita dos fallos muy comunes: buscar código antes de hacer checkout y esperar que el archivo de `build` exista en `deploy`.

## Cómo funciona

```text
Workflow run
│
├── Job build → Runner A → checkout → compile → test
│
└── Job deploy → Runner B → download artifact → deploy
```

Los steps de un mismo job se ejecutan secuencialmente en su runner y comparten su workspace, sistema de archivos y variables exportadas a pasos posteriores. Cada job normalmente obtiene otro runner: su filesystem empieza vacío y no recibe automáticamente resultados del anterior. Un runner GitHub-hosted suele desaparecer después del job.

## Ejemplo: de build a deploy

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - run: ./mvnw -B package
      - uses: actions/upload-artifact@v7
        with:
          name: application
          path: target/*.jar
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/download-artifact@v8
        with:
          name: application
          path: package
      - run: ./deploy.sh package
```

El artifact es el contrato explícito entre jobs. Para un dato pequeño, como una versión, usa un [job output](../execution/outputs-and-workflow-commands.md), no un artifact ni una variable supuestamente global.

## Cuándo separar jobs

Separa cuando cambien permisos, environment, plataforma o responsabilidad; por ejemplo, test sin privilegios y deploy con `id-token: write`. Mantén steps juntos si requieren el mismo checkout y archivos temporales. Separar cada comando en un job hace más lento el pipeline y crea transporte innecesario.

!!! warning
    Un self-hosted runner persistente puede conservar estado, pero diseñar un workflow dependiendo de ese estado es frágil y amplía el riesgo de contaminación entre ejecuciones.

## Práctica recomendada

El [Lab 03 — Jobs y needs](../../../../courses/devops/github-actions/labs/lab-03-jobs-and-needs.md) hace visible el paralelismo y el archivo que desaparece al cambiar de job.
