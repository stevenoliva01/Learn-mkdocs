# Estructura actual

## Modelo observado

La fuente esta organizada principalmente por origen o contexto de adquisicion: area amplia, curso, proveedor, certificacion, laboratorio o proyecto. Conviven notas, PDFs, diapositivas, imagenes, codigo, configuracion, repositorios y dependencias instaladas.

```text
TI/
├── Conceptos_Informatica/
├── Control_Source_Management/
│   └── Bitbucket/Curso_Git_Essentials_Bitbucket_Sourcetree_Udemy/
├── Developer/
│   ├── Backend/Java/.../Proyectos/
│   ├── Frontend/Angular_2+|AngularJS|JavaScript|TypeScript/
│   └── Mobile/Flutter/
├── DevOps/
│   ├── AWS/<certificacion o curso>/
│   ├── Azure/<certificacion o curso>/
│   ├── Kubernetes/CKA|CKAD|CKS/
│   ├── Terraform/<examen o libro>/
│   └── Repositorios/<proyectos>/
├── IA/Cursos_Udemy|Machine_Learning/
├── Infra_Middleware/Linux|Windows_Server|Apache|Certificados/
├── Ingenieria_Software/<estilo arquitectonico o disciplina>/
├── Oracle/<producto, curso o caso actual>/
└── Estandares_TI|Gestion_Proyectos|Figma|QA/
```

## Limitaciones para encontrar conocimiento

1. Un mismo concepto puede quedar en varios lugares: por ejemplo, seguridad aparece en cloud, Kubernetes, certificados y Oracle; arquitectura aparece en desarrollo e ingenieria de software.
2. La ruta expresa a menudo la procedencia (`Curso_Udemy`, proveedor, examen o libro), no el tema que alguien buscaria.
3. Los proyectos, binarios, entornos virtuales y resultados de compilacion inflan la jerarquia y no representan conocimiento editorial.
4. No hay una separacion estructural uniforme entre explicaciones, comandos reutilizables, procedimientos operativos y material pendiente de consolidar.
5. Los recursos binarios estan mezclados con notas y codigo; esto dificulta una futura publicacion por GitHub Pages y el control de peso/licencias.

## Activos estructurales que conviene conservar conceptualmente

- La especializacion concreta ya existente: Azure, AWS, Kubernetes, Terraform, Ansible, Grafana, Java, Angular, Oracle, Linux, SQL Server, IA y QA.
- Las certificaciones identificables (AWS, Azure, CKA, CKAD, CKS, Terraform y Oracle) como contexto de estudio, sin convertirlas en el eje de todo el contenido.
- Los laboratorios y casos operativos, pero separados de notas explicativas y de material de curso.
