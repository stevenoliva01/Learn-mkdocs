---
title: Proyecto final de GitHub Actions
description: Escenario conceptual de CI/CD para una aplicación Java y Angular en Azure.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Proyecto final de GitHub Actions

El escenario integra un backend Spring Boot con Java, un frontend Angular, Docker, Terraform, Azure, AKS, Sonar, Trivy y Gitleaks.

```text
Pull Request
  -> build + test + Sonar + Gitleaks + Trivy + Terraform plan
       |
Merge controlado
  -> imagen Docker/GHCR + promoción por environment
       |
OIDC -> Azure -> AKS
```

El proyecto debe separar CI de CD, publicar artifacts y reportes útiles, etiquetar imágenes para trazabilidad y aplicar permisos mínimos. Terraform valida y genera plan en pull request; `apply` se reserva para un flujo autorizado. Azure se autentica por OIDC, no por secretos de larga duración cuando la federación cubra el caso.

La solución final debe demostrar reutilización, control de environments, calidad, seguridad y una estrategia de recuperación o diagnóstico. No se crean aquí backend, frontend, Terraform ni workflows reales.
