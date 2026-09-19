---
title: Custom Actions
description: Diseñar, versionar y consumir Actions propias sin confundirlas con workflows o contenedores de jobs.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Custom Actions

Una Action propia existe cuando una integración o regla repetida merece un contrato reutilizable, probado y versionado. Antes de crearla, conserva la opción más simple: un script para lógica local, una Composite Action para steps del caller y un reusable workflow para jobs completos.

| Opción | Comparte | Elige cuando |
| --- | --- | --- |
| Script | Código local | Solo lo usa un repositorio o job. |
| Composite Action | Steps | La operación usa el runner del caller. |
| JavaScript Action | Lógica empaquetada | Necesitas SDKs, manejo de inputs/outputs o portabilidad sin Docker. |
| Docker Container Action | Entorno y lógica | La herramienta requiere una imagen Linux aislada. |
| Reusable Workflow | Jobs y políticas | Debes compartir `needs`, runners o permisos. |

## Anatomía y contrato

La raíz de una Action contiene `action.yml` o `action.yaml`. `name` y `description` explican la interfaz; `author` y `branding` son metadatos opcionales, y `inputs`, `outputs` y `runs` forman el contrato ejecutable. Invócala mediante `uses: owner/repo@ref` o, en el mismo repositorio, una ruta como `./.github/actions/validate-version` tras checkout.

### JavaScript Action mínima

```yaml
# action.yml
name: Validate version
description: Comprueba una versión no vacía y publica su valor
inputs:
  version:
    description: Versión esperada
    required: true
outputs:
  normalized-version:
    description: Valor validado
runs:
  using: node24
  main: dist/index.js
```

```javascript
// src/index.js
const core = require('@actions/core');

try {
  const version = core.getInput('version', { required: true }).trim();
  if (!/^\d+\.\d+\.\d+$/.test(version)) throw new Error('Use semantic version X.Y.Z');
  core.setOutput('normalized-version', version);
  core.notice(`Validated ${version}`);
} catch (error) {
  core.setFailed(error.message);
}
```

`@actions/core` lee inputs, escribe outputs y emite anotaciones; `@actions/github` proporciona un cliente autenticado cuando realmente se necesita llamar a GitHub. `core.setFailed` marca fallo y hace que el proceso tenga una conclusión no exitosa. Un exit code `0` comunica éxito; cualquier exit code distinto de `0` hace fallar el step, salvo que el workflow altere ese comportamiento explícitamente. Empaqueta el código según la estrategia vigente del proyecto (a menudo `dist/` se versiona para que el runner no necesite instalar dependencias); no publiques `node_modules`.

### Docker Container Action

```yaml
# action.yml
name: Print greeting
description: Saluda desde una imagen controlada
inputs:
  name: {description: Nombre, required: true}
runs:
  using: docker
  image: Dockerfile
  args: [${{ inputs.name }}]
```

```dockerfile
FROM alpine:3.22
COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
```

```sh
#!/bin/sh
set -eu
printf 'Hello %s\n' "$1"
```

Una Docker Action es un step empaquetado. No es `container:` (contenedor que ejecuta todos los steps de un job) ni `services:` (contenedores auxiliares como PostgreSQL para un job). Sus restricciones Linux y el coste de construir/obtener una imagen importan al elegirla.

## Versionado, Marketplace y seguridad

Para consumidores internos, un tag mayor como `v1` puede representar una línea compatible; `v1.2.0` identifica una versión exacta legible. Para una Action de terceros o una frontera sensible, fija el SHA completo revisado y anota el release humano correspondiente. Mueve tags mayores con un proceso de release, pruebas y changelog; no cambies silenciosamente el contrato.

Marketplace facilita descubrir y publicar Actions, no certifica exhaustivamente su seguridad. Evalúa editor, releases, mantenimiento, código, documentación, inputs/outputs y permisos antes de usar una Action. Publicar una Action allí requiere un repositorio público y una release etiquetada; no es requisito para reutilizarla dentro de una organización. Los [Labs 19 y 20](../labs/lab-19-javascript-custom-action.md) reproducen Action JavaScript y Docker sin publicar nada.
