# ADR-004: GitHub-Based Project Management with Kanban Workflow

## Status
Accepted

## Context
The todo application is split across three repositories (todo-docs, todo-api, todo-web) with independent deployment lifecycles. Each merge to main triggers a CI/CD pipeline that builds and releases automatically. We need a project management approach that provides cross-repo visibility, supports a continuous flow of work without fixed sprints, and integrates naturally with the GitHub-based development workflow.

## Decision
We use GitHub's native project management features with a kanban workflow:

### GitHub Project Board
A single user-level GitHub Project pulls issues from all three repositories into a unified view.

**Status columns:**
- **Backlog** — Captured but not yet prioritized
- **Ready** — Prioritized and ready to be picked up
- **In Progress** — Actively being worked on
- **In Review** — Pull request open, awaiting review
- **Done** — Merged and deployed (auto-closed via CI/CD)

**Custom fields:**
- **Priority** — High, Medium, Low
- **Effort** — S (a few hours or less), M (up to a day or two), L (multiple days, consider breaking down)

**Views:**
- **Board view** — Primary view for daily work, visualizing flow through status columns
- **Table view** — Used for backlog grooming, sorting and filtering by priority and effort

### Labels
A consistent label scheme is applied across all three repositories:

| Label | Purpose |
|-------|---------|
| `feature` | New functionality |
| `bug` | Something broken |
| `chore` | Maintenance, dependencies, CI/CD |
| `docs` | Documentation changes |
| `api-contract` | Cross-project API change requiring coordination |
| `priority:high` | High priority |
| `priority:medium` | Medium priority |
| `priority:low` | Low priority |
| `effort:s` | Small — a few hours or less |
| `effort:m` | Medium — up to a day or two |
| `effort:l` | Large — multiple days, consider breaking down |

### Issue Templates
Each repository has standardized issue templates:
- **Bug report** — Steps to reproduce, expected vs actual behavior, environment
- **Feature request** — User story, acceptance criteria, affected projects
- **ADR proposal** (todo-docs only) — Problem statement, proposed decision, alternatives

### What We Don't Use
- **Milestones** — Not needed; no sprints or release groupings since every merge to main deploys automatically via CI/CD
- **GitHub Discussions** — ADRs in-repo serve as the source of truth for decisions
- **GitHub Wiki** — Documentation lives in-repo alongside the code it describes

## Alternatives Considered
- **External tools (Jira, Linear, Trello):** More feature-rich but introduces context switching away from GitHub where the code, PRs, and CI/CD already live. Adds cost and integration overhead.
- **Sprint-based workflow with milestones:** Would add artificial time-boxing to a CI/CD workflow where every merge deploys. Kanban's continuous flow better matches the deployment model.
- **Per-repo project boards:** Would provide project-specific views but fragments visibility across the three repositories. A single cross-repo board better supports coordination, especially for `api-contract` changes.
- **GitHub Issues without a Project board:** Issues alone lack the visual workflow and cross-repo aggregation needed to manage work across three repositories.

## Consequences
- **Good:** All project management lives in GitHub alongside the code, PRs, and CI/CD — no context switching.
- **Good:** The cross-repo Project board provides unified visibility into work across all three repositories.
- **Good:** Kanban's continuous flow aligns naturally with CI/CD where every merge deploys.
- **Good:** Consistent labels across repos make it easy to filter and triage in the unified board.
- **Good:** The `api-contract` label provides an explicit signal when coordination is needed between API and web teams.
- **Bad:** GitHub Projects has fewer features than dedicated tools like Jira (no time tracking, limited reporting, no dependency graphs between issues).
- **Bad:** Label consistency across repos must be maintained manually or via automation — there is no built-in mechanism for syncing labels across repositories.
