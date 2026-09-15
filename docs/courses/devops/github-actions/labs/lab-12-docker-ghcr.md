---
title: Lab 12 - Docker y GHCR
description: Construir y publicar una imagen trazable en GitHub Container Registry.
tags: [GitHub Actions, Laboratorios]
---

# Lab 12 — Docker y GHCR

## Objetivo y prerrequisitos

Publicar una imagen mínima con `GITHUB_TOKEN`, no con PAT. Necesitas permitir que el repositorio publique packages y revisar [Docker y GHCR](../../../../devops/cicd/github-actions/pipelines/docker-and-ghcr.md).

## Archivos a crear

Crea `Dockerfile` en la raíz:

```dockerfile
FROM alpine:3.22
CMD ["sh", "-c", "echo payments-api lab image"]
```

Crea `.github/workflows/lab-12.yml` y sustituye `OWNER` por tu usuario u organización en minúsculas:

```yaml
name: Lab 12 - Docker GHCR
on: {workflow_dispatch: {}}
permissions:
  contents: read
  packages: write
jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - uses: docker/login-action@v3
        with: {registry: ghcr.io, username: ${{ github.actor }}, password: ${{ secrets.GITHUB_TOKEN }}}
      - uses: docker/build-push-action@v7
        with: {push: true, tags: ghcr.io/OWNER/github-actions-lab:${{ github.sha }}}
```

## Validación, reto y limpieza

Tras el run verde, abre tu perfil u organización → Packages y confirma el tag SHA. Quita `packages: write` para observar el fallo de publicación; restáuralo. Elimina el package desde GitHub Packages si no quieres conservarlo; también puedes eliminar el workflow y Dockerfile. No ejecutes esto desde PRs no confiables. Siguiente: [Lab 13](lab-13-quality-gates.md).
