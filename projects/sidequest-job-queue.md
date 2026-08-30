# Sidequest

## Summary
A Redis-free background job processor for Node.js. Jobs are persisted durably in a database you already run — PostgreSQL, MySQL, SQLite, or, notably, MongoDB via a first-party backend package — instead of Redis. Multiple nodes claim jobs atomically against the shared database, workers execute in worker threads, and it ships a built-in web dashboard, cron scheduling, retries with backoff, and job uniqueness. Explicitly positioned as a production-grade BullMQ and pg-boss alternative.

## Why I Should Care
This removes Redis as a hard dependency for background jobs on a MERN stack. Any project already running MongoDB can add durable job processing without provisioning ElastiCache or a Redis container purely to satisfy BullMQ.

## Problems It Can Remove
- A standalone Redis instance stood up only to run BullMQ
- Hand-rolled polling/locking logic for a Mongo-backed job table
- Bull-board or a custom dashboard for job visibility

## Practical Uses
- Background email, webhook, and report-generation jobs in an Express or NestJS backend
- Scheduled/cron jobs that must survive process restarts and run across multiple app instances
- Migrating an existing BullMQ codebase (official migration guide provided)

## Product Opportunities
- Job backbone for a micro-SaaS product where a managed Redis instance is avoidable infra cost
- Could back scan orchestration or admin-portal background tasks for the MSSP SaaS using the existing MongoDB instance instead of adding Redis

## Agent / Automation Opportunities
CLI tooling for migrations and management; the web dashboard's HTTP surface could feed job status into an internal automation or agent tool.

## Integration
`npm install sidequest` plus one backend driver package (`@sidequest/mongo-backend`, `@sidequest/postgres-backend`, etc.). Requires Node.js >= 22.6.0. Does not run on Bun yet (tracked as open issue #72). TypeScript jobs run natively on Node >= 23.6.0.

## Architecture Notes
Atomic job claiming per backend (e.g. `SELECT ... FOR UPDATE SKIP LOCKED` on Postgres/MySQL) is what makes multi-node distribution safe without Redis. The engine runs in a forked child process with jobs executing in worker threads, with inline/no-fork execution modes also available for simpler deployments.

## Maturity
Emerging, bordering mature: created July 2025 (~1 year of history), actively released (v1.16.3, 2026-08-27), published to npm, CI badge present. Smaller community (1,012 stars, 23 forks) than BullMQ.

## License
LGPL-3.0-or-later. Safe to use as an unmodified npm dependency in proprietary/commercial products — LGPL only requires releasing modifications to Sidequest's own source if those modified files are distributed. Does not obligate open-sourcing the application using it.

## Alternatives
BullMQ + Redis, pg-boss, Graphile Worker, Agenda (older, less actively maintained MongoDB job scheduler).

## Risks / Limitations
- No Bun support
- SQLite backend explicitly not recommended for production
- Younger and smaller community than BullMQ; worth a small pilot before full migration

## Recommendation
USE NOW — for any new Node.js service on this stack that needs background jobs, especially where MongoDB is already the database and adding Redis is avoidable.

## Change History
### 2026-08-30
Initial discovery and review.
