# ADR-002: Backend Exposes Both REST and GraphQL Interfaces

## Status
Accepted

## Context
The backend API needs to serve data to the frontend application. REST is the most widely adopted API style and is well-understood by most developers. GraphQL offers more flexible querying, reduced over-fetching, and a strong type system. Different consumers of the API may benefit from different interface styles — REST for simple CRUD operations and external integrations, GraphQL for complex frontend data requirements.

## Decision
The backend exposes both REST and GraphQL interfaces. REST endpoints provide standard CRUD operations, while the GraphQL API offers flexible querying capabilities. Both interfaces share the same underlying domain logic and service layer, ensuring consistent behavior regardless of how the API is consumed.

## Alternatives Considered
- **REST only:** Simpler to implement and maintain, widely supported by tooling and infrastructure. However, can lead to over-fetching or require multiple requests for complex views, and lacks the introspection and type safety benefits of GraphQL.
- **GraphQL only:** Provides maximum flexibility for clients and excellent developer experience with tooling like GraphiQL. However, may be overkill for simple CRUD operations, has a steeper learning curve, and can be harder to cache and monitor.
- **REST with BFF (Backend for Frontend):** Adds a dedicated aggregation layer for the frontend, but introduces additional infrastructure complexity and another service to maintain.

## Consequences
- **Good:** Frontend developers can choose the most appropriate interface for each use case — REST for simple operations, GraphQL for complex data requirements.
- **Good:** External consumers and integrations can use the familiar REST API without needing to learn GraphQL.
- **Bad:** Maintaining two API interfaces increases the surface area for bugs and requires additional testing.
- **Bad:** Developers must understand both REST and GraphQL conventions, increasing the learning curve for new team members.
