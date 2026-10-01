# Grit

## Summary
A full-stack application framework/generator targeting Go + Next.js: one command scaffolds a monorepo (API, admin, web, mobile, desktop) with auth, 2FA, passkeys, RBAC, an admin panel, background jobs/cron, file storage, email, webhooks, realtime, feature flags, a verifiable audit log, and one-command deploy already wired. `grit generate resource X` writes the Go model/service/handler/routes, a shared Zod schema, TypeScript types, React Query hooks, and an admin CRUD screen from one field-definition command.

## Why I Should Care
Positions itself explicitly as "Laravel + Filament for Go and React" — the exact kind of batteries-included scaffold that removes the week-one setup tax (auth, admin panel, jobs, storage, deploy) from any new SaaS idea. Directly hits the micro-SaaS/automation leverage axis: it shortens the path from idea to a shippable paid product.

## Problems It Can Remove
Replaces hand-assembling a router, ORM, auth library, validator, mailer, queue, S3 client, and admin kit for every new project. Replaces building an admin panel per project. Replaces hand-writing a deploy pipeline (Dockerfile, reverse proxy, TLS, systemd).

## Practical Uses
- Standing up a client-facing or internal SaaS MVP (CRM, invoicing, support desk, inventory) in hours
- Prototyping a micro-SaaS idea far enough to validate before committing to a hand-built backend
- Generating an internal admin tool for an existing data model without hand-building CRUD screens
- A reference architecture for a batteries-included typed full-stack scaffold, even without direct adoption

## Product Opportunities
Could serve as the scaffold for new client engagements if a Go backend is acceptable, cutting weeks off a build. The generated audit log, RBAC, and plugin system (Stripe, push, video) are exactly the plumbing a micro-SaaS needs before it can charge anyone.

## Agent / Automation Opportunities
None directly exposed as an MCP/agent capability. The README does mention "Build with AI" tooling for using coding agents against the generated codebase, not reviewed in depth this run.

## Integration
CLI installer (curl script, no Go toolchain required) or `go install`. Local dev needs Go 1.21+, Node 20+, pnpm, and Docker (for Postgres/Redis/MinIO/Mailhog). Deploys via `grit deploy` over SSH with systemd + Caddy, or documented guides for Railway, Render, Fly.io, Coolify, Dokploy. Integration effort: **Medium** — straightforward to scaffold a new project, but adopting it means taking on a Go backend rather than extending an existing Node/Express one.

## Architecture Notes
Code generation writes a Go API (GORM models, services, handlers, routes) and a Next.js frontend as plain, readable files with "no runtime magic" — no reflection or hidden proxies, so stopping use of the CLI still leaves a normal buildable project. Types flow Go → Zod + TypeScript via `grit sync`.

## Maturity
Emerging. Created February 2026 (~8 months old), 136 stars, 17 forks, 33 open issues. Calendar-style version numbers (v3.337.1) suggest frequent incremental builds rather than a settled semver cadence — treat as pre-1.0 in spirit despite the version number. Contributor activity appears concentrated in a small team.

## License
MIT, verified via GitHub API. No restrictions on commercial use, SaaS deployment, redistribution, or embedding.

## Alternatives
- **Laravel + Filament** (PHP) — the explicit model Grit is copying into Go/TypeScript.
- **AdminForth** (already catalogued, USE NOW) — narrower: generates only the admin panel over an existing backend, whereas Grit generates the whole stack including the backend.
- Hand-assembling Express/Fastify + Prisma + a React admin kit — the status quo this replaces, at the cost of adopting Go instead of Node for the backend.

## Risks / Limitations
- Young project with limited track record; version numbering style suggests active, fast-moving but not yet settled development.
- Backend is Go + GORM, not Node/Express — a second backend language alongside the MERN stack rather than an extension of it.
- Small apparent team; bus-factor risk for anything built as a real product on top of it.

## Recommendation
**PROTOTYPE** — strong fit for the micro-SaaS leverage axis despite the Go backend being outside the core MERN stack. Worth a real trial scaffold for a side project or small client engagement before trusting it with anything load-bearing.

## Change History
### 2026-10-01
Initial discovery and review. Found via GitHub Search API (authorization/RBAC-adjacent query surfaced it through its audit-log description), slot 2 (product infrastructure) run.
