# AGENTS.md
## Project purpose
`Learn-mkdocs` is a personal technical knowledge base built with MkDocs Material.

The repository consolidates historical technical notes into structured, maintainable and searchable documentation.

The canonical information architecture is:

`Domain -> Technology -> Concept`

Do not organize canonical knowledge primarily by:

- course
- provider
- book
- certification
- original file location

The goal is to transform historical notes into a curated knowledge base, not to reproduce the historical folder structure.

---

## Source of truth
Before making structural or editorial decisions, read:

- `analysis/architecture-decisions.md`
- `analysis/proposed-structure.md`
- `analysis/migration-map.md`

These documents define the approved architecture and migration rules.

Do not redesign the repository architecture unless explicitly requested.

If a task conflicts with an existing ADR, report the conflict before changing the architecture.

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

Historical notes are input material, not automatically current technical truth.

When migrating or deepening a technology that changes over time:

- preserve useful knowledge from the authorized historical source
- verify current behavior against authoritative documentation when accuracy may have changed
- prefer official vendor or project documentation for mutable technical facts
- distinguish historical practices from current recommended practices
- do not preserve outdated syntax only because it exists in OneDrive
- do not invent missing knowledge; research it only when the current task authorizes expansion or deepening

The objective is not merely to move notes from OneDrive into MkDocs. The objective
is to turn them into current, understandable and reusable technical knowledge.

---

## Knowledge depth and teaching standard
Canonical Knowledge should not behave like a collection of isolated snippets.

For important concepts, explain enough context that a reader can understand:

- what the concept is
- why it exists
- what problem it solves
- when to use it
- when not to use it
- how it works conceptually
- its essential syntax or configuration
- a minimal example
- a realistic example when useful
- the expected result
- common errors and troubleshooting clues
- good practices and security considerations
- relationships with adjacent concepts
- comparisons with familiar platforms when they materially improve understanding

Do not assume that the reader already knows why a feature exists. Explain the
problem and purpose before focusing on syntax.

Avoid reducing canonical Knowledge to definitions followed immediately by code.
The preferred progression is:

`problem -> purpose -> mental model -> syntax -> example -> validation -> caveats`

When Azure DevOps provides a useful comparison, explain the correspondence and
also the differences. Do not force one-to-one equivalences when the platforms use
different abstractions.

Do not duplicate this depth inside Cheatsheets or Labs. Canonical explanation
belongs in Knowledge.

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

Course material should link back to canonical Knowledge instead of re-teaching
the same concept in full.

---

### Guided labs and exercises
Guided labs and exercises are different learning assets.

An exercise is usually a smaller challenge used to practice or test a concept.
It may intentionally provide less guidance.

A guided lab is a reproducible practical walkthrough with an observable result.
A lab should normally include:

- objective
- what the learner will practice
- prerequisites
- conceptual flow or architecture when useful
- exact files and paths to create
- step-by-step implementation
- explanation of why each important step exists
- commands required to trigger or reproduce behavior
- expected observable result
- validation steps
- at least one deliberate failure or troubleshooting exercise when useful
- common errors
- cleanup when persistent resources are created
- links back to canonical Knowledge
- a clear next step or related lab when part of a learning path

The preferred lab learning cycle is:

`build -> run -> observe -> break -> diagnose -> fix -> rerun`

A lab must not consist only of a YAML or command block to copy. It should explain
what the learner is reproducing and how to know whether it worked.

Keep labs under the relevant course path when they belong to a complete course,
an independent learning path, course-specific material, or a multi-technology
sequence. Technology-specific labs may instead live inside their canonical
technology tree when they are a direct part of that documentation and keeping
theory and practice together improves navigation. For example:

`docs/devops/cicd/github-actions/labs/`

Canonical theory for the same subject remains under its Knowledge location, for
example:

`docs/devops/cicd/github-actions/`

Use cross-links between theory and labs instead of duplicating complete
explanations.

Labs that require external services, paid features, cloud subscriptions,
organization-level configuration or special permissions must state those
prerequisites before the procedure starts. Prefer a no-cost or simulated path
when it can teach the same core concept.

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
Important domains, technologies and meaningful internal categories should have an `index.md`.

Examples:

`docs/devops/index.md`

`docs/devops/cicd/index.md`

`docs/devops/cicd/github-actions/index.md`

`docs/devops/cicd/github-actions/fundamentals/index.md`

A navigation category should normally open its landing page before an individual article.

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

A category card should normally link to its `index.md`, not arbitrarily to its first article.

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

Do not execute historical workflow examples unless the current task explicitly requires execution.

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

Do not change these root groups during a technology migration unless explicitly requested.

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

Executable labs or substantial example applications should live in separate repositories when required.

Documentation labs may contain complete copyable snippets, expected file trees
and commands, but must not turn `Learn-mkdocs` into an application repository or
activate training workflows that create unnecessary side effects.

When a learning path outgrows documentation-only examples, prefer a dedicated
repository such as a technology-specific lab repository. In that model:

- `Learn-mkdocs` remains the source of theory and guided instructions
- the lab repository contains executable projects and workflows
- MkDocs links to the exact lab or example
- the executable repository links back to the relevant theory
- avoid maintaining two independent copies of the same explanation

Do not create a separate executable repository unless the current task explicitly
authorizes it.

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
Migration is progressive and should mature a technology through three editorial
stages instead of stopping after files have been copied or reorganized.

**Stage 1 - Curate and migrate**

1. identify the explicitly authorized historical source
2. read only that source
3. inventory useful material
4. determine canonical Knowledge
5. consolidate duplicated material
6. normalize terminology and structure
7. separate Cheatsheets when appropriate
8. separate Courses, Exercises or Labs when appropriate
9. separate Runbooks when appropriate
10. create or update landing pages and local navigation

**Stage 2 - Deepen and verify**

11. identify canonical pages that are only definitions, snippets or shallow notes
12. expand important concepts using the Knowledge depth and teaching standard
13. verify mutable technical behavior against authoritative current sources when authorized
14. add conceptual diagrams, realistic examples and troubleshooting where useful
15. add comparisons with familiar platforms only when they improve understanding
16. remove obsolete duplication instead of preserving every historical variant

**Stage 3 - Practice and validate learning**

17. create exercises for focused practice when useful
18. create guided reproducible labs for important workflows or behaviors
19. link labs to canonical Knowledge and canonical Knowledge to the most relevant labs
20. keep external or cloud-dependent labs clearly marked and optional when possible

**Repository validation**

21. validate navigation and internal links
22. run MkDocs strict build
23. review visually when navigation or layout changed
24. run Git diff validation
25. commit only when approved
26. push only when approved

A migration is not considered editorially mature merely because historical notes
exist under `docs/`. Important technologies should progressively move from
curated notes to deep canonical Knowledge and then to practical learning material.

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
- canonical theory that explains purpose before syntax
- progressive deepening after initial migration
- guided labs that reproduce important concepts
- theory-to-lab cross-links
- internal links
- Material navigation
- update-date convention

Future technologies should follow the same principles.

Do not mechanically copy the exact GitHub Actions folder structure if another technology requires a different taxonomy.

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

Do not make a category link directly to an arbitrary child article when an index landing page exists.

GitHub Actions is currently the reference technology for the full migration
maturity model. Its canonical Knowledge should be intentionally deeper than a
cheatsheet. In particular, important features such as events, jobs, runners,
steps, actions, contexts, reusable workflows, environments, permissions and OIDC
should explain why they exist and when they are appropriate before presenting
the YAML.

The current Knowledge structure may include specialized pages such as:

- model of execution and isolation
- steps and actions
- reusable workflows
- composite actions
- workflow templates

when separating those concepts improves learning. Do not fragment the navigation
into one tiny page per keyword.

GitHub Actions guided reproducible labs belong under:

`docs/devops/cicd/github-actions/labs/`

Do not duplicate the same technology in Knowledge and Courses solely to
separate theory from practice. GitHub Actions is the reference structure when
keeping both together improves navigation:

```text
Technology
├── Knowledge
└── Labs
```

Existing exercise inventories and guided labs serve different purposes and should
coexist:

- exercises are smaller challenges or drills
- labs are guided end-to-end reproductions with observable validation

For GitHub Actions labs, prefer early exercises that require only GitHub. Mark
organization-level features, Azure, Terraform or other external dependencies as
advanced or optional when they are not necessary for the core learning objective.

Do not place training workflows under the repository's active
`.github/workflows/` merely to make a lab executable. The documentation must
show the learner what to create in their own test repository.

If a dedicated executable GitHub Actions lab repository is created in the future,
it must be explicitly authorized and should complement, not replace, the MkDocs
theory and guided instructions.

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

Do not introduce a separate `gh-pages` publishing strategy unless explicitly requested.

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
Do not commit or push automatically unless the current task explicitly authorizes it.

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

For learning or lab implementation tasks, also report when applicable:

11. labs or exercises created
12. which labs are self-contained and which require external services
13. theory-to-lab links added
14. cleanup or cost-related caveats introduced

Do not claim the task is complete if required validation failed.
