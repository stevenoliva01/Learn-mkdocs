# Mapa de migracion conceptual

> Este mapa usa solo nombres y jerarquia de origen. `Revisar` significa que la clasificacion final requiere inspeccion futura del contenido, seguridad, derechos de distribucion y utilidad; no autoriza ninguna migracion ahora.

| Origen actual | Destino propuesto | Categoria | Accion sugerida |
| --- | --- | --- | --- |
| `Conceptos_Informatica` | `docs/fundamentals/` | Knowledge | Revisar y consolidar por concepto |
| `Control_Source_Management/Bitbucket` | `docs/source-control/platforms/` | Knowledge/Course | Consolidar conceptos; conservar curso aparte si no esta sintetizado |
| `Developer/Backend/Java` | `docs/development/backend/java/` | Knowledge | Migrar selectivamente por concepto |
| `Developer/Backend/SpringBoot` | `docs/development/backend/java/spring-boot/` | Knowledge/Course | Consolidar; separar ejercicios de curso |
| `Developer/Backend/Kafka` | `docs/development/integration/messaging/` | Knowledge | Migrar selectivamente |
| `Developer/Frontend/Angular_2+` | `docs/development/frontend/angular/` | Knowledge | Migrar selectivamente |
| `Developer/Frontend/JavaScript` | `docs/development/frontend/javascript/` | Knowledge | Migrar selectivamente |
| `Developer/Frontend/TypeScript` | `docs/development/frontend/typescript/` | Knowledge | Migrar selectivamente |
| `Developer/Mobile/Flutter` | `docs/development/mobile/flutter/` | Knowledge | Migrar selectivamente |
| `Developer/*/Proyectos` y arboles `build`, `target`, `dist` | fuera de `docs`; repositorio de ejemplos separado | Code/Artifact | No migrar como documentacion; extraer notas reutilizables despues |
| `DevOps/Ansible` | `docs/devops/configuration-management/ansible/` | Knowledge/Course | Consolidar boletines, cursos y laboratorios |
| `DevOps/GitHubActions` | `docs/devops/cicd/github-actions/` | Knowledge | Migrar selectivamente |
| `DevOps/AWS/*Certified*` | `docs/certifications/aws/` y `docs/cloud/aws/` | Certification/Knowledge | Mantener temario en certificaciones; consolidar conceptos en cloud |
| `DevOps/Azure/AZ-*`, `SC-100*` | `docs/certifications/azure/` y `docs/cloud/azure/` | Certification/Knowledge | Mantener objetivo de examen separado de conceptos |
| `DevOps/Azure/Course_*` | `docs/courses/cloud/azure/` | Course | Revisar y sintetizar gradualmente |
| `DevOps/Kubernetes/CKA`, `CKAD`, `CKS` | `docs/certifications/kubernetes/` y `docs/kubernetes/` | Certification/Knowledge | Separar temario/examen de las notas por concepto |
| `DevOps/Kubernetes/ARGOCD` | `docs/kubernetes/delivery/` | Knowledge | Migrar selectivamente |
| `DevOps/Terraform/Exam_Associate_003` | `docs/certifications/terraform/` | Certification | Migrar tras revision |
| `DevOps/Terraform/Libro_Terraform Up & Running` | `docs/courses/devops/terraform/` y `docs/infrastructure-as-code/terraform/` | Course/Knowledge | Sintetizar conceptos; no copiar el libro |
| `DevOps/Grafana` | `docs/observability/grafana/` | Knowledge | Migrar selectivamente |
| `DevOps/Istio`, `Service_Mesh` | `docs/devops/service-mesh/istio/` | Knowledge | Consolidar y deduplicar |
| `DevOps/Repositorios` | fuera de `docs`; repositorios separados | Code/Artifact | Excluir de la migracion documental |
| `IA/Cursos_Udemy/Local_LLM_Ollama_LM_Studio` | `docs/courses/artificial-intelligence/` y `docs/artificial-intelligence/llm/` | Course/Knowledge | Revisar y consolidar |
| `IA/Cursos_Udemy/RAG_LangChain_LangGraph_LangSmith` | `docs/courses/artificial-intelligence/` y `docs/artificial-intelligence/rag/` | Course/Knowledge | Revisar y consolidar |
| `IA/Machine_Learning` | `docs/artificial-intelligence/machine-learning/` | Knowledge | Migrar selectivamente |
| `Infra_Middleware/Linux/Linux_Comandos` | `docs/cheatsheets/linux.md` | Cheatsheet | Curar comandos por tareas |
| `Infra_Middleware/Linux/Linux_Servidores`, `Linux_Shell` | `docs/fundamentals/operating-systems/` y `docs/runbooks/linux/` | Knowledge/Runbook | Separar explicacion de procedimiento |
| `Infra_Middleware/Certificados` | `docs/security/certificates/` | Knowledge/Runbook | Revisar; excluir claves, certificados privados y datos sensibles |
| `Infra_Middleware/Windows_Server` | `docs/fundamentals/operating-systems/` y `docs/runbooks/` | Knowledge/Runbook | Clasificar por tecnologia y operacion |
| `Ingenieria_Software/Arquitectura_*`, `SOA`, `BPM` | `docs/architecture/` | Knowledge | Consolidar por patron o estilo |
| `Estandares_TI/ITIL` | `docs/it-management/service-management/` | Knowledge | Migrar selectivamente |
| `Estandares_TI/ISOS`, `Auditoria` | `docs/it-management/governance-and-standards/` | Knowledge | Migrar selectivamente |
| `Estandares_TI/UML` | `docs/inbox/` | Inbox | Pendiente: decidir entre `architecture/modeling/` o software engineering tras revisar contenido |
| `Gestion_Proyectos/Scrum`, `Kanban`, `Design_Thinking` | `docs/it-management/agile/` | Knowledge | Migrar selectivamente |
| `Gestion_Proyectos/Microsoft_Project`, `Proyectos_TI` | `docs/it-management/project-management/` | Knowledge | Migrar selectivamente |
| `Figma/Prototipado_Interfaces` | `docs/design/` | Knowledge/Course | Revisar y migrar selectivamente |
| `Oracle/Oracle_Cloud_Infrastructure` | `docs/cloud/oracle-cloud/` y `docs/certifications/oracle/` | Knowledge/Certification | Separar producto de examen |
| `Developer/Backend/SQL_Server`, `Developer/Backend/MySQL`, SQL general | `docs/databases/fundamentals/sql/` o el motor correspondiente | Knowledge | Separar SQL general de contenido especifico por motor |
| `Oracle/Oracle_DataBase` | `docs/databases/oracle/` | Knowledge/Runbook | Consolidar por concepto u operacion especifica de Oracle |
| `Oracle/Oracle_Weblogic` | `docs/middleware/oracle/weblogic/` | Knowledge | Migrar solo conocimiento sintetizado |
| `Oracle/Oracle_Forms` | `docs/middleware/oracle/forms/` | Knowledge | Migrar solo conocimiento sintetizado |
| `Oracle/Oracle_Http_Server` | `docs/middleware/oracle/http-server/` | Knowledge | Migrar solo conocimiento sintetizado |
| `Oracle/hoy/*`, `CASOS_SOTE`, procedimientos de middleware | `docs/runbooks/oracle-middleware/` o `docs/inbox/` | Runbook/Inbox | Convertir un caso real en procedimiento generico y sanitizado; nunca publicar el caso original |
| `QA/Selenium` | `docs/quality/automation/` | Knowledge | Migrar selectivamente |
| libros, PDFs, presentaciones, cursos descargados, videos y material propietario | referencia/origen desde Markdown; fuera de `docs` | External resource | No migrar ni redistribuir; revisar licencia antes de incorporar assets |
| `.git`, `.venv`, `site-packages`, `__pycache__`, `target`, `build`, `dist`, `node_modules`, `.tfstate`, `.env`, claves, certificados privados, credenciales y repositorios completos | fuera de `docs` | Security/Artifact | Exclusion estricta: nunca migrar automaticamente |
| archivos sin clasificacion clara | `docs/inbox/` | Inbox | Registrar y triage posterior |
