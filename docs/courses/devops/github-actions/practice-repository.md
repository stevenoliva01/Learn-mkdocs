---
title: Repositorio de práctica
description: Estructura recomendada para practicar GitHub Actions.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Repositorio de práctica

El repositorio propuesto se denomina `github-actions-lab/` y organiza documentación, ejercicios y componentes de ejemplo.

```text
github-actions-lab/
├── docs/
├── labs/
├── backend/
├── frontend/
├── terraform/
└── .github/
    ├── actions/
    └── workflows/
```

- `docs/` conserva guías y decisiones del laboratorio.
- `labs/` agrupa ejercicios progresivos.
- `backend/`, `frontend/` y `terraform/` aíslan los dominios que activa cada pipeline.
- `.github/actions/` guarda acciones locales y `.github/workflows/` los workflows.

Esta es una estructura conceptual: no se crea un repositorio de práctica durante esta fase. Cuando el bloque práctico se convierta en un repositorio separado, `github-actions-labs` puede adoptar esta estructura y copiar, uno por uno, los archivos indicados en las [prácticas guiadas](labs/index.md). No copies todos los workflows a la vez: cada laboratorio está diseñado para observar un concepto de forma aislada.
