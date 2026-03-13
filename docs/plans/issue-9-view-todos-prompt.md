# Issue 9: View Todos — Implementation Prompt

## Context

You are working on the `todo-web` project, a Next.js 15 / TypeScript frontend for a todo application. The project uses Vertical Slice Architecture, TanStack Query for data fetching, and urql for GraphQL.

**Working directory:** `todo-web/`

Before writing any code, follow the git workflow:
1. Create a GitHub Issue for this work (title: "View todos page — wire up data fetching and render todo list")
2. Branch from `main` using the convention `<issue-number>-view-todos` (e.g., `9-view-todos`)
3. Merge via Pull Request — do not commit directly to `main`

---

## What's Already Built

All the pieces exist — this task is purely integration work.

### Hooks (ready to use)
- `src/features/todos/queries.ts` — `useTodos(filter?)` hook that fetches all todos via REST

### Components (ready to use)
- `src/components/todos/todo-list.tsx` — renders a list of `TodoItem` components; accepts a `todos` array and callbacks
- `src/components/todos/todo-item.tsx` — renders a single todo with toggle and delete buttons; accepts `onStatusChange` and `onDelete` callbacks
- `src/components/todos/todo-filters.tsx` — status and search filter UI; accepts filter state and a change callback
- `src/components/layout/page-header.tsx` — page title header component

### Page files (exist but incomplete)
- `src/app/(app)/todos/page.tsx` — currently has a placeholder, needs real implementation
- `src/app/(app)/todos/loading.tsx` — skeleton UI, already complete
- `src/app/(app)/todos/error.tsx` — error boundary, already complete

### Types
- `src/features/todos/types.ts` — `Todo`, `TodoStatus`, `TodoFilter` interfaces

---

## What Needs to Be Done

Wire up `src/app/(app)/todos/page.tsx` so it:

1. **Fetches todos** using the `useTodos()` hook from `src/features/todos/queries.ts`
2. **Renders the todo list** using the `TodoList` component
3. **Shows the create form** — mount `TodoForm` in this page (wiring to mutations is covered in Issue 10; for now, `onSubmit` can be a no-op or left as a prop)
4. **Shows filters** — mount `TodoFilters` and connect local filter state to `useTodos(filter)`
5. **Handles loading and empty states** — the `loading.tsx` skeleton fires automatically via Suspense; ensure the page renders correctly when the list is empty

The page must be a **Client Component** (`"use client"`) because it uses hooks.

### Acceptance criteria
- Navigating to `/todos` shows the list of todos fetched from the API
- A loading skeleton appears while the request is in flight
- An empty state is shown when there are no todos
- The filter UI updates which todos are shown
- No TypeScript errors
- No console errors

---

## Key Files to Read First

Before writing code, read these files to understand the existing contracts:

- `src/app/(app)/todos/page.tsx` — see what's there now
- `src/features/todos/queries.ts` — understand `useTodos` signature and return shape
- `src/features/todos/types.ts` — `Todo` and `TodoFilter` types
- `src/components/todos/todo-list.tsx` — props interface
- `src/components/todos/todo-item.tsx` — props interface
- `src/components/todos/todo-filters.tsx` — props interface
- `src/components/todos/todo-form.tsx` — props interface (for mounting, not wiring yet)

---

## Architecture Notes

- **Vertical Slice:** feature logic lives in `src/features/todos/`, UI components in `src/components/todos/`
- **Client vs Server Components:** pages that use hooks must be `"use client"`; the `loading.tsx` and `error.tsx` files handle Suspense/error boundaries automatically in Next.js App Router
- **TanStack Query:** `useTodos()` returns `{ data, isLoading, isError, error }` — use these for conditional rendering
- **API base URL:** configured via `NEXT_PUBLIC_API_BASE_URL` in `.env.example` — ensure `.env.local` is set pointing at the running `todo-api`

---

## Out of Scope for This Issue

Do not implement these — they are separate issues:
- Creating todos (Issue 10)
- Toggling todo completed (Issue 11)
- Deleting todos (Issue 12)

Callbacks like `onStatusChange` and `onDelete` on `TodoItem` can be passed as no-ops (`() => {}`) for now.
