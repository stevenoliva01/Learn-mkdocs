---
title: Lab 24 — Rulesets y workflows requeridos
description: Explorar controles de merge vigentes sin presentar terminología histórica como funcionalidad actual.
tags: [GitHub Actions, Laboratorios, Security]
---

# Lab 24 — Rulesets y workflows requeridos

!!! warning
    **Opcional / organización / plan dependent.** Realiza esta práctica solo en un repositorio de pruebas con autoridad para cambiar reglas. Consulta [Gobierno de repositorios y enterprise](../advanced/repository-governance-and-enterprise.md).

## Objetivo

Observar cómo una señal de CI participa en la decisión de merge y distinguir required status checks de reglas que requieren workflows al hacer merge.

```text
Pull request → ruleset → required workflow/check → merge allowed or blocked
```

Crea un workflow de CI mínimo que se ejecute en `pull_request`, con un job `validate` que haga una comprobación inocua. Abre un PR de prueba y observa su check. Después, en la configuración de rulesets o protección de rama disponible para tu repositorio/plan, configura la regla correspondiente siguiendo la documentación actual; anota alcance, excepciones y quién puede modificarla. `validate` es un job_id definido por el ejemplo, no una keyword.

Prueba un fallo controlado (`exit 1`) y observa el merge bloqueado; restaura el step y confirma que el check vuelve a pasar. Si usas merge queue, estudia `merge_group` y añade ese evento solo cuando la regla y tu flujo lo requieran. No llames “Required Workflows” a una capacidad histórica sin comprobar la función vigente: registra si configuraste status check, ruleset con workflow requerido o ninguna por limitación de plan. Elimina reglas y PR de prueba si no deben permanecer.
