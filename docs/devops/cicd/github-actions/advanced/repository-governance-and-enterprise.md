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
