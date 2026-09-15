---
title: Self-hosted runners
description: Operación, aislamiento y riesgo de infraestructura propia para GitHub Actions.
tags: [GitHub Actions, CI/CD, DevOps, Security]
---

# Self-hosted runners

Un self-hosted runner permite ejecutar jobs en infraestructura controlada por la organización. Resuelve necesidades de red privada, software especial o capacidad concreta, pero convierte el runner en un activo que se debe operar y defender.

| Aspecto | GitHub-hosted | Self-hosted |
| --- | --- | --- |
| Administración | GitHub | Organización |
| Entorno | Normalmente efímero | Puede ser persistente |
| Red privada | Limitada | Posible |
| Patching y credenciales | Menor carga propia | Responsabilidad propia |
| Riesgo por código no confiable | Menor aislamiento compartido | Alto si el host persiste |

```text
GitHub → Self-hosted runner → VNet privada → AKS, DB o recursos internos
```

```yaml
runs-on: [self-hosted, linux, private-network]
```

Los labels seleccionan capacidad y deben representar una frontera de confianza, no solo un sistema operativo. Un host persistente puede retener archivos, herramientas o credenciales; código no confiable + runner persistente + credenciales de valor es una combinación de alto riesgo.

## Buenas prácticas

Prefiere runners efímeros, imágenes inmutables, identidades de mínimo privilegio, segmentación de red y grupos separados por workload. Registra inventario, parchea el host y revoca su registro cuando deja de usarse. Controladores de runners en Kubernetes pueden ayudar a escalar y aislar, pero no sustituyen la evaluación de confianza del evento.

No uses un self-hosted runner como atajo para acceder a producción desde un PR. Separa CI no confiable de despliegues autorizados por branch, environment y permisos.
