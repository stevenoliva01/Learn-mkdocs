---
title: Lab 14 - Azure OIDC
description: Autenticación federada de solo lectura contra Azure sin client secret.
tags: [GitHub Actions, Laboratorios, Azure]
---

# Lab 14 — Azure OIDC

!!! warning
    **Avanzado y requiere Azure.** Puede implicar administración de identidades y suscripciones. Este laboratorio solo consulta la cuenta; no crea recursos.

## Objetivo y arquitectura

Configurar identidad temporal en vez de guardar un client secret. Lee [Azure OIDC y AKS](../pipelines/azure-oidc-and-aks.md).

`id-token: write` autoriza al job a solicitar un token OIDC; no concede acceso Azure. `identity-check` es el job_id elegido y `azure-lab` es el Environment de ejemplo. `azure/login` intercambia el token por sesión Azure usando los inputs `client-id`, `tenant-id` y `subscription-id`, leídos desde `vars` de repositorio. Los ids no son secretos, pero no deben imprimirse innecesariamente; RBAC y la federated credential siguen decidiendo el acceso.

```text
GitHub Actions → token OIDC → Microsoft Entra ID → federated credential → RBAC → Azure
```

## Preparación externa

En Azure, un administrador debe crear o seleccionar una aplicación o managed identity, añadir una federated credential restringida al repositorio, rama o environment del laboratorio y asignar el rol mínimo de lectura necesario. Registra **client ID**, **tenant ID** y **subscription ID** como variables del repositorio `AZURE_CLIENT_ID`, `AZURE_TENANT_ID` y `AZURE_SUBSCRIPTION_ID`. No son secretos que deban imprimirse; no crees ni guardes un client secret.

## Archivo a crear

```yaml
name: Lab 14 - Azure OIDC
on: {workflow_dispatch: {}}
permissions:
  contents: read
  id-token: write
jobs:
  identity-check:
    name: Verify federated Azure identity
    runs-on: ubuntu-latest
    environment: azure-lab
    steps:
      - uses: azure/login@v3
        with:
          client-id: ${{ vars.AZURE_CLIENT_ID }}
          tenant-id: ${{ vars.AZURE_TENANT_ID }}
          subscription-id: ${{ vars.AZURE_SUBSCRIPTION_ID }}
      - run: az account show --query "{name:name,id:id,tenantId:tenantId}" -o table
```

Guárdalo como `.github/workflows/lab-14.yml`, crea el environment `azure-lab` si lo deseas y ejecútalo manualmente.

## Validación, troubleshooting y cleanup

El job verde muestra solo metadatos de la cuenta. Si `azure/login` falla, compara exactamente issuer, subject, audience, client ID y tenant con la federated credential; `id-token: write` no concede RBAC por sí solo. Al terminar elimina la federated credential, el role assignment y las variables de prueba si no se usarán. Siguiente: [Lab 15](lab-15-terraform.md).
