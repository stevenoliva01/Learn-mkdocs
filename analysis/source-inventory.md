# Inventario estructural de la fuente

> Alcance: inventario basado exclusivamente en nombres, extensiones y jerarquía de `C:\Users\Steven\OneDrive\Aprendizaje\TI`. No se abrio, leyo, modifico, movio ni creo ningun archivo dentro de la fuente.

## Resumen cuantitativo

- 12 directorios de primer nivel.
- 7.130 directorios y 37.357 archivos en total.
- La mayor concentracion esta en `IA` (20.223 archivos) y `DevOps` (15.665 archivos); ambos incluyen arboles que aparentan ser dependencias, entornos o repositorios, ademas de material de estudio.

## Directorios principales

| Origen | Directorios descendientes | Archivos descendientes | Senales estructurales |
| --- | ---: | ---: | --- |
| `Conceptos_Informatica` | 0 | 20 | archivos `.txt` en la raiz |
| `Control_Source_Management` | 3 | 13 | Bitbucket y curso Udemy |
| `Developer` | 255 | 445 | backend, frontend, mobile, lenguajes y proyectos Java |
| `DevOps` | 3.934 | 15.665 | cloud, Kubernetes, Terraform, Ansible, GitHub Actions, repositorios |
| `Estandares_TI` | 7 | 44 | auditoria, ISO, ITIL y UML |
| `Figma` | 3 | 4 | prototipado e imagenes/recursos |
| `Gestion_Proyectos` | 10 | 90 | Design Thinking, Kanban, Project, Scrum |
| `IA` | 2.737 | 20.223 | cursos Udemy, RAG/LLM y machine learning |
| `Infra_Middleware` | 24 | 344 | Linux, Windows Server, Apache, certificados |
| `Ingenieria_Software` | 14 | 98 | arquitectura, BPM y SOA |
| `Oracle` | 130 | 409 | base de datos, OCI, Forms, WebLogic y casos operativos |
| `QA` | 1 | 2 | Selenium |

## Extensiones predominantes

| Extension | Cantidad | Tratamiento inicial propuesto |
| --- | ---: | --- |
| `.py`, `.pyi`, `.pyc`, `.dat`, `.typed` | 20.000+ | revisar como entornos/dependencias o laboratorios; no tratar como notas por defecto |
| sin extension | 6.865 | clasificar manualmente antes de migrar |
| `.yml`, `.yaml`, `.tf`, `.tfvars`, `.tpl`, `.hcl` | 4.000+ | laboratorios, ejemplos IaC o configuracion; filtrar secretos y archivos generados |
| `.md` | 1.549 | candidatos a consolidacion, sujetos a revision de contenido y licencia |
| `.txt` | 651 | candidatos a convertir/consolidar, sujetos a revision posterior |
| `.png`, `.jpg`, `.jpeg`, `.svg` | 1.051+ | recursos visuales; mantener separados de las paginas |
| `.pdf`, `.doc`, `.docx`, `.ppt`, `.pptx`, `.xlsx`, `.vsdx`, `.mpp` | 450+ | recursos de referencia; enlazar o conservar fuera de `docs` segun licencia y tamano |
| `.java`, `.js`, `.ts`, `.sql`, `.sh` | 700+ | ejemplos, laboratorios o codigo de proyectos; no migrar ciegamente como documentacion |

## Jerarquia relevante detectada

- `Developer`: `Backend` (Java, Spring Boot, Kafka, Maven, Gradle, SQL Server, Python, microservicios), `Frontend` (Angular, AngularJS, JavaScript, TypeScript, Bootstrap), `Mobile/Flutter`, expresiones regulares y programacion general.
- `DevOps`: Ansible, AWS, Azure, GitHub Actions, Grafana, Istio, Kubernetes (incluye CKA, CKAD y CKS), Terraform y `Repositorios`.
- `IA`: cursos `Local_LLM_Ollama_LM_Studio`, `RAG_LangChain_LangGraph_LangSmith` y fundamentos de machine learning.
- `Infra_Middleware`: Linux (comandos, shell, servidores y cursos IBM), Windows Server, Apache y certificados.
- `Oracle`: OCI, Database (Data Guard/RAC), Forms, WebLogic y una rama `hoy` orientada a casos de trabajo.

## Exclusiones que requeriran tratamiento especial

Los nombres revelan proyectos y artefactos no apropiados para una base publicada de MkDocs: `.git`, `.idea`, `.venv`, `site-packages`, `__pycache__`, `target`, `build`, `dist`, `__MACOSX` y arboles bajo `Repositorios`. Tambien hay extensiones que pueden contener credenciales o estado (`.key`, `.crt`, `.tfvars`, `.tfstate`, `.pem`, `.env`-like). Su inclusion debe quedar prohibida hasta una revision explicita de seguridad y licencia.
