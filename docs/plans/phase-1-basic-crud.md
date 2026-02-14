# Phase 1: Basic Todo CRUD

## Goal

A working todo application where users can view, create, mark as done, and delete todos. Backend API is implemented first, then the frontend consumes it.

## Approach

Small, iterative issues. Each issue is independently implementable, testable, and deployable. Backend issues are completed before their corresponding frontend issues begin.

---

## Backend (todo-api)

### 1. Project bootstrap

Set up the Flask application factory, configuration, and development tooling so the app can start and serve requests.

- `pyproject.toml` with dependencies (Flask, SQLAlchemy, Marshmallow, Strawberry or Ariadne)
- Flask application factory in `__init__.py`
- Configuration classes in `config.py` (dev, test)
- Extension initialization in `extensions.py`
- Health check endpoint (`GET /health`)
- Development server runs with `uv run flask run`

### 2. Database and Todo model

Set up SQLAlchemy with SQLite (dev) and define the Todo domain model and database table.

- Database setup in `infrastructure/database.py`
- Todo domain entity in `features/todos/domain.py` — fields: `id`, `title`, `completed`, `created_at`, `updated_at`
- SQLAlchemy model mapping
- Database initialization on app startup

### 3. Todo repository

Implement the repository port and SQL adapter for todo persistence.

- Repository Protocol in `features/todos/repository.py` — methods: `get_all()`, `get_by_id()`, `create()`, `update()`, `delete()`
- SQL adapter in `features/todos/adapters/sql_repository.py`
- Unit tests with an in-memory database

### 4. Todo service layer

Implement the service layer orchestrating domain logic through the repository port.

- Service in `features/todos/service.py` — use cases: list todos, get todo, create todo, toggle completed, delete todo
- Error handling with Result type or domain exceptions
- Unit tests with mocked repository

### 5. REST API endpoints

Expose todo CRUD operations via REST.

- Blueprint and routes in `features/todos/rest/`
- Request/response schemas in `features/todos/rest/schemas.py`
- Endpoints:
  - `GET /api/todos` — list all todos
  - `GET /api/todos/:id` — get a single todo
  - `POST /api/todos` — create a todo
  - `PATCH /api/todos/:id` — update a todo (toggle completed)
  - `DELETE /api/todos/:id` — delete a todo
- Error responses with appropriate status codes
- Integration tests with Flask test client

### 6. GraphQL API

Expose todo CRUD operations via GraphQL.

- GraphQL types in `features/todos/graphql/types.py`
- Queries in `features/todos/graphql/queries.py` — `todos`, `todo(id)`
- Mutations in `features/todos/graphql/mutations.py` — `createTodo`, `toggleTodo`, `deleteTodo`
- Unified schema assembly in `graphql/schema.py`
- GraphQL endpoint mounted at `/graphql`
- Integration tests executing queries against the schema

---

## Frontend (todo-web)

### 7. Project bootstrap

Set up Next.js with dependencies, providers, and API client configuration so the app can start and communicate with the backend.

- `package.json` with dependencies (TanStack Query, urql, Tailwind, shadcn/ui)
- Tailwind and shadcn/ui configuration
- API client in `lib/api-client.ts`
- GraphQL client in `lib/graphql-client.ts`
- Provider setup in `app/providers.tsx` (QueryClient, urql)
- Root layout with providers
- Environment variables for API base URL

### 8. Todo types and API layer

Define TypeScript types and implement the data-fetching layer for todos.

- Todo types in `features/todos/types.ts`
- REST API functions in `features/todos/api.ts`
- GraphQL documents in `features/todos/graphql.ts`
- TanStack Query hooks in `features/todos/queries.ts` — `useTodos`, `useTodo`, `useCreateTodo`, `useToggleTodo`, `useDeleteTodo`

### 9. View todos

Build the todo list page that displays all todos.

- Todos page in `app/(app)/todos/page.tsx` — Server Component for initial fetch
- `TodoList` component in `components/todos/todo-list.tsx`
- `TodoItem` component in `components/todos/todo-item.tsx`
- Loading state in `app/(app)/todos/loading.tsx`
- Error boundary in `app/(app)/todos/error.tsx`
- Basic layout shell (navbar, page header)

### 10. Create todo

Add the ability to create new todos from the UI.

- `TodoForm` component in `components/todos/todo-form.tsx`
- Form with title input and submit button
- Calls `useCreateTodo` mutation
- Optimistic update or invalidation of todo list query
- Form validation (title required)

### 11. Toggle todo completed

Add the ability to mark a todo as done or not done.

- Checkbox or toggle in `TodoItem`
- Calls `useToggleTodo` mutation
- Optimistic update for instant feedback
- Visual distinction for completed todos (strikethrough, muted)

### 12. Delete todo

Add the ability to delete a todo.

- Delete button in `TodoItem`
- Confirmation before delete
- Calls `useDeleteTodo` mutation
- Optimistic removal from list

---

## Issue Dependency Order

```
1. Project bootstrap (api)
2. Database and Todo model (api)         → depends on 1
3. Todo repository (api)                 → depends on 2
4. Todo service layer (api)              → depends on 3
5. REST API endpoints (api)              → depends on 4
6. GraphQL API (api)                     → depends on 4
7. Project bootstrap (web)               → independent, can parallel with api work
8. Todo types and API layer (web)        → depends on 5 or 6
9. View todos (web)                      → depends on 8
10. Create todo (web)                    → depends on 9
11. Toggle todo completed (web)          → depends on 9
12. Delete todo (web)                    → depends on 9
```

Issues 10, 11, and 12 can be worked in parallel once issue 9 is complete.
