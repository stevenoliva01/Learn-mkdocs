---
title: Terraform
description: Validación, plan revisable y aplicación controlada de infraestructura.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Terraform

Terraform cambia infraestructura: una pipeline debe diferenciar propuesta y autorización, no convertir el merge de un PR no confiable en un `apply` automático.

```text
Pull Request → fmt → validate → plan → review → merge → environment/approval → apply
```

```yaml
steps:
  - uses: actions/checkout@v7
  - run: terraform init
  - run: terraform fmt -check
  - run: terraform validate
  - run: terraform plan -out=tfplan
  - uses: actions/upload-artifact@v7
    with: {name: terraform-plan, path: tfplan}
```

`init` prepara providers y backend; `fmt -check` verifica formato; `validate` revisa la configuración; `plan` muestra el cambio propuesto. Conserva el plan como artifact o resumen para revisión, entendiendo que puede contener información sensible según proveedores y configuración. El state y su locking pertenecen al backend de Terraform, no al artifact ni al cache del workflow.

Ejecuta `apply` desde una ref protegida, con environment, concurrencia para el estado compartido e identidad federada como [OIDC](azure-oidc-and-aks.md). Nunca ejecutes apply desde un PR de fork; no uses `continue-on-error` para ocultar `validate` o plan fallidos.
