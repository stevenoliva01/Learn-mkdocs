---
title: Terraform
description: Validación, plan y aplicación controlada de infraestructura.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Terraform

Un pipeline de Terraform separa formato, validación, plan y aplicación.

```text
Pull Request
  -> fmt + validate + plan

Merge o control autorizado
  -> apply
```

```yaml
- run: terraform fmt -check
- run: terraform validate
- run: terraform plan -out=tfplan
```

El `plan` informa el cambio propuesto y puede conservarse como artifact o resumen. La aplicación exige control de rama, environment y aprobación adecuados.

!!! danger
    No realices `apply` automáticamente desde un pull request no confiable.
