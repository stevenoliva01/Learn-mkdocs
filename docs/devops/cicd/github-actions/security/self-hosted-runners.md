---
title: Self-hosted runners
description: Operación y aislamiento de runners administrados por la organización.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Self-hosted runners

Un self-hosted runner es infraestructura propia registrada en GitHub. Puede atender labels específicos, redes privadas o workloads con software especializado.

```yaml
runs-on: [self-hosted, linux, azure]
```

Su ventaja operativa introduce responsabilidad de parcheo, identidades, secretos persistentes y aislamiento. Segmenta runners por confianza y workload; evita que código no confiable comparta un host con credenciales de alto valor.

Buenas prácticas: runners efímeros, imágenes inmutables, segmentación de red, identidades de mínimo privilegio y mantenimiento continuo. Kubernetes y runner controllers son alternativas conceptuales para gestionar escala y aislamiento.
