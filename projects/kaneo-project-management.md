# Kaneo

## Summary
A self-hosted, minimal project-management tool positioned as a deliberately stripped-down Linear/Jira alternative — Kanban boards, projects, and workspaces without the notification and workflow sprawl of larger PM suites. TypeScript throughout (React frontend, Bun/Hono-style backend inferred from the monorepo layout), Postgres-backed, MIT-licensed, with a real multi-contributor team and an optional managed cloud.

## Why I Should Care
Directly in the MERN-adjacent stack (TypeScript/React/Postgres) rather than a foreign ecosystem, so it's realistic to both self-host as-is and read/borrow from its codebase. It's also a plausible internal tool: a lightweight PM board for personal projects or a small team, without paying for Linear/Jira seats or standing up something heavier like a self-hosted OpenProject/Redmine instance.

## Problems It Can Remove
- Per-seat SaaS PM tool cost (Linear, Jira, Asana) for a solo developer or small team.
- The setup and maintenance overhead of heavier self-hosted alternatives (OpenProject, Redmine) when the actual need is a clean Kanban board.
- Building a bespoke internal task tracker from scratch for a small product team.

## Practical Uses
- Self-hosted personal/team Kanban board for side projects and client work.
- Reference implementation for a React + TypeScript + Postgres admin-style dashboard when building similar internal tooling.
- Candidate to embed a simplified board/task-list UI pattern into an internal admin panel rather than building one from zero.

## Product Opportunities
The "one-click deploy via a companion CLI" pattern (`drim`, a separate install/ops tool from the same org) plus a free self-hosted core with a paid managed cloud is a directly reusable go-to-market template for a small internal tool turned into a product.

## Agent / Automation Opportunities
Ships its own `skills/` directory, `AGENTS.md`, and `CLAUDE.md` at the repo root — native support for AI coding-agent skills is already part of the project, not bolted on. That makes it a candidate to wire into an agent-driven workflow (agent files tasks it discovers while working) with less glue code than a generic PM tool's REST API.

## Integration
Docker Compose (`compose.yml`, `compose.local.yml`, `compose.coolify.yml`), Helm charts (`charts/`) for Kubernetes, or the `drim` CLI installer for a batteries-included setup with automatic HTTPS and Postgres provisioning. Optional managed cloud if self-hosting isn't wanted. **Integration effort: Low.**

## Architecture Notes
Turborepo monorepo (`apps/`, `packages/`) with i18n tooling, OpenAPI schema checks, and a PLpgSQL component alongside the TypeScript codebase, suggesting real Postgres-side logic rather than an ORM-only backend. Biome for linting, Husky for commit hooks, Sentry integration present — the tooling of a project run with production discipline, not a weekend prototype.

## Maturity
Mature for its age. Created 2024-12-31 (~21 months), 9,136 stars, 802 forks (healthy ~9% fork ratio, no sign of inflated stars), latest release v2.25.0 (2026-09-17) — near-daily pushes. Contributor spread is real: `andrejsshell` (maintainer, 1,153 commits) plus `tinsever` (316), `randoneering` (199), `MonsPropre` (85), and several more in double digits — not a single-person project.

## License
MIT. No restrictions on commercial use, self-hosting, or embedding.

## Alternatives
Plane, Vikunja, Focalboard, Huly — other self-hosted Linear/Jira-style alternatives. Taskosaur (evaluated 2026-09-13, rejected here for a Business Source License misrepresented as "open source"). Kaneo's distinguishing pitch is explicitly minimalism ("every feature exists because it solves a real problem") against tools like Huly/Plane that have grown feature-heavy; worth confirming that claim holds up against the current state of those alternatives before treating it as settled.

## Risks / Limitations
- Crowded niche — the self-hosted-Linear-alternative space has several credible competitors; differentiation is UX/minimalism, not unique capability.
- No independent security audit found; standard self-hosting due diligence (auth, network exposure) applies as with any self-hosted admin tool.

## Recommendation
USE NOW — low-risk, MIT-licensed, Docker-deployable, and directly in-stack. Worth trying as a personal or small-team PM board before reaching for a heavier self-hosted alternative or paying for Linear/Jira seats.

## Change History
### 2026-09-20
Discovered and reviewed. GitHub API verified: MIT, 9,136 stars, 802 forks, created 2024-12-31, latest release v2.25.0 (2026-09-17), archived: false. Repo contents confirm Docker Compose, Helm charts, and native `skills/`/`AGENTS.md`/`CLAUDE.md` agent-skill support.
