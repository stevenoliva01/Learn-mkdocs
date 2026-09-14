# Decisiones de arquitectura

## ADR-001: taxonomia orientada al conocimiento

**Decision.** La ubicacion canonica de una pagina se define por dominio, tecnologia y concepto, no por curso, proveedor ni tipo de archivo de origen.

**Consecuencia.** Una nota de Kubernetes sobre redes vive en `kubernetes/networking/`; CKA puede enlazarla desde `certifications/kubernetes/` sin duplicarla.

## ADR-002: colecciones con proposito editorial

| Coleccion | Contiene | No contiene |
| --- | --- | --- |
| Knowledge (dominios) | explicaciones duraderas, decisiones, modelos mentales | temarios lineales o listados de comandos sin contexto |
| `cheatsheets/` | referencias breves, comandos, tablas de consulta | procedimientos largos o secretos |
| `runbooks/` | objetivo, precondiciones, pasos, validacion, reversa y diagnostico | apuntes de curso sin adaptar |
| `courses/` | material aun no consolidado y su procedencia | la fuente completa sin curacion |
| `certifications/` | objetivos, plan, simulacros y trazabilidad de examen | duplicados de conceptos canonicos |
| `inbox/` | entrada temporal sin clasificar | destino permanente |

## ADR-003: convenciones de rutas y archivos

- Rutas, nombres de directorio y archivos: minusculas y `kebab-case`, sin espacios ni tildes.
- Cada dominio activado tiene `index.md`; no se crean directorios vacios de forma preventiva.
- Profundidad habitual: hasta dominio/tecnologia/concepto; un cuarto nivel solo cuando evita mezclar conceptos incompatibles.
- Usar titulos legibles en el encabezado Markdown, aunque la ruta sea normalizada.
- Una pagina debe tener una unica ubicacion canonica. Usar enlaces cruzados y etiquetas, no copias.

## ADR-004: recursos, copyright y contenido ejecutable

- Learn-mkdocs contiene principalmente conocimiento creado o sintetizado por el autor. Las paginas pueden citar o registrar el origen, pero no copiar material de terceros completo.
- No migrar ni redistribuir libros, PDFs de cursos, presentaciones de terceros, descargas de Udemy u otros proveedores, documentacion propietaria, videos ni material sin derechos de redistribucion claros. Permanecen en su ubicacion original.
- Imagenes: junto al dominio bajo `assets/images/<dominio>/` o, cuando solo sirvan a una pagina, en un subdirectorio `assets/` local. Diagramas fuente bajo `assets/diagrams/`. Solo se incorporan assets propios o con licencia compatible.
- PDFs y otros adjuntos publicables solo se alojan en `assets/files/` tras validar licencia, sensibilidad y tamano. Enlazar antes que duplicar.
- Codigo de ejemplo pequeno puede vivir en bloques Markdown o en un repositorio de ejemplos. Proyectos completos, binarios, caches y dependencias no van a `docs/`.
- Nunca versionar `.git`, `.venv`, `site-packages`, `__pycache__`, `target`, `build`, `dist`, `node_modules`, `.tfstate`, `.tfvars` sensibles, `.env`, claves `.key`/`.pem`, certificados privados, credenciales, secretos, artefactos generados ni repositorios completos.

## ADR-005: preparacion para publicacion y automatizacion

- La base usa MkDocs con Material for MkDocs y queda preparada para publicarse posteriormente en GitHub Pages.
- Antes de publicar, definir licencia, politica de recursos externos, revision de secretos y alcance de visibilidad de runbooks.
- La futura automatizacion de GitHub Actions debe validar enlaces, formato Markdown, rutas en kebab-case, referencias de assets y construccion de MkDocs; el despliegue debe ocurrir solo tras esas validaciones.

## ADR-006: proceso de ingreso futuro

1. Todo material nuevo entra en `inbox/` con origen y fecha.
2. Se clasifica como Knowledge, Cheatsheet, Runbook, Course o Certification.
3. Se verifica seguridad, derechos de distribucion, duplicados y vigencia.
4. Se sintetiza hacia una pagina canonica o se conserva temporalmente en la coleccion correspondiente.
5. Se enlaza desde cursos/certificaciones a la nota canonica y se revisa periodicamente el contenido temporal.

## ADR-007: it management, middleware y SQL

- `it-management/` sustituye completamente a `ways-of-working/`: agilidad, gestion de proyectos, gestion de servicios y gobierno/estandares son sus cuatro areas iniciales.
- UML no entra en `it-management/`; queda en `inbox/` hasta elegir, con revision de contenido, entre `architecture/modeling/` y una ubicacion de ingenieria de software.
- Oracle WebLogic, Forms y HTTP Server se tratan como middleware bajo `middleware/oracle/`, no como una subdivision generica de desarrollo.
- Los runbooks reutilizables y sanitizados de dichos productos se ubican bajo `runbooks/oracle-middleware/`.
- SQL general tiene un unico hogar: `databases/fundamentals/sql/`. `databases/oracle/`, `databases/sql-server/` y `databases/mysql/` contienen exclusivamente contenido especifico de cada motor.
- `platform-engineering/` no se crea hasta contar con contenido claramente clasificable.

## ADR-008: saneamiento de runbooks y casos reales

- Un runbook publicable debe ser generico, reutilizable, sanitizado y verificable.
- No migrar directamente nombres de clientes o empresas, tenants, subscriptions, resource IDs, IPs, hostnames, URLs internas, credenciales, secretos, tokens, certificados ni configuraciones sensibles.
- Los casos originales de trabajo permanecen fuera de la documentacion publicable. Solo se puede incorporar una version generica creada a partir de ellos tras eliminar datos corporativos y validar que no revele contexto sensible.

## ADR-009: repositorios, laboratorios y runbooks publicables

- Proyectos completos, repositorios y laboratorios con codigo no se migran a `docs/` ni se importan automaticamente como arboles completos.
- Learn-mkdocs puede documentar la explicacion, arquitectura, comandos, fragmentos pequenos de codigo, conclusiones, troubleshooting y enlaces a repositorios externos de un laboratorio.
- Cuando un laboratorio tenga valor como proyecto ejecutable, debe permanecer o crearse en un repositorio separado.
- Un runbook publicable debe ser generico, reutilizable, sanitizado, verificable y libre de informacion corporativa o sensible. Los casos reales originales permanecen fuera del repositorio.

## ADR-010: UX y navegacion

- La navegacion es hibrida: Material gestiona automaticamente las paginas dentro de cada dominio y `mkdocs-awesome-pages-plugin` ordena, agrupa y da nombres visuales solo a los dominios de primer nivel mediante `docs/.pages`. Asi escala a cientos de paginas sin un `nav` manual por pagina.
- Los nombres visibles se definen en `docs/.pages`; las rutas fisicas normalizadas permanecen estables. Por ejemplo, `artificial-intelligence/` se presenta como `AI` e `infrastructure-as-code/` como `Infrastructure as Code`.
- El sidebar agrupa los dominios en Core, Engineering, Reference, Learning, Management y Workspace. La homepage ofrece accesos visuales complementarios hacia indices existentes; no duplica la taxonomia. La navegacion Dominio -> Tecnologia -> Concepto se conserva dentro de cada dominio.
- Material habilita `navigation.sections`, `navigation.indexes`, `navigation.path`, `navigation.top` y `navigation.tracking`. No se usan tabs para evitar una barra superior saturada, ni expansion global para evitar un sidebar excesivamente largo cuando crezca el contenido.
- La busqueda integrada de MkDocs/Material es la unica buscadora inicial; indexa titulos, encabezados y contenido Markdown sin servicios externos.
- La homepage usa grids/cards, botones e iconos nativos de Material. No se agrega CSS personalizado mientras estas capacidades cubran la necesidad; no se modifican breakpoints ni el comportamiento responsive nativo.
- El navigation drawer es el patron responsive normal de Material en tablet y movil. En desktop, su estado depende de la accion del usuario y no de la configuracion actual; no se aplican hacks CSS sin una reproduccion concreta de un fallo.
- `mkdocs-awesome-pages-plugin==2.10.1` es el unico plugin adicional de UX: resuelve la agrupacion y etiquetas visibles sin listas manuales de paginas. Los plugins futuros requieren una necesidad que no cubran MkDocs o Material, compatibilidad verificada y una mejora duradera.

## ADR-011: landing pages y actualización documental

- Los dominios o tecnologías importantes tienen `index.md`.
- Las agrupaciones internas relevantes pueden tener su propio `index.md`.
- Una categoría del menú debe llevar primero a su landing page y no arbitrariamente al primer artículo.
- Los índices principales de tecnologías muestran una fecha visible de última actualización.
- Por ahora la fecha se mantiene manualmente.
- En el futuro puede automatizarse mediante información de Git si aporta valor.
- Los enlaces internos deben ser relativos y compatibles con MkDocs y GitHub Pages.
- Se evitan enlaces construidos manualmente con paths absolutos del sitio.

## Aprobaciones necesarias antes de migrar

1. Definir, tras revisar su contenido, si UML se ubica en `architecture/modeling/` o en una futura rama de ingenieria de software.
2. Definir la visibilidad futura de los runbooks sanitizados; los casos originales de trabajo quedan excluidos de publicacion.
