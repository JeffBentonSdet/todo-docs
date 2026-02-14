# Todo Docs

Cross-cutting architectural decision records for the todo application.

## Purpose

This project holds ADRs that span both the API and web frontend — decisions about project boundaries, API strategy, and deployment approach.

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
