---
title: Gobierno de repositorio y empresa
description: Controles compartidos, responsabilidades y estandarización de entrega.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Gobierno de repositorio y empresa

El gobierno evita depender de convenciones no comprobables. Branch protection, rulesets y required checks determinan qué debe pasar antes de integrar; CODEOWNERS asigna revisión de áreas sensibles; environments protegen promoción; permisos limitan el radio de impacto.

```text
Platform Team
├── Workflow Templates
├── Reusable Workflows
└── Policies
     ↓
Product Teams → Repositories
```

Platform Engineering ofrece contratos mantenidos, templates de adopción y políticas. Los equipos de producto siguen siendo responsables de sus triggers, datos, decisiones de release y respuesta a fallos. Centralizar no significa retirar ownership.

Un patrón útil es un [Workflow Template](../reuse/workflow-templates.md) que crea un caller pequeño hacia un [Reusable Workflow](../reuse/reusable-workflows.md). Define versionado, compatibilidad y proceso de excepción antes de forzar la adopción. No conviertas cada YAML local en un componente central: centraliza controles que realmente son comunes y estables.

## Reglas de merge vigentes

Un ruleset puede combinar protección de ramas/tags, revisiones, required status checks y, cuando la disponibilidad del producto lo permita, requisitos de workflow antes del merge. Define con precisión la rama, los actores exentos, el check/workflow esperado y la política de `merge_group` si usas merge queue. No adoptes “Required Workflows” como etiqueta histórica: revisa la configuración de rulesets y documenta el comportamiento actual. [Lab 24](../labs/lab-24-rulesets-and-required-workflows.md) es una práctica opcional y dependiente de plan.
