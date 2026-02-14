# ADR-001: Separate Frontend and Backend Repositories

## Status
Accepted

## Context
The todo application requires both a web frontend and a backend API. As the project grows, different teams may own and develop these layers independently. We need to decide whether to use a monorepo approach or separate the frontend and backend into distinct projects.

Team autonomy, independent release cadences, and clear ownership boundaries are important considerations for long-term maintainability. Different teams may have different tooling preferences, CI/CD pipelines, and deployment targets.

## Decision
The project is split into separate frontend (`todo-web`) and backend (`todo-api`) projects, each representing separate team ownership. A shared `todo-docs` project houses cross-cutting architectural decisions and documentation.

## Alternatives Considered
- **Monorepo with shared tooling (e.g., Turborepo, Nx):** Would simplify cross-project changes and dependency management, but introduces coupling between teams and requires agreement on shared tooling. Adds complexity for teams that want to move independently.
- **Single repository with frontend and backend directories:** Simpler setup, but blurs ownership boundaries and makes it harder to enforce independent deployment and CI/CD pipelines.

## Consequences
- **Good:** Each team has full autonomy over their project's tooling, dependencies, CI/CD, and release schedule. Clear ownership boundaries reduce coordination overhead.
- **Good:** Independent repositories allow for different branching strategies and code review workflows per team.
- **Bad:** Cross-cutting changes (e.g., API contract changes) require coordination across repositories and may be harder to keep in sync.
- **Bad:** Shared types or contracts must be managed through explicit versioning or contract testing rather than direct imports.
