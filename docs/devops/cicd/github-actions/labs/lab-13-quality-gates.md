---
title: Lab 13 - Quality Gates
description: Hacer que tests obligatorios bloqueen un pipeline.
tags: [GitHub Actions, Laboratorios]
---

# Lab 13 — Quality Gates

## Objetivo

Ver que un control obligatorio detiene el pipeline y corregirlo; no se requiere SonarCloud ni otra cuenta externa. Consulta [Calidad y DevSecOps](../../../../devops/cicd/github-actions/pipelines/quality-and-devsecops.md).

## Archivos a crear

En la raíz crea `package.json`:

```json
{"name":"quality-lab","private":true,"scripts":{"lint":"node --check index.js","test":"node test.js","build":"node --check index.js"}}
```

Crea `index.js` con `module.exports = (a, b) => a + b;` y `test.js`:

```javascript
const add = require("./index");
if (add(2, 2) !== 4) throw new Error("La suma esperada es 4");
console.log("Tests aprobados");
```

Crea `.github/workflows/lab-13.yml`:

```yaml
name: Lab 13 - Quality gate
on: {workflow_dispatch: {}}
jobs:
  build-test-quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - uses: actions/setup-node@v7
        with: {node-version: "24", package-manager-cache: false}
      - run: npm run lint
      - run: npm test
      - run: npm run build
```

## Validación y reto

El run debe mostrar los tres comandos verdes. Cambia el test para comparar contra `5`: `npm test` falla y `build` no se ejecuta. Corrige el test y vuelve a ejecutar. No uses `continue-on-error`: ocultaría el gate. Opcionalmente integra SonarCloud solo después de configurar su proyecto y token fuera del YAML. Elimina los archivos de prueba si no los reutilizarás. Siguiente: [Lab 14](lab-14-azure-oidc.md).
