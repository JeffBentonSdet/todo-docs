# ADR-003: Independent Deployment Lifecycles for API and Web Frontend

## Status
Accepted

## Context
With the frontend and backend maintained as separate projects by potentially different teams, we need to decide whether deployments should be coordinated or independent. Tightly coupled deployments can become a bottleneck, especially when teams work at different velocities. However, independent deployments require careful API versioning and backward compatibility practices.

## Decision
The API (`todo-api`) and web frontend (`todo-web`) are separate projects with independent deployment lifecycles. Each project can be built, tested, and deployed on its own schedule without requiring the other to be deployed simultaneously. API versioning and backward-compatible changes are used to ensure the frontend and backend remain compatible across independent releases.

## Alternatives Considered
- **Coordinated deployments:** Both frontend and backend are always deployed together in lockstep. Simpler to reason about compatibility, but creates a deployment bottleneck and prevents teams from releasing independently. A bug fix in one project would require deploying both.
- **Monolithic deployment:** A single deployable artifact containing both frontend and backend. Simplest deployment model, but eliminates team autonomy and prevents independent scaling.

## Consequences
- **Good:** Teams can release bug fixes, improvements, and new features on their own schedule without waiting for the other team.
- **Good:** Each project can be scaled independently based on its own resource requirements.
- **Good:** Reduces deployment risk — a failed frontend deployment doesn't affect the API and vice versa.
- **Bad:** Requires disciplined API versioning and backward compatibility practices to avoid breaking the frontend when the API changes.
- **Bad:** Integration testing across projects becomes more important and potentially more complex to orchestrate.
