---
title: Permisos y seguridad
description: Mínimo privilegio, secretos y ejecución segura de workflows.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Permisos y seguridad

`GITHUB_TOKEN` es un token temporal proporcionado al workflow. Declara permisos mínimos por workflow o job.

```yaml
permissions:
  contents: read

jobs:
  azure:
    permissions:
      contents: read
      id-token: write
```

Evita `permissions: write-all`. `id-token: write` habilita OIDC, no acceso genérico a Azure; el proveedor debe validar la identidad federada. Consulta el flujo de [Azure OIDC y AKS](../pipelines/azure-oidc-and-aks.md).

## Código e inputs no confiables

!!! danger
    No ejecutes código no confiable con privilegios elevados.

Los forks, `pull_request_target` e inputs requieren especial atención. No interpolar directamente datos no confiables en shell: pásalos por variables de entorno y valida sus valores. No imprimas secretos, ni siquiera para depurar.

## Dependencias y acciones

Fija actions de terceros por SHA completo cuando el requisito de seguridad lo exija, revisa su procedencia y mantén Dependabot para Actions. Limita la exposición de secretos a los jobs que realmente los necesitan.
