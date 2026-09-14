# Learn

Learn es una base de conocimiento tecnica personal construida con MkDocs y Material for MkDocs. Su objetivo es conservar conocimiento propio y sintetizado, organizado por dominio, tecnologia y concepto.

## Arquitectura

Las paginas de conocimiento viven en dominios tecnicos. Las colecciones `cheatsheets`, `runbooks`, `courses`, `certifications` e `inbox` tienen propositos editoriales distintos. Consulta `analysis/proposed-structure.md` y `analysis/architecture-decisions.md` para las reglas completas.

`inbox` es una entrada temporal: todo contenido nuevo se clasifica, se revisa por seguridad y copyright, y luego se consolida en una pagina canonica o se conserva en una coleccion editorial.

## Requisitos

- Python 3.13 o compatible.

## Instalacion local

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

## Ejecucion local

```powershell
mkdocs serve
```

Abre `http://127.0.0.1:8000`.

## Build

```powershell
mkdocs build --strict
```

El sitio generado se escribe en `site/`, que no se versiona.

## Estructura de `docs/`

Los dominios principales incluyen fundamentos, desarrollo, DevOps, cloud, contenedores, Kubernetes, infraestructura como codigo, observabilidad, seguridad, bases de datos, middleware, inteligencia artificial, arquitectura, calidad, gestion de TI y diseno. Cada uno tiene una pagina de entrada.

## Contribuir nuevos apuntes

1. Crea o registra el material en `docs/inbox/`.
2. Clasificalo como Knowledge, Cheatsheet, Runbook, Course o Certification.
3. Sintetiza el conocimiento en su ubicacion canonica, sin duplicarlo.
4. No incorpores secretos, datos corporativos, recursos sin licencia compatible, repositorios completos ni artefactos generados.

Los laboratorios ejecutables deben vivir en repositorios separados; Learn solo puede contener su explicacion, arquitectura, comandos, fragmentos pequenos, conclusiones, troubleshooting y enlaces externos.
