---
title: Azure OIDC y AKS
description: Autenticación federada y despliegue conceptual en Kubernetes.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Azure OIDC y AKS

```text
GitHub Actions
     |
     | OIDC
     v
Microsoft Entra ID
     |
     v
Azure
```

OIDC permite obtener una identidad temporal para Azure sin guardar un client secret de larga duración cuando ese modelo resuelve el caso.

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
  - run: kubectl apply -f k8s/
```

Configura federated credentials con claims y restricciones acordes al repositorio, rama o environment, y aplica mínimo privilegio. Para operación empresarial, GitOps con Argo CD o Flux puede separar la promoción del acceso directo al clúster.

Consulta también [permisos y seguridad](../security/permissions-and-security.md).
