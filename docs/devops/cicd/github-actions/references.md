---
title: Referencias de GitHub Actions
description: Fuentes oficiales para ampliar y verificar GitHub Actions.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Referencias de GitHub Actions

Esta guía sintetiza y explica; verifica sintaxis, límites, disponibilidad por plan y comportamiento que puede cambiar en las fuentes oficiales.

## Getting Started, sintaxis y eventos

- [GitHub Actions](https://docs.github.com/actions) explica el modelo general; la [sintaxis de workflows](https://docs.github.com/actions/reference/workflows-and-actions/workflow-syntax) define `on`, jobs, `run-name` e inputs.
- [Eventos](https://docs.github.com/actions/reference/workflows-and-actions/events-that-trigger-workflows), [contexts](https://docs.github.com/actions/learn-github-actions/contexts) y [variables](https://docs.github.com/actions/writing-workflows/choosing-what-your-workflow-does/store-information-in-variables) son la referencia al filtrar y calcular datos.
- [Runners](https://docs.github.com/actions/concepts/runners/about-github-hosted-runners), [self-hosted runners](https://docs.github.com/actions/hosting-your-own-runners) y [ARC](https://docs.github.com/actions/hosting-your-own-runners/managing-self-hosted-runners-with-actions-runner-controller) cubren ejecución y autoscaling.

## Datos, reutilización y custom Actions

- [Artifacts](https://docs.github.com/actions/using-workflows/storing-workflow-data-as-artifacts), [cache](https://docs.github.com/actions/using-workflows/caching-dependencies-to-speed-up-workflows) y [environments](https://docs.github.com/actions/deployment/targeting-different-environments) distinguen persistencia, aceleración y promoción.
- [Reutilizar workflows](https://docs.github.com/actions/sharing-automations/reusing-workflows), [Composite Actions](https://docs.github.com/actions/sharing-automations/creating-actions/creating-a-composite-action), [crear JavaScript Actions](https://docs.github.com/actions/sharing-automations/creating-actions/creating-a-javascript-action), [Docker Actions](https://docs.github.com/actions/sharing-automations/creating-actions/creating-a-docker-container-action) y [workflow templates](https://docs.github.com/actions/sharing-automations/creating-workflow-templates-for-your-organization) delimitan contratos distintos.

## Seguridad, monitoring y gobierno

- [Seguridad de secretos](https://docs.github.com/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions), [OIDC](https://docs.github.com/actions/security-for-github-actions/security-hardening-your-deployments/about-security-hardening-with-openid-connect) y [hardening](https://docs.github.com/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions) son la referencia para privilegio y confianza.
- [Monitor workflows](https://docs.github.com/actions/how-tos/monitor-workflows), [logs](https://docs.github.com/actions/how-tos/monitor-workflows/use-workflow-run-logs), [debug logging](https://docs.github.com/actions/how-tos/monitor-workflows/enable-debug-logging) y [reruns](https://docs.github.com/actions/how-tos/manage-workflow-runs/re-run-workflows-and-jobs) sustentan diagnóstico y operación.
- [Rulesets](https://docs.github.com/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets) y [required workflows](https://docs.github.com/enterprise-cloud@latest/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/available-rules-for-rulesets) se deben consultar para la disponibilidad vigente por plan.

## Marketplace, CLI/API y migración

- [Marketplace](https://github.com/marketplace?type=actions) permite descubrir Actions, no sustituye revisión de proveedor, versión ni permisos.
- [GitHub CLI](https://cli.github.com/manual/gh_api) y [REST API](https://docs.github.com/rest) documentan automatización controlada desde workflows.
- [GitHub Actions Importer](https://docs.github.com/actions/reference/github-actions-importer) y la [migración desde Azure DevOps](https://docs.github.com/actions/tutorials/migrate-to-github-actions/automated-migrations/azure-devops-migration) documentan `audit`, `forecast`, `dry-run` y `migrate`.

## Actions utilizadas en ejemplos

Consulta los repositorios y releases de [checkout](https://github.com/actions/checkout), [setup-java](https://github.com/actions/setup-java), [setup-node](https://github.com/actions/setup-node), [cache](https://github.com/actions/cache), [upload-artifact](https://github.com/actions/upload-artifact), [download-artifact](https://github.com/actions/download-artifact), [login-action](https://github.com/docker/login-action), [build-push-action](https://github.com/docker/build-push-action) y [Azure Login](https://github.com/Azure/login). Las versiones mayor usadas aquí fueron verificadas frente a sus fuentes oficiales en esta ampliación; vuelve a comprobar releases y compatibilidad antes de actualizar un workflow existente.
