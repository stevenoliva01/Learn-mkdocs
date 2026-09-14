---
title: Calidad y DevSecOps
description: Integración de controles de calidad y seguridad en CI.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Calidad y DevSecOps

GitHub Actions puede coordinar controles sin convertir el workflow en documentación de cada herramienta:

- SonarQube o SonarCloud y su Quality Gate.
- SAST y análisis de dependencias o SCA.
- Secret scanning y Gitleaks.
- Trivy para imágenes o dependencias.
- SBOM como inventario de componentes.

Un pipeline debe decidir explícitamente qué hallazgos bloquean el flujo, cuáles generan aviso y quién trata excepciones. Publica reportes como artifacts o resúmenes cuando sea útil; no ignores un control crítico con `continue-on-error`.
