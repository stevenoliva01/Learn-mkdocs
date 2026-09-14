---
title: Laboratorios de GitHub Actions
description: 35 ejercicios progresivos de GitHub Actions.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Laboratorios de GitHub Actions

Los laboratorios son objetivos de aprendizaje; esta fase no crea workflows ejecutables.

## Nivel 1 — Fundamentos

1. Crear un workflow mínimo con `push`.
2. Ejecutar un step `run` y una action `uses`.
3. Usar `workflow_dispatch`.
4. Recibir inputs manuales tipados.
5. Filtrar por ramas, tags y paths.

## Nivel 2 — CI real

6. Configurar checkout y runner Linux.
7. Construir y probar un proyecto Maven.
8. Usar Java 21 y caché de Maven.
9. Construir, probar y empaquetar un proyecto Node o Angular.
10. Publicar reportes o paquetes como artifacts.

## Nivel 3 — Docker

11. Construir una imagen Docker.
12. Autenticarse contra GHCR.
13. Publicar una imagen con tag por SHA.
14. Añadir tags de release sin depender solo de `latest`.
15. Ejecutar una prueba con un service container PostgreSQL.

## Nivel 4 — Seguridad

16. Declarar `permissions` de mínimo privilegio.
17. Consumir variables y secretos sin exponerlos.
18. Incorporar Gitleaks.
19. Incorporar Trivy.
20. Definir una política de bloqueo para hallazgos.

## Nivel 5 — CD

21. Configurar environment `dev`.
22. Promover mediante environments `pre` y `prod`.
23. Usar variables y secretos específicos por environment.
24. Añadir aprobación o protección cuando esté disponible.
25. Aplicar concurrencia a un despliegue.

## Nivel 6 — Azure

26. Configurar una credencial federada OIDC.
27. Iniciar sesión con `azure/login`.
28. Restringir la identidad por claims y mínimo privilegio.
29. Obtener credenciales de AKS y ejecutar `kubectl`.
30. Comparar despliegue directo con GitOps.

## Nivel 7 — Terraform

31. Ejecutar `terraform fmt` y `validate`.
32. Generar y revisar un `terraform plan` en pull request.
33. Aplicar infraestructura solo desde un flujo controlado.

## Nivel 8 — Plataforma

34. Extraer una composite action o reusable workflow.
35. Diseñar un workflow centralizado con gobierno, seguridad y documentación.
