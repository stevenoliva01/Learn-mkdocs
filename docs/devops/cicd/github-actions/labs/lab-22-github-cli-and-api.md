---
title: Lab 22 — GitHub CLI y API
description: Consultar datos del repositorio mediante GITHUB_TOKEN y permisos mínimos.
tags: [GitHub Actions, Laboratorios, API]
---

# Lab 22 — GitHub CLI y API

## Objetivo

Usar `gh api` para una lectura segura desde un workflow. Revisa [GitHub CLI y API](../advanced/github-cli-and-api.md) y [Permisos y seguridad](../security/permissions-and-security.md).

## Implementación incremental

Crea un workflow manual. Declara `permissions: contents: read`, un job_id `repository-info` y el siguiente step. `GH_TOKEN` es la variable que consume GitHub CLI; `github.token` es el token temporal entregado por Actions.

```yaml
- name: Read repository metadata
  env:
    GH_TOKEN: ${{ github.token }}
  run: |
    gh api repos/${{ github.repository }} --jq '{name: .full_name, default_branch: .default_branch}'
```

Ejecuta y confirma que el log muestra solo metadata no sensible. Si aparece autorización denegada, inspecciona el permiso del workflow y las políticas del repositorio; no generes un PAT por reflejo. Como ejercicio opcional controlado, diseña una escritura idempotente en un repositorio de prueba y declara exactamente el permiso necesario, pero no la ejecutes por defecto. Limpia el workflow y continúa con [Lab 23](lab-23-migrating-from-azure-devops.md).
