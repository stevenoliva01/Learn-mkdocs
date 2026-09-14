---
title: Monorepos y releases
description: Ejecución selectiva, etiquetas y versionado.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Monorepos y releases

En un monorepo, los filtros por `paths` activan solo el pipeline afectado.

```yaml
on:
  push:
    paths: ["backend/**", ".github/workflows/backend.yml"]
```

Combina rutas, directorios de trabajo y cachés por aplicación para reducir ejecuciones innecesarias. Los tags pueden iniciar una release; Semantic Versioning ofrece un lenguaje común para las versiones. Usa tags de release y SHA en las imágenes para trazabilidad.
