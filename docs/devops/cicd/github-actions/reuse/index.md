---
title: Reutilización en GitHub Actions
description: Elegir el mecanismo correcto para estandarizar automatización sin duplicarla.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Reutilización

Reutilizar no significa ocultar YAML: significa publicar un contrato con una responsabilidad clara. El mecanismo depende de qué se quiere compartir.

| Mecanismo | Reutiliza | Se ejecuta como | Uso típico |
| --- | --- | --- | --- |
| Reusable Workflow | Jobs o workflow | Job | Pipeline estándar corporativo |
| Composite Action | Steps | Step | Operación repetida y local |
| JavaScript/Docker Action | Lógica especializada | Step | Integración o lógica empaquetada |
| Workflow Template | Scaffold inicial | Archivo generado | Estándar para repositorios nuevos |
| YAML anchors | Fragmento YAML local | Mismo workflow | Reducir repetición pequeña |

Un template no se reutiliza en runtime; una composite action no puede sustituir un job completo. Tampoco existe una equivalencia única con un Azure DevOps template: depende de si el template compartía steps, jobs o el pipeline.

<div class="grid cards" markdown>

- [**Reusable Workflows**](reusable-workflows.md): contratos de jobs, inputs, secretos, outputs y versionado.
- [**Composite Actions**](composite-actions.md): empaquetar steps repetidos como una Action.
- [**Custom Actions**](custom-actions.md): Actions JavaScript y Docker para lógica empaquetada.
- [**Workflow Templates**](workflow-templates.md): punto de partida gobernado para repositorios nuevos.

</div>

Los YAML anchors son útiles solo dentro del mismo archivo y no sustituyen contratos reutilizables. Prioriza claridad sobre eliminar cada línea repetida.

## Práctica recomendada

Construye callers reales en el [Lab 09 — Reusable Workflows](../labs/lab-09-reusable-workflows.md) y una Action local en el [Lab 10 — Composite Actions](../labs/lab-10-composite-actions.md).
