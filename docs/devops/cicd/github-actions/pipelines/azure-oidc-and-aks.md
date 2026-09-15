---
title: Azure OIDC y AKS
description: Identidad federada y patrones de despliegue hacia Azure y Kubernetes.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Azure OIDC y AKS

OIDC resuelve el problema de una credencial persistente de Azure guardada en GitHub. El workflow solicita un token de identidad temporal; Microsoft Entra ID lo valida contra una federated credential y Azure RBAC decide qué puede hacer esa identidad.

```text
GitHub Actions → OIDC token → Microsoft Entra ID → Federated credential → Azure RBAC → Azure resource
```

```yaml
permissions:
  contents: read
  id-token: write
steps:
  - uses: azure/login@v3
    with:
      client-id: ${{ vars.AZURE_CLIENT_ID }}
      tenant-id: ${{ vars.AZURE_TENANT_ID }}
      subscription-id: ${{ vars.AZURE_SUBSCRIPTION_ID }}
  - run: az aks get-credentials --resource-group "$RG" --name "$AKS_CLUSTER"
```

`id-token: write` solo permite solicitar el token OIDC; no concede permisos Azure. La federated identity credential debe restringir subject según repositorio, rama o environment y RBAC debe conceder el mínimo privilegio. Client, tenant y subscription IDs no son secretos por sí mismos, pero siguen siendo configuración que se debe gobernar; nunca documentes credenciales reales.

## AKS: directo o GitOps

```text
GitHub Actions → AKS directamente
GitHub Actions → GitOps repo → Argo CD → AKS
```

El acceso directo simplifica un despliegue pequeño, pero el workflow posee acceso al clúster. GitOps separa la promoción: Actions modifica el estado declarado y un controlador del clúster lo reconcilia, con mejor auditoría operacional a costa de otro componente y flujo. No elijas uno por moda; decide dónde debe vivir la autoridad de despliegue. Conceptualmente, OIDC ocupa un papel parecido a una Service Connection de Azure DevOps, aunque el modelo operativo no es idéntico.

## Práctica recomendada

El [Lab 14 — Azure OIDC](../../../../courses/devops/github-actions/labs/lab-14-azure-oidc.md) autentica en modo lectura y enumera el cleanup de identidad federada y RBAC.
