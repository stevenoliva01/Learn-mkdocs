# Estructura propuesta para `docs/`

La navegacion principal usa Dominio -> Tecnologia -> Concepto. Las colecciones transversales (`cheatsheets`, `runbooks`, `courses`, `certifications`, `inbox`) tienen proposito editorial propio y no duplican las notas de conocimiento.

```text
docs/
├── index.md
├── fundamentals/
│   ├── index.md
│   ├── computing/
│   ├── networking/
│   └── operating-systems/
├── source-control/
│   ├── index.md
│   ├── git/
│   └── platforms/
├── development/
│   ├── index.md
│   ├── backend/java/
│   ├── backend/python/
│   ├── frontend/angular/
│   ├── frontend/javascript/
│   ├── frontend/typescript/
│   ├── mobile/flutter/
│   └── integration/messaging/
├── devops/
│   ├── index.md
│   ├── cicd/github-actions/
│   ├── configuration-management/ansible/
│   ├── service-mesh/istio/
│   └── platform-tooling/
├── cloud/
│   ├── index.md
│   ├── azure/
│   ├── aws/
│   └── oracle-cloud/
├── containers/
│   ├── index.md
│   └── docker/
├── kubernetes/
│   ├── index.md
│   ├── workloads/
│   ├── networking/
│   ├── security/
│   └── delivery/
├── infrastructure-as-code/
│   ├── index.md
│   └── terraform/
├── observability/
│   ├── index.md
│   ├── metrics/
│   ├── logging/
│   └── grafana/
├── security/
│   ├── index.md
│   ├── identity-and-access/
│   ├── certificates/
│   └── supply-chain/
├── databases/
│   ├── index.md
│   ├── fundamentals/sql/
│   ├── oracle/
│   ├── sql-server/
│   └── mysql/
├── middleware/
│   ├── index.md
│   └── oracle/
│       ├── weblogic/
│       ├── forms/
│       └── http-server/
├── artificial-intelligence/
│   ├── index.md
│   ├── fundamentals/
│   ├── machine-learning/
│   ├── llm/
│   └── rag/
├── architecture/
│   ├── index.md
│   ├── software/
│   ├── integration/
│   └── patterns/
├── quality/
│   ├── index.md
│   ├── testing/
│   └── automation/
├── it-management/
│   ├── index.md
│   ├── agile/
│   ├── project-management/
│   ├── service-management/
│   └── governance-and-standards/
├── design/
│   └── index.md
├── cheatsheets/
│   ├── index.md
│   ├── git.md
│   ├── linux.md
│   ├── kubernetes.md
│   └── sql.md
├── runbooks/
│   ├── index.md
│   ├── linux/
│   ├── oracle-middleware/
│   ├── cloud/
│   └── kubernetes/
├── courses/
│   ├── index.md
│   ├── cloud/
│   ├── devops/
│   ├── development/
│   └── artificial-intelligence/
├── certifications/
│   ├── index.md
│   ├── aws/
│   ├── azure/
│   ├── kubernetes/
│   ├── terraform/
│   └── oracle/
├── inbox/
│   └── index.md
└── assets/
    ├── images/
    ├── diagrams/
    └── files/
```

No se deben crear todos los subdirectorios inicialmente. Crear una ruta solo al incorporar su primera pagina aprobada; cada dominio existente tendra `index.md` al activarse. `platform-engineering/` queda deliberadamente fuera hasta que exista contenido claramente clasificable.

### Regla de ubicacion

- Un articulo explicativo vive una sola vez bajo su dominio tecnico; desde cursos o certificaciones se enlaza hacia el.
- Un comando breve y reutilizable va a `cheatsheets/`.
- Un procedimiento reproducible, con precondiciones y validacion, va a `runbooks/`.
- Material lineal aun no sintetizado conserva su procedencia bajo `courses/` o `certifications/`.
- Lo no evaluado entra primero por `inbox/`.
- SQL general vive exclusivamente en `databases/fundamentals/sql/`; cada motor conserva solo contenido especifico de ese motor.
- Oracle WebLogic, Forms y HTTP Server viven en `middleware/oracle/`; los procedimientos reutilizables y sanitizados para estos productos viven en `runbooks/oracle-middleware/`.
- UML queda en `inbox/` hasta decidir, tras revisar su contenido, entre `architecture/modeling/` o una futura ubicacion de ingenieria de software.
