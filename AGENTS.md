# AGENTS.md

## Project purpose

`Learn-mkdocs` is a personal technical knowledge base built with MkDocs Material.

The repository consolidates historical technical notes into structured,
maintainable and searchable documentation.

The canonical information architecture is:

`Domain -> Technology -> Concept`

Do not organize canonical knowledge primarily by:

- course
- provider
- book
- certification
- original file location

The goal is to transform historical notes into a curated knowledge base,
not to reproduce the historical folder structure.

---

## Source of truth

Before making structural or editorial decisions, read:

- `analysis/architecture-decisions.md`
- `analysis/proposed-structure.md`
- `analysis/migration-map.md`

These documents define the approved architecture and migration rules.

Do not redesign the repository architecture unless explicitly requested.

If a task conflicts with an existing ADR, report the conflict before changing
the architecture.

---

## Historical source

Historical notes are stored outside this repository under:

`C:\Users\Steven\OneDrive\Aprendizaje\TI`

Treat this location as external source material and READ ONLY.

Rules:

- Never modify files under `Aprendizaje\TI`.
- Never move historical files.
- Never rename historical files.
- Never delete historical files.
- Never overwrite historical files.
- Read only files or folders explicitly authorized in the current task.
- Do not recursively inspect all of `Aprendizaje\TI` unless explicitly requested.
- Do not expand the authorized scope to adjacent technologies.
- Do not copy historical content blindly into `docs/`.

Migration means:

1. read the authorized source
2. curate
3. consolidate
4. normalize
5. remove duplication
6. classify
7. restructure
8. validate
9. publish only approved knowledge

---

## Documentation model

Content belongs to one of these editorial categories.

### Knowledge

Canonical explanatory documentation under technical domains.

Examples:

- `docs/devops/`
- `docs/cloud/`
- `docs/kubernetes/`
- `docs/databases/`
- `docs/security/`
- `docs/observability/`

A concept must have one canonical location.

Use links instead of duplicating complete explanations.

---

### Cheatsheets

Location:

`docs/cheatsheets/`

Contains concise material for quick consultation:

- commands
- syntax
- small snippets
- lookup tables
- common expressions
- frequently used references

Do not put long explanations or complete tutorials here.

---

### Runbooks

Location:

`docs/runbooks/`

Contains reproducible operational procedures.

A runbook should normally include:

- objective
- prerequisites
- procedure
- validation
- rollback or recovery when applicable
- troubleshooting

Runbooks must be:

- generic
- reusable
- sanitized
- free of confidential corporate information

Do not migrate real incidents directly.

---

### Courses

Location:

`docs/courses/`

Contains:

- learning paths
- labs
- exercises
- practical projects
- study sequences
- material not yet consolidated into canonical Knowledge

Do not duplicate complete technical explanations already present in Knowledge.

---

### Certifications

Location:

`docs/certifications/`

Contains:

- exam objectives
- study plans
- certification roadmaps
- exam mappings
- certification-specific notes

Canonical technical concepts remain in their technical domain.

---

### Inbox

Location:

`docs/inbox/`

Temporary location for material that cannot yet be classified safely.

`inbox/` is not a permanent destination.

---

## Naming conventions

Use:

- lowercase paths
- kebab-case
- no spaces
- no accents in file or directory names
- descriptive names
- `index.md` for domain and category landing pages

Good examples:

`github-actions`

`infrastructure-as-code`

`private-endpoint.md`

`variables-contexts-and-expressions.md`

Avoid:

`GitHub_Actions`

`01-Private-Endpoint.md`

`Azure Network Notes.md`

`Curso_Udemy_Azure.md`

Visible Markdown titles may use normal capitalization, spaces and accents.

---

## Landing pages

Important domains, technologies and meaningful internal categories should have
an `index.md`.

Examples:

`docs/devops/index.md`

`docs/devops/cicd/index.md`

`docs/devops/cicd/github-actions/index.md`

`docs/devops/cicd/github-actions/fundamentals/index.md`

A navigation category should normally open its landing page before an
individual article.

Landing pages should:

- explain the scope briefly
- list or link to real child pages
- avoid duplicating full technical content
- use Material cards when useful
- avoid links to pages that do not exist

---

## Documentation update date

Important technology landing pages should show a visible update date.

Current convention:

```markdown
**Última actualización:** 14 de septiembre de 2026
```

For now, the date is maintained manually.

Do not add manual dates to every individual article unless explicitly requested.

The main technology landing page is the primary place for this metadata.

In the future, Git metadata may be used to automate update dates if approved.

---

## Internal links

Use links compatible with MkDocs and GitHub Pages.

Prefer relative Markdown links.

Do not create absolute site-root links unless explicitly required.

Avoid links that resolve incorrectly, for example:

`/fundamentals/concepts-and-syntax.md`

when the real page belongs to:

`docs/devops/cicd/github-actions/fundamentals/concepts-and-syntax.md`

A category card should normally link to its `index.md`, not arbitrarily to its
first article.

After changing documentation or navigation, validate the relevant URLs with
`mkdocs serve --strict`.

---

## Markdown conventions

Use YAML front matter for technical pages when appropriate:

```yaml
---
title: Page title
description: Short description
tags:
  - Example
---
```

Use:

- one H1 per page
- H2/H3 for structure
- fenced code blocks with language identifiers
- Material admonitions only when useful
- tables when comparison is clearer than prose
- Mermaid for diagrams when appropriate
- descriptive internal links
- short introductions before detailed sections

Do not overuse:

- admonitions
- icons
- decorative formatting
- excessive nested headings

---

## Code blocks

Always use a language identifier when possible.

YAML:

```yaml
name: CI

on:
  push:
    branches:
      - main
```

Bash:

```bash
echo "hello"
```

PowerShell:

```powershell
Get-ChildItem
```

Plain structures and diagrams:

```text
Workflow
  |
  +-- Job
      |
      +-- Step
```

Do not execute historical workflow examples unless the current task explicitly
requires execution.

---

## MkDocs navigation

The project uses:

- MkDocs
- Material for MkDocs
- `mkdocs-awesome-pages-plugin`

Do not create a large manual global `nav:` in `mkdocs.yml`.

Use local `.pages` files for:

- ordering
- grouping
- display names
- local navigation behavior

Prefer local navigation changes close to the affected documentation.

Do not modify:

`docs/.pages`

globally unless the task explicitly requires a root navigation change.

---

## Current root navigation model

The main navigation is conceptually grouped as:

### Core

- Fundamentals
- Development
- Source Control
- Architecture

### Engineering

- DevOps
- Cloud
- Containers
- Kubernetes
- Infrastructure as Code
- Middleware
- Databases
- Observability
- Security
- Quality
- AI

### Reference

- Cheatsheets
- Runbooks

### Learning

- Courses
- Certifications

### Management

- IT Management
- Design

### Workspace

- Inbox

Do not change these root groups during a technology migration unless explicitly
requested.

---

## Security

Never commit:

- credentials
- tokens
- passwords
- PATs
- private keys
- `.env`
- sensitive `.tfvars`
- `.tfstate`
- `.pem`
- `.key`
- private certificates
- confidential internal URLs
- tenant IDs when confidential
- customer names
- confidential company information
- secrets extracted from historical notes

Never migrate real company incidents directly into public documentation.

Convert reusable lessons into generic and sanitized documentation.

---

## Copyright

Do not copy entire third-party materials into this repository.

Do not migrate directly:

- books
- complete course PDFs
- proprietary presentations
- paid course content
- downloaded videos
- third-party documentation without redistribution rights

Prefer:

- original synthesis
- rewritten explanations
- official references
- links to authoritative sources

---

## Assets

Images and other assets must be intentional.

Use:

- `docs/assets/images/`
- `docs/assets/diagrams/`
- `docs/assets/files/`

when appropriate.

Only include external assets when:

- they are authored by the repository owner, or
- their license allows redistribution

Avoid copying large binary resources when linking to the source is sufficient.

---

## Code and repositories

Do not import complete application repositories into `docs/`.

Small examples and snippets are allowed.

Executable labs or substantial example applications should live in separate
repositories when required.

Do not migrate:

- `.git`
- `.venv`
- `node_modules`
- `site-packages`
- `__pycache__`
- `target`
- `build`
- `dist`
- caches
- generated artifacts
- complete source repositories

---

## Scope discipline

Only modify files required by the current task.

Do not opportunistically refactor unrelated documentation.

Do not expand a migration into adjacent technologies unless explicitly asked.

Example:

If the task is GitHub Actions, do not begin migrating:

- Terraform
- Azure
- Kubernetes
- Java
- Angular
- AI

unless explicitly authorized.

---

## Progressive migration strategy

Migration is progressive.

For each technology:

1. identify the explicitly authorized historical source
2. read only that source
3. determine canonical Knowledge
4. consolidate duplicated material
5. separate Cheatsheets when appropriate
6. separate Courses or Labs when appropriate
7. separate Runbooks when appropriate
8. create or update landing pages
9. validate navigation
10. run MkDocs strict build
11. review visually
12. commit only when approved
13. push only when approved

Do not perform bulk migration of all historical notes.

---

## Current reference implementation

The GitHub Actions migration under:

`docs/devops/cicd/github-actions/`

is the first reference implementation for future migrations.

It demonstrates:

- technology landing page
- category landing pages
- local `.pages`
- Knowledge separation
- Cheatsheet separation
- Course/Lab separation
- internal links
- Material navigation
- update-date convention

Future technologies should follow the same principles.

Do not mechanically copy the exact GitHub Actions folder structure if another
technology requires a different taxonomy.

---

## GitHub Actions documentation structure

The current GitHub Actions Knowledge structure is conceptually:

```text
GitHub Actions
├── Fundamentos
├── Ejecución
├── Reutilización
├── Pipelines
├── Seguridad
├── Avanzado
└── Referencias
```

Each meaningful category should have an `index.md`.

Navigation should behave conceptually as:

```text
GitHub Actions
    |
    +-- Fundamentos
            |
            +-- index.md
            +-- Conceptos y sintaxis
            +-- Eventos e inputs
            +-- Jobs, runners y shells
            +-- Variables, contexts y expresiones
```

Do not make a category link directly to an arbitrary child article when an
index landing page exists.

---

## Git workflow

Before changes:

```powershell
git status
```

Review current branch when relevant:

```powershell
git branch --show-current
```

The primary branch is:

`main`

After changes:

```powershell
git diff --check
git status
```

Do not assume a clean working tree.

---

## MkDocs environment

MkDocs is installed in the repository virtual environment.

Use:

```powershell
.\.venv\Scripts\mkdocs.exe
```

Do not assume `mkdocs` is installed globally in `PATH`.

---

## Required validation

For documentation changes, run at minimum:

```powershell
.\.venv\Scripts\mkdocs.exe build --strict
git diff --check
git status
```

A task is not complete if:

`mkdocs build --strict`

fails.

---

## Navigation validation

When changing:

- links
- `.pages`
- landing pages
- folder hierarchy
- navigation labels

also run temporarily:

```powershell
.\.venv\Scripts\mkdocs.exe serve --strict
```

Verify relevant pages return HTTP 200.

For example:

```text
/Learn-mkdocs/
/Learn-mkdocs/devops/cicd/
/Learn-mkdocs/devops/cicd/github-actions/
```

When category landing pages exist, validate those URLs too.

Stop the temporary server after validation.

---

## GitHub Pages

The site is published using GitHub Actions and GitHub Pages.

Current site URL:

`https://stevenoliva01.github.io/Learn-mkdocs/`

Do not introduce a separate `gh-pages` publishing strategy unless explicitly
requested.

Do not change the deployment workflow during ordinary content migrations.

---

## Workflow files

Current CI/CD workflows live under:

`.github/workflows/`

Do not modify them during documentation migrations unless:

- the current task explicitly requires it, or
- a real build/deployment defect is demonstrated

Documentation migration should not opportunistically refactor CI/CD.

---

## Commit and push policy

Do not commit or push automatically unless the current task explicitly
authorizes it.

Default workflow:

1. make requested changes
2. run required validation
3. report files created/modified
4. report build result
5. report `git diff --check`
6. report `git status`
7. wait for approval when requested
8. commit only when explicitly authorized
9. push only when explicitly authorized

Never use:

```text
git push --force
```

or:

```text
git push --force-with-lease
```

unless explicitly authorized.

Never rewrite repository history without explicit authorization.

---

## Commit messages

Use clear conventional-style commit messages.

Examples:

```text
docs(github-actions): migrate and structure GitHub Actions knowledge
```

```text
docs(terraform): migrate Terraform knowledge
```

```text
feat(ux): improve MkDocs navigation and homepage
```

```text
chore(ci): update GitHub Actions runtime dependencies
```

Do not create vague commit messages such as:

```text
update docs
```

or:

```text
changes
```

---

## Do not fabricate success

Do not report:

- build PASS
- HTTP 200
- link validation success
- deployment success
- clean Git status

unless the corresponding validation was actually executed.

If validation fails, report the failure clearly.

---

## Final response after implementation

After completing an implementation task, report:

1. files created
2. files modified
3. important decisions
4. navigation changes
5. validations executed
6. `mkdocs build --strict` result
7. `git diff --check` result
8. HTTP validation when applicable
9. `git status`
10. remaining issues or decisions

Do not claim the task is complete if required validation failed.