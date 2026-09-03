# Liteque

## Summary
Liteque is a small, typesafe, SQLite-backed job queue for Node.js/TypeScript, extracted
from the Karakeep (formerly Hoarder) self-hosted bookmarking app. No Redis, no Postgres,
no external services — just a SQLite file, a typed queue, and a runner with retries and
Zod-validated payloads.

## Why I Should Care
Most Node job-queue options (BullMQ, pg-boss, Sidequest) assume you already run Redis or
Postgres. For a small service or micro-SaaS side project that doesn't otherwise need
either, Liteque gives background jobs (retries, concurrency, timeouts) with zero
additional infrastructure to provision or pay for.

## Problems It Can Remove
Removes the "spin up Redis just to get a job queue" tax for small services, and removes
hand-rolled setTimeout/cron-based background processing that lacks retries or typed
payloads.

## Practical Uses
- Background email sending, webhook retries, or scheduled cleanup jobs in a small Node
  service without adding Redis to the deployment.
- Local-first or single-VM micro-SaaS products where SQLite is already the database of
  choice.
- Prototyping background-job behavior before deciding whether a heavier queue (Sidequest,
  BullMQ) is actually needed at scale.

## Product Opportunities
Removes one more infra dependency from the "smallest possible footprint" version of a
micro-SaaS MVP — relevant directly to the product-building-blocks priority.

## Agent / Automation Opportunities
None specific — it's a plain library, not an agent-facing tool.

## Integration
`npm install liteque`; in-process library, no separate service to run. **Integration
effort: Low** — smaller surface area than BullMQ or Sidequest by design.

## Architecture Notes
Typed queue and runner built around a SQLite client with built-in migrations
(`buildDBClient(..., { runMigrations: true })`), Zod schema validation on job payloads,
and lifecycle hooks (`run`, `onComplete`, `onError`).

## Maturity
Emerging but maintained. Created 2024-10-27 (~2 years old), 78 stars, 5 forks, 3 open
issues, actively released (v0.9.1, 2026-08-31). Backed by the Karakeep team, who use it in
a real, moderately popular self-hosted product.

## License
MIT. No commercial-use restrictions.

## Alternatives
Sidequest (already catalogued, USE NOW — Redis-free but targets Postgres/MySQL/
SQLite/Mongo with a web dashboard and multi-node coordination), BullMQ (Redis-based),
pg-boss (Postgres-based), graphile-worker (Postgres-based). Liteque is the right choice
specifically when the deployment doesn't want any database beyond SQLite and doesn't need
multi-node coordination or a dashboard.

## Risks / Limitations
- No multi-node coordination or web dashboard (unlike Sidequest) — single-process/
  single-file SQLite model only.
- Small maintainer surface relative to Sidequest; verify it still gets attention beyond
  the needs of the Karakeep app itself.

## Recommendation
**PROTOTYPE** — low-risk to try in a small service that doesn't already have Redis or
Postgres; not a replacement for Sidequest in anything that needs multi-node workers.

## Change History
### 2026-09-03
Initial discovery and review, flagged as today's Small but High-Leverage Utility. GitHub
API confirmed MIT, 78 stars, created 2024-10-27, pushed_at 2026-08-31, not archived,
latest release 0.9.1. README verified via raw.githubusercontent.com.
