---
title: Lab 15 - Terraform
description: Validar configuración Terraform sin aplicar infraestructura por defecto.
tags: [GitHub Actions, Laboratorios, Terraform]
---

# Lab 15 — Terraform

!!! warning
    **Avanzado.** Requiere Terraform CLI para validar localmente. El flujo base no crea recursos; una integración cloud/OIDC es opcional.

## Objetivo

Separar formato, inicialización, validación y plan de `apply`. Lee [Terraform](../../../../devops/cicd/github-actions/pipelines/terraform.md).

## Archivos a crear

Crea `terraform/main.tf`:

```hcl
terraform {
  required_version = ">= 1.6.0"
}
```

Crea `.github/workflows/lab-15.yml`:

```yaml
name: Lab 15 - Terraform validation
on: {workflow_dispatch: {}}
jobs:
  validate:
    runs-on: ubuntu-latest
    defaults: {run: {working-directory: terraform}}
    steps:
      - uses: actions/checkout@v7
      - uses: hashicorp/setup-terraform@v3
      - run: terraform fmt -check
      - run: terraform init -backend=false
      - run: terraform validate
      - run: terraform plan -input=false
```

## Ejecutar, validar y ampliar

Ejecuta localmente `terraform fmt -check`, `terraform init -backend=false`, `terraform validate` y `terraform plan`; después publica el workflow. Debes ver los cuatro pasos verdes y un plan sin cambios. Rompe una llave de HCL para observar `validate` fallido y corrígela.

Una variante avanzada puede usar Azure + OIDC para `plan` contra un backend real, pero `apply` debe ser manual, desde una rama protegida, con environment/aprobación, concurrencia para state y cleanup explícito de los recursos creados. El laboratorio base no tiene cleanup cloud. Sigue con [Lab 16](lab-16-complete-pipeline.md).
