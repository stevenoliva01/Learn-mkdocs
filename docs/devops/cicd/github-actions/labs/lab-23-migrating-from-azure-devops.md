---
title: Lab 23 — Migrar desde Azure DevOps
description: Analizar una pipeline de Azure DevOps y practicar Importer sin abrir un PR.
tags: [GitHub Actions, Azure DevOps, Laboratorios]
---

# Lab 23 — Migrar desde Azure DevOps

## Objetivo

Migrar mentalmente un pipeline pequeño, distinguir equivalencias de decisiones y usar GitHub Actions Importer solo en análisis/dry-run. Lee [Migrar desde Azure DevOps](../advanced/migrating-from-azure-devops.md).

## Parte A — Conversión razonada

Parte de este ejemplo conceptual de Azure Pipelines: trigger en `main`, pool Linux, variable `NODE_VERSION`, checkout, `NodeTool`, `npm ci`, `npm test` y publicación de `dist/`. Clasifica: trigger → `push`; pool → runner; variable → `env`/input de setup-node; task → Action o `run`; publish artifact → `actions/upload-artifact`. Construye el workflow incrementalmente y explica por qué `dist/` es artifact, no cache. `ci` es un job_id inventado por ti.

No traduzcas sintaxis literalmente: revisa permisos, hashes/tags de Actions, branch protection, environments, secrets y si el checkout implícito de Azure debe ser `actions/checkout` explícito.

## Parte B — Importer sin mutación remota

En un entorno autorizado con Docker y GitHub CLI, consulta primero ayuda y configura credenciales fuera de archivos versionados:

```bash
gh extension install github/gh-actions-importer
gh actions-importer configure
gh actions-importer audit azure-devops --output-dir tmp/audit
gh actions-importer forecast azure-devops --output-dir tmp/forecast
gh actions-importer dry-run azure-devops pipeline --pipeline-id PIPELINE_ID --output-dir tmp/dry-run
```

Inspecciona `audit_summary.md`, forecast, logs y YAML convertido. No ejecutes `migrate`: puede abrir un PR. Documenta manualmente tasks desconocidas, service connections, secrets, environments, approvals y agents. Borra `tmp/` si contiene tokens o datos internos y no lo subas. Sigue con [Lab 24](lab-24-rulesets-and-required-workflows.md).
