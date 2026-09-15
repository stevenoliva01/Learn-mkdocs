---
title: Contenedores y servicios
description: Entorno principal en contenedor y dependencias efímeras de integración.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Contenedores y servicios

`container` define dónde corren los steps del job; `services` inicia dependencias auxiliares, como PostgreSQL. Son distintos: una aplicación de pruebas puede vivir en el container del job y conectarse al service.

```text
Runner Ubuntu
└── Job container: aplicación y tests
    └── Service: PostgreSQL
```

```yaml
jobs:
  integration:
    runs-on: ubuntu-latest
    container: maven:3-eclipse-temurin-21
    services:
      postgres:
        image: postgres:16
        env: {POSTGRES_PASSWORD: postgres, POSTGRES_DB: payments}
        options: >-
          --health-cmd "pg_isready" --health-interval 10s --health-timeout 5s --health-retries 5
    steps:
      - uses: actions/checkout@v7
      - run: mvn -B verify
```

El container aporta un entorno de herramientas repetible; los services aportan infraestructura efímera de prueba. Son apropiados para integration tests, no para reemplazar un environment real ni para guardar estado de producción. Revisa networking, puertos, health checks, imágenes confiables y compatibilidad con el runner antes de adoptar el patrón.
