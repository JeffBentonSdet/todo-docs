# Todo Project

The canonical home for project-wide documentation, operational files, and cross-cutting architectural decision records for the todo application.

## Purpose

This repo is the source of truth for project-level concerns that span both `todo-api` and `todo-web`:

- **`docs/adr/`** — Cross-cutting ADRs covering project boundaries, API strategy, and deployment approach
- **`docs/plans/`** — Feature and milestone plans
- **`ideas.md`** — Backlog of ideas and future improvements
- **`status.md`** — Running project journal and status log
- **`CONTRIBUTORS.md`** — Local development setup and tooling (uses Overmind to run all services)
- **`Procfile`** — Overmind process definitions for running the full stack locally

## Git Workflow

- All work is tracked by a GitHub Issue before starting
- Branch from `main` using the naming convention: `<issue-number>-<short-description>` (e.g., `12-add-user-auth`)
- Work is merged to `main` via Pull Requests — never commit directly to `main`
- Each merge to `main` triggers CI/CD build and release
- Kanban workflow — no sprints, continuous flow

## ADR Format

All ADRs follow this structure:

```markdown
# ADR-00X: Title

## Status
Accepted

## Context
Why does this decision need to be made?

## Decision
What was decided?

## Alternatives Considered
What other options were evaluated?

## Consequences
What are the implications, good and bad?
```

## Conventions

- Number ADRs sequentially (001, 002, ...)
- Use kebab-case filenames (e.g., `001-separate-frontend-backend-repos.md`)
- Write for a new developer joining the project — include enough context to understand the "why"
- Once accepted, ADRs are immutable; supersede with a new ADR rather than editing
