---
title: Contenedores y servicios
description: Jobs en contenedor y dependencias de servicio efímeras.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Contenedores y servicios

`runs-on` selecciona el runner. `container` ejecuta los steps de un job dentro de una imagen; `services` inicia dependencias auxiliares accesibles durante el job.

```yaml
jobs:
  integration:
    runs-on: ubuntu-latest
    container: maven:3-eclipse-temurin-21
    services:
      postgres:
        image: postgres:16
        env:
          POSTGRES_PASSWORD: postgres
        ports: [5432:5432]
    steps:
      - uses: actions/checkout@v7
      - run: mvn -B verify
```

Un service container es apropiado para pruebas de integración, como PostgreSQL. Un container job define el entorno principal de ejecución. Revisa compatibilidad de herramientas y rutas antes de mover un job a un contenedor.
