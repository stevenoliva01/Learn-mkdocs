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

## Operar el runner, no solo instalarlo

Un runner self-hosted se registra en el alcance repository, organization o enterprise, descarga la aplicación runner y puede instalarse como servicio. Selecciona jobs con `runs-on: [self-hosted, linux, private-network]`: son labels, no permisos. Los runner groups limitan qué repositorios pueden usar un conjunto de runners; combínalos con labels de capacidad y confianza. Mantén inventario, logs, parches, actualizaciones, rotación de credenciales y un procedimiento de eliminación que quite registro y servicio cuando el host deje de ser necesario.

Los GitHub-hosted usan imágenes mantenidas por GitHub, software preinstalado y filesystem efímero. Consulta *Set up job* para la imagen y herramientas disponibles; instala una herramienta adicional dentro del job cuando haga falta. El tools cache puede evitar descargas repetidas, pero no guarda entregables. Los self-hosted requieren decidir actualizaciones y red privada; no dependas de archivos dejados por runs previos.

## Persistentes, efímeros, JIT y ARC

Un runner **persistente** acepta varios jobs y puede retener contaminación entre ellos. Un runner **efímero** acepta un único job y se retira; GitHub recomienda este modelo para autoscaling porque reduce estado residual y simplifica limpieza. El registro con `--ephemeral` expresa esa intención; el orquestador aún debe destruir o sanear la infraestructura y conservar logs externos.

Un runner **Just-In-Time (JIT)** recibe una configuración de corto alcance para registrar capacidad bajo demanda. En Kubernetes, Actions Runner Controller puede coordinar el ciclo:

```text
GitHub → ARC → Runner Scale Set → ephemeral runner pods → jobs
```

ARC y runner scale sets ayudan a escalar runners efímeros; no sustituyen segmentación de red, revisión de eventos o permisos mínimos.

## Buenas prácticas

Prefiere runners efímeros, imágenes inmutables, identidades de mínimo privilegio, segmentación de red y grupos separados por workload. Registra inventario, parchea el host y revoca su registro cuando deja de usarse. Controladores de runners en Kubernetes pueden ayudar a escalar y aislar, pero no sustituyen la evaluación de confianza del evento.

No uses un self-hosted runner como atajo para acceder a producción desde un PR. Separa CI no confiable de despliegues autorizados por branch, environment y permisos.
