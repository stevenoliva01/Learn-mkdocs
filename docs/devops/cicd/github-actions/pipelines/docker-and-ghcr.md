---
title: Docker y GHCR
description: Construcción y publicación trazable de imágenes de contenedor.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Docker y GHCR

GitHub Container Registry (GHCR) permite publicar imágenes del repositorio. El job requiere `packages: write` además del privilegio mínimo de lectura.

```yaml
permissions:
  contents: read
  packages: write

steps:
  - uses: actions/checkout@v7
  - uses: docker/login-action@v3
    with:
      registry: ghcr.io
      username: ${{ github.actor }}
      password: ${{ secrets.GITHUB_TOKEN }}
  - uses: docker/build-push-action@v7
    with:
      push: true
      tags: ghcr.io/org/app:${{ github.sha }}
```

Usa Buildx mediante la acción de build/push. Etiqueta por SHA para trazabilidad y agrega tags de release cuando corresponda. Evita depender solamente de `latest`.
