---
title: Seguridad en GitHub Actions
description: Permisos, secretos, OIDC y protección de runners.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Seguridad

El mínimo privilegio del `GITHUB_TOKEN`, OIDC, la protección de secretos y el aislamiento de runners son controles básicos de una automatización segura. Lee esta sección antes de dar permisos de publicación o acceso a infraestructura: un workflow que funciona también puede ampliar el radio de impacto de un cambio no confiable.

<div class="grid cards" markdown>

- [**Permisos y seguridad**](permissions-and-security.md): privilegios mínimos, código no confiable, forks y pinning.
- [**Self-hosted runners**](self-hosted-runners.md): mantenimiento, redes, identidades y aislamiento de infraestructura propia.

</div>
