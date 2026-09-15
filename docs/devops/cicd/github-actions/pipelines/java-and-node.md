---
title: Pipelines Java y Node
description: Integración continua progresiva para Maven, Node y Angular.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Pipelines Java y Node

Una pipeline tecnológica orquesta herramientas que ya deben poder ejecutarse localmente. El workflow no reemplaza Maven, npm ni sus lockfiles: hace visible el orden, entorno y resultado esperados.

## Java / Maven

```text
checkout → setup-java → cache → compile → unit tests → verify → artifact
```

```yaml
steps:
  - uses: actions/checkout@v7
  - uses: actions/setup-java@v6
    with:
      distribution: temurin
      java-version: "21"
      cache: maven
  - run: ./mvnw -B compile
  - run: ./mvnw -B test
  - run: ./mvnw -B verify
  - uses: actions/upload-artifact@v7
    with: {name: maven-reports, path: target/}
```

`setup-java` instala y añade al PATH la distribución solicitada; su cache acelera dependencias, pero no publica el paquete. `verify` normalmente ejecuta el ciclo completo configurado por el proyecto, por lo que puede hacer redundante un `test` separado: mantenlo solo si quieres una señal temprana o etapas de diagnóstico separadas. El artifact permite inspeccionar reportes y transportar el binario.

## Node / Angular

```text
checkout → setup-node → npm ci → lint → test → build → artifact
```

```yaml
steps:
  - uses: actions/checkout@v7
  - uses: actions/setup-node@v7
    with: {node-version: "24", cache: npm}
  - run: npm ci
  - run: npm run lint
  - run: npm test -- --watch=false
  - run: npm run build
  - uses: actions/upload-artifact@v7
    with: {name: frontend-dist, path: dist/}
```

`npm ci` instala exactamente desde el lockfile y falla si no es coherente con `package.json`; por eso es preferible a `npm install` en CI. Fija una versión de Node compatible con el proyecto, conserva el lockfile y usa cache para descargas, no para `node_modules` como artefacto de entrega.

## Cuándo no copiar este flujo

No subas `target/` o `dist/` si no habrá consumidor o diagnóstico; no añadas todos los comandos posibles si el proyecto no los define. Si backend y frontend viven juntos, combina `paths`, `working-directory` y claves de cache por componente.
