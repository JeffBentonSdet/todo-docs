# Plan: Rename todo-docs to todo-project and Consolidate Project-Level Files

**Issue:** [#3 — Chore: rename todo-docs to todo-project and consolidate project-level files](https://github.com/JeffBentonSdet/todo-docs/issues/3)

## Goal

Expand the role of `todo-docs` from an ADR-only repository to the canonical home for all project-wide documentation and operational files. Rename it `todo-project` to reflect this broader purpose.

## Steps

### 1. Branch
Create branch `3-consolidate-project-docs` from `main` in `todo-docs`.

### 2. Move files from local `todo-project/` into `todo-docs/`
Copy the following files into the `todo-docs` repo root:

| Source (`todo-project/`) | Destination (`todo-docs/`) |
|---|---|
| `ideas.md` | `ideas.md` |
| `status.md` | `status.md` |
| `CONTRIBUTORS.md` | `CONTRIBUTORS.md` |
| `Procfile` | `Procfile` |

### 3. Update `todo-docs/CLAUDE.md`
- Expand the purpose section to describe the repo as the home for project-wide docs, not just ADRs
- Document what each root-level file is for (`ideas.md`, `status.md`, `CONTRIBUTORS.md`, `Procfile`)

### 4. Open PR and merge to `main`
PR against `main` in `todo-docs`. Merge before renaming the repo so git history is clean.

### 5. Rename the GitHub repository
In GitHub Settings, rename `todo-docs` → `todo-project`.

### 6. Rename the local directory
```
mv todo-docs todo-project
```

### 7. Update the local remote URL
```
cd todo-project
git remote set-url origin https://github.com/JeffBentonSdet/todo-project.git
```

### 8. Update references to `todo-docs` across the workspace
- Root `todo/CLAUDE.md` — update project structure description
- `todo-api/CLAUDE.md` (if it references `todo-docs`)
- `todo-web/CLAUDE.md` (if it references `todo-docs`)
- `todo-project.code-workspace` — update any paths

### 9. Retire the old local `todo-project/` directory
Remove files that have been migrated. The remaining files (`CLAUDE.md`, `todo-project.code-workspace`) should either move into the new `todo-project` repo or be evaluated for deletion.

## Out of Scope

- No changes to ADR content
- No changes to `todo-api` or `todo-web` source code
