---
title: Calidad y DevSecOps
description: Controles de calidad y seguridad como decisiones explícitas de entrega.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Calidad y DevSecOps

Una pipeline segura combina señales distintas; ninguna herramienta por sí sola prueba que un cambio sea correcto o seguro.

```text
Build → unit tests, quality, SAST, SCA, secret scan, container scan, SBOM → gate → deploy
```

| Control | Qué busca | Ejemplos |
| --- | --- | --- |
| Unit tests | Regresiones funcionales | JUnit, Jest |
| Calidad | Mantenibilidad y duplicación | SonarQube, SonarCloud |
| SAST | Patrones vulnerables en código | CodeQL |
| SCA | Riesgos en dependencias | Dependabot, herramientas SCA |
| Secret scan | Credenciales expuestas | Gitleaks |
| Container scan | Riesgos en imagen y SO | Trivy |
| SBOM | Inventario de componentes | formatos SBOM |

Un **warning** informa sin detener el flujo; un **blocking gate** impide promoción. Decide esa política por riesgo, dueño y proceso de excepción, no por la facilidad de hacer verde el pipeline. Publica reportes como artifact o summary cuando ayuden a investigar, minimizando datos sensibles.

!!! danger
    `continue-on-error` en un control crítico convierte un gate en decoración. Úsalo solamente para señales explícitamente no bloqueantes, por ejemplo una combinación experimental.
