---
title: Pipelines Java y Node
description: Integración continua para Maven y Angular o Node.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Pipelines Java y Node

Estas integraciones describen cómo orquestar las herramientas desde GitHub Actions, no sustituyen su documentación propia.

## Java / Maven

```yaml
- uses: actions/checkout@v7
- uses: actions/setup-java@v6
  with:
    distribution: temurin
    java-version: "21"
    cache: maven
- run: mvn -B verify
- uses: actions/upload-artifact@v7
  with:
    name: reports
    path: target/
```

La caché de Maven acelera dependencias; los reports o paquetes resultantes pueden publicarse como artifacts.

## Node / Angular

```yaml
- uses: actions/checkout@v7
- uses: actions/setup-node@v7
  with:
    node-version: "24"
    cache: npm
- run: npm ci
- run: npm run lint
- run: npm test
- run: npm run build
- uses: actions/upload-artifact@v7
  with:
    name: frontend-dist
    path: dist/
```

Usa `npm ci` para una instalación reproducible basada en el lockfile.
