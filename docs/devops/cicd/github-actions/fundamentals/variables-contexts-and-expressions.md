---
title: Variables, contextos y expresiones
description: Configuración, datos de ejecución y lógica declarativa en workflows.
tags: [GitHub Actions, CI/CD, DevOps]
---

# Variables, contextos y expresiones

Un workflow necesita distinguir configuración estable, secretos y datos de la ejecución. Mezclarlos provoca configuraciones frágiles y, peor aún, secretos expuestos en logs.

| Mecanismo | Origen | Uso correcto |
| --- | --- | --- |
| `env` | YAML del workflow/job/step | Variable de proceso para comandos |
| `vars` | Organización, repositorio o environment | Configuración no sensible |
| `secrets` | GitHub | Valor confidencial y mínimo alcance |
| `inputs` | Evento manual o reusable workflow | Parámetro explícito del contrato |
| Contexts | GitHub y estado del run | Decisiones y metadatos |
| Outputs | Step o job previo | Resultado calculado explícitamente |

## `env`, alcance y precedencia

`env` puede vivir en workflow, job o step. El ámbito más cercano gana para ese step.

```text
workflow env → job env → step env
```

```yaml
env:
  APP_NAME: payments-api
jobs:
  test:
    env:
      JAVA_VERSION: "21"
    steps:
      - env:
          PROFILE: ci
        run: echo "$APP_NAME / $JAVA_VERSION / $PROFILE"
```

`env` no es una bóveda y no transporta valores entre jobs. Para un valor generado usa outputs; para secretos usa `secrets`.

## `vars` y `secrets`

Las variables de configuración pueden definirse en organization, repository o environment; sirven para nombres de región, IDs no sensibles o flags. Los secretos existen en los mismos ámbitos con restricciones y deben exponerse solamente al job que los requiere. Nunca conviertas un secreto en output ni lo imprimas para diagnosticar.

```yaml
environment: production
env:
  REGION: ${{ vars.AZURE_REGION }}
steps:
  - run: ./deploy.sh
    env:
      API_TOKEN: ${{ secrets.DEPLOYMENT_TOKEN }}
```

La [sección de seguridad](../security/permissions-and-security.md) explica por qué un secreto puede estar vacío en un fork y cómo evitar interpolación insegura.

## Contexts: información del run

Las expresiones `${{ }}` leen contexts antes de entregar la instrucción al runner. Los más frecuentes son:

| Context | Ejemplos y propósito |
| --- | --- |
| `github` | `event_name`, `actor`, `repository`, `ref`, `ref_name`, `sha`, `head_ref`, `base_ref` |
| `env`, `vars`, `secrets`, `inputs` | Valores declarados o recibidos |
| `runner`, `job` | Sistema del runner y estado del job |
| `steps` | Outputs de un step con `id` |
| `needs` | Outputs y resultado de jobs requeridos |
| `matrix`, `strategy` | Combinación y estrategia de una matriz |

```yaml
if: ${{ github.event_name == 'push' && github.ref_name == 'main' }}
run: echo "${{ github.repository }} @ ${{ github.sha }} en ${{ runner.os }}"
```

`github.head_ref` y `github.base_ref` son especialmente útiles en `pull_request`; no supongas que contienen un valor significativo en `push`.

## Expresiones frente a variables del shell

`${{ github.sha }}` se calcula por Actions; `$GITHUB_SHA` o `$VARIABLE` los expande el shell. Una condición usa expresiones; un script debe tratar datos no confiables como entrada y entrecomillarlos.

```yaml
if: ${{ startsWith(github.ref, 'refs/tags/v') && success() }}
run: |
  echo "Ref recibida: $GITHUB_REF"
  ./publish.sh "$GITHUB_SHA"
```

Funciones habituales: `success()`, `failure()`, `always()` y `cancelled()` describen estado; `contains()`, `startsWith()` y `endsWith()` comparan texto; `format()`, `fromJSON()` y `toJSON()` transforman datos. `always()` no debe usarse para una operación que no debe ejecutarse al cancelarse un run; piensa qué estado quieres tratar.

!!! warning
    `toJSON()` sirve para diagnosticar un context, pero no lo imprimas indiscriminadamente: puede incluir datos de evento o configuración que no corresponden a los logs.
