---
title: Artefactos, caché y entornos
description: Resultados persistentes, aceleración de dependencias y promoción controlada.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Artefactos, caché y entornos

Artifacts, cache y environments resuelven problemas distintos. Confundirlos produce pipelines lentos o despliegues con controles implícitos.

| | Artifact | Cache |
| --- | --- | --- |
| Propósito | Conservar resultado | Acelerar ejecuciones |
| Ejemplo | JAR, cobertura, SBOM, plan | Maven `.m2`, npm |
| Consumo humano | Frecuente | No es el objetivo |
| Entre jobs | Sí, download explícito | Posible por clave, no contrato |
| Persistencia | Retención configurada | Gestionada como caché |

```text
build → app.jar → upload-artifact → deploy → download-artifact
```

```yaml
- uses: actions/upload-artifact@v7
  with:
    name: payments-api
    path: target/*.jar
    retention-days: 7
```

Un artifact conserva un resultado concreto de este run. Una cache se identifica por una key y puede restaurarse desde keys parecidas; úsala para dependencias reproducibles, no para publicar el entregable. `actions/setup-java` y `actions/setup-node` pueden gestionar caches de Maven y npm. Cuando necesites una caché propia, define ruta, key basada en lockfiles y `restore-keys` con cuidado.

## Environments

Un GitHub Environment (`dev`, `pre`, `production`) representa un destino de despliegue con historial, variables, secretos y, según disponibilidad, reglas de protección. No es simplemente una variable llamada `environment`.

```yaml
deploy-production:
  environment:
    name: production
  permissions:
    contents: read
    id-token: write
  runs-on: ubuntu-latest
```

Los controles del environment se aplican al job que lo referencia, por lo que es una frontera adecuada para una promoción. Un input que dice `prod` no concede acceso por sí solo. Mantén un artifact trazable desde build hasta deploy y valida reglas, approvals y plan de GitHub antes de depender de ellos.

## Práctica recomendada

El [Lab 05](../labs/lab-05-artifacts.md) transporta un archivo entre jobs, el [Lab 06](../labs/lab-06-cache.md) observa cache hit/miss y el [Lab 11](../labs/lab-11-environments.md) registra promociones simuladas.
