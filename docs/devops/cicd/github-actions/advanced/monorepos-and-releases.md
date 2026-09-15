---
title: Monorepos y releases
description: Ejecución selectiva, directorios de trabajo, tags y trazabilidad de releases.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Monorepos y releases

Un monorepo necesita evitar que un cambio de frontend ejecute Terraform innecesariamente, sin omitir validaciones de archivos compartidos.

```text
repo/ → backend/ | frontend/ | terraform/
```

```yaml
on:
  pull_request:
    paths: ["backend/**", ".github/workflows/backend.yml"]
```

Define workflows por componente con `paths`, `working-directory` y caches basadas en el lockfile de cada componente. Incluye archivos compartidos que sí afecten el resultado. Los filtros reducen costo y tiempo, pero una refactorización transversal puede requerir una validación integrada adicional.

## Releases

```text
commit → tag v1.4.0 → release → Docker/package
```

Semantic Versioning usa `MAJOR.MINOR.PATCH` para comunicar compatibilidad a alto nivel. Un tag `v*` puede disparar `push`; una GitHub Release publicada puede disparar `release`. Decide cuál representa el contrato de publicación. Publica imágenes con SHA y versión; evita desplegar solo `latest` porque no ofrece trazabilidad.
