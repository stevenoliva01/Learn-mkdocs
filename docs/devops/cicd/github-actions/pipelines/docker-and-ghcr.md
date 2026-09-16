---
title: Docker y GHCR
description: Construcción y publicación trazable de imágenes de contenedor.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Docker y GHCR

Publicar una imagen no es solo ejecutar `docker build`: debe identificar el código que contiene y otorgar solamente el permiso de registro requerido.

```text
source → Docker build → tag SHA y release → GHCR
```

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
      context: .
      push: true
      tags: |
        ghcr.io/acme/payments-api:${{ github.sha }}
        ghcr.io/acme/payments-api:1.4.0
```

`contents: read` permite checkout; `packages: write` publica en GHCR. `GITHUB_TOKEN` es temporal y evita almacenar una credencial de usuario para este caso. La Action de build/push usa Buildx para construir y publicar; su contexto determina qué archivos llegan al build, por lo que un `.dockerignore` correcto importa.

El tag SHA ofrece trazabilidad inmutable hacia el commit. Un tag de versión o release sirve al consumo humano. `latest` puede ser una comodidad, nunca la única referencia de despliegue, porque no identifica qué código se está ejecutando. No publiques desde un PR no confiable ni concedas `packages: write` a jobs de validación.

## Práctica recomendada

El [Lab 12 — Docker y GHCR](../labs/lab-12-docker-ghcr.md) publica una imagen Alpine mínima y explica cómo eliminarla.
