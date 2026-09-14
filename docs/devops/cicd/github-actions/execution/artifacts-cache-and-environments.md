---
title: Artefactos, caché y entornos
description: Persistencia de resultados, aceleración y promociones.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Artefactos, caché y entornos

Un artifact conserva un resultado de ejecución —JAR, reportes, cobertura, plan de Terraform o SBOM—; una caché reutiliza dependencias para acelerar ejecuciones posteriores.

```yaml
- uses: actions/upload-artifact@v7
  with:
    name: app
    path: target/*.jar
    retention-days: 7

- uses: actions/setup-node@v7
  with:
    node-version: "24"
    cache: npm

- uses: actions/setup-java@v6
  with:
    distribution: temurin
    java-version: "21"
    cache: maven
```

Para un caso no cubierto por las actions de preparación, `actions/cache@v4` permite definir ruta, clave y `restore-keys`.

## Environments y promoción

Los environments representan `dev`, `pre` y `prod`; pueden contener variables, secretos, ramas autorizadas y reglas de protección.

```yaml
deploy-prod:
  environment:
    name: production
  runs-on: ubuntu-latest
```

El mismo workflow debe cambiar por configuración del environment, no por copias de YAML. Las aprobaciones, reviewers y protecciones dependen del plan y tipo de repositorio: confirma su disponibilidad antes de diseñar el control.

Consulta [diagnóstico, costos y anti-patrones](../advanced/troubleshooting-costs-and-antipatterns.md) para retención y uso eficiente.
