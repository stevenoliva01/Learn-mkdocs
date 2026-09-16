---
title: Ejercicios adicionales de GitHub Actions
description: Retos agrupados para ampliar la práctica después de los laboratorios guiados.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Ejercicios adicionales de GitHub Actions

Esta colección propone retos para ampliar la práctica libre; no son laboratorios guiados ni sustituyen los [16 laboratorios reproducibles](index.md). Para cada laboratorio encontrarás YAML completo, resultado observable y troubleshooting en su página individual.

## Índice de ejercicios

1. [Ejercicio 01 — Workflow mínimo](#ejercicio-01)
2. [Ejercicio 02 — `run` y `uses`](#ejercicio-02)
3. [Ejercicio 03 — Ejecución manual](#ejercicio-03)
4. [Ejercicio 04 — Inputs tipados](#ejercicio-04)
5. [Ejercicio 05 — Filtros de eventos](#ejercicio-05)
6. [Ejercicio 06 — Checkout y runner](#ejercicio-06)
7. [Ejercicio 07 — Maven](#ejercicio-07)
8. [Ejercicio 08 — Java y caché](#ejercicio-08)
9. [Ejercicio 09 — Node o Angular](#ejercicio-09)
10. [Ejercicio 10 — Artifacts](#ejercicio-10)
11. [Ejercicio 11 — Imagen Docker](#ejercicio-11)
12. [Ejercicio 12 — Autenticación en GHCR](#ejercicio-12)
13. [Ejercicio 13 — Tag por SHA](#ejercicio-13)
14. [Ejercicio 14 — Tags de release](#ejercicio-14)
15. [Ejercicio 15 — Service container](#ejercicio-15)
16. [Ejercicio 16 — Mínimo privilegio](#ejercicio-16)
17. [Ejercicio 17 — Variables y secretos](#ejercicio-17)
18. [Ejercicio 18 — Gitleaks](#ejercicio-18)
19. [Ejercicio 19 — Trivy](#ejercicio-19)
20. [Ejercicio 20 — Política de bloqueo](#ejercicio-20)
21. [Ejercicio 21 — Environment de desarrollo](#ejercicio-21)
22. [Ejercicio 22 — Promoción por entornos](#ejercicio-22)
23. [Ejercicio 23 — Configuración por entorno](#ejercicio-23)
24. [Ejercicio 24 — Protección de entornos](#ejercicio-24)
25. [Ejercicio 25 — Concurrencia](#ejercicio-25)
26. [Ejercicio 26 — Credencial federada OIDC](#ejercicio-26)
27. [Ejercicio 27 — Inicio de sesión en Azure](#ejercicio-27)
28. [Ejercicio 28 — Claims y mínimo privilegio](#ejercicio-28)
29. [Ejercicio 29 — AKS y kubectl](#ejercicio-29)
30. [Ejercicio 30 — GitOps](#ejercicio-30)
31. [Ejercicio 31 — Terraform fmt y validate](#ejercicio-31)
32. [Ejercicio 32 — Terraform plan](#ejercicio-32)
33. [Ejercicio 33 — Terraform apply controlado](#ejercicio-33)
34. [Ejercicio 34 — Reutilización](#ejercicio-34)
35. [Ejercicio 35 — Workflow centralizado](#ejercicio-35)

## Nivel 1 — Fundamentos

## Ejercicio 01 — Workflow mínimo {#ejercicio-01}

Crear un workflow mínimo con `push`.

## Ejercicio 02 — `run` y `uses` {#ejercicio-02}

Ejecutar un step `run` y una action `uses`.

## Ejercicio 03 — Ejecución manual {#ejercicio-03}

Usar `workflow_dispatch`.

## Ejercicio 04 — Inputs tipados {#ejercicio-04}

Recibir inputs manuales tipados.

## Ejercicio 05 — Filtros de eventos {#ejercicio-05}

Filtrar por ramas, tags y paths.

## Nivel 2 — CI real

## Ejercicio 06 — Checkout y runner {#ejercicio-06}

Configurar checkout y runner Linux.

## Ejercicio 07 — Maven {#ejercicio-07}

Construir y probar un proyecto Maven.

## Ejercicio 08 — Java y caché {#ejercicio-08}

Usar Java 21 y caché de Maven.

## Ejercicio 09 — Node o Angular {#ejercicio-09}

Construir, probar y empaquetar un proyecto Node o Angular.

## Ejercicio 10 — Artifacts {#ejercicio-10}

Publicar reportes o paquetes como artifacts.

## Nivel 3 — Docker

## Ejercicio 11 — Imagen Docker {#ejercicio-11}

Construir una imagen Docker.

## Ejercicio 12 — Autenticación en GHCR {#ejercicio-12}

Autenticarse contra GHCR.

## Ejercicio 13 — Tag por SHA {#ejercicio-13}

Publicar una imagen con tag por SHA.

## Ejercicio 14 — Tags de release {#ejercicio-14}

Añadir tags de release sin depender solo de `latest`.

## Ejercicio 15 — Service container {#ejercicio-15}

Ejecutar una prueba con un service container PostgreSQL.

## Nivel 4 — Seguridad

## Ejercicio 16 — Mínimo privilegio {#ejercicio-16}

Declarar `permissions` de mínimo privilegio.

## Ejercicio 17 — Variables y secretos {#ejercicio-17}

Consumir variables y secretos sin exponerlos.

## Ejercicio 18 — Gitleaks {#ejercicio-18}

Incorporar Gitleaks.

## Ejercicio 19 — Trivy {#ejercicio-19}

Incorporar Trivy.

## Ejercicio 20 — Política de bloqueo {#ejercicio-20}

Definir una política de bloqueo para hallazgos.

## Nivel 5 — CD

## Ejercicio 21 — Environment de desarrollo {#ejercicio-21}

Configurar environment `dev`.

## Ejercicio 22 — Promoción por entornos {#ejercicio-22}

Promover mediante environments `pre` y `prod`.

## Ejercicio 23 — Configuración por entorno {#ejercicio-23}

Usar variables y secretos específicos por environment.

## Ejercicio 24 — Protección de entornos {#ejercicio-24}

Añadir aprobación o protección cuando esté disponible.

## Ejercicio 25 — Concurrencia {#ejercicio-25}

Aplicar concurrencia a un despliegue.

## Nivel 6 — Azure

## Ejercicio 26 — Credencial federada OIDC {#ejercicio-26}

Configurar una credencial federada OIDC.

## Ejercicio 27 — Inicio de sesión en Azure {#ejercicio-27}

Iniciar sesión con `azure/login`.

## Ejercicio 28 — Claims y mínimo privilegio {#ejercicio-28}

Restringir la identidad por claims y mínimo privilegio.

## Ejercicio 29 — AKS y kubectl {#ejercicio-29}

Obtener credenciales de AKS y ejecutar `kubectl`.

## Ejercicio 30 — GitOps {#ejercicio-30}

Comparar despliegue directo con GitOps.

## Nivel 7 — Terraform

## Ejercicio 31 — Terraform fmt y validate {#ejercicio-31}

Ejecutar `terraform fmt` y `validate`.

## Ejercicio 32 — Terraform plan {#ejercicio-32}

Generar y revisar un `terraform plan` en pull request.

## Ejercicio 33 — Terraform apply controlado {#ejercicio-33}

Aplicar infraestructura solo desde un flujo controlado.

## Nivel 8 — Plataforma

## Ejercicio 34 — Reutilización {#ejercicio-34}

Extraer una composite action o reusable workflow.

## Ejercicio 35 — Workflow centralizado {#ejercicio-35}

Diseñar un workflow centralizado con gobierno, seguridad y documentación.
