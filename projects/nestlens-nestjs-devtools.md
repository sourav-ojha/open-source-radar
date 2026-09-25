# NestLens

## Summary
Laravel Telescope-inspired debugging and monitoring dashboard for NestJS. A single
module import gives a NestJS app a real-time web dashboard tracking requests, database
queries, exceptions, jobs, cache, Redis, scheduled tasks, events, batches, mail,
outgoing HTTP calls, notifications, gates, commands, views, dumps, and GraphQL — 16
watchers in total, most requiring zero configuration.

## Why I Should Care
NestJS is explicitly part of Sourav's core stack. NestLens is a one-line-import
replacement for the request/query/exception debugging dashboard he would otherwise
hand-build per project — the exact kind of "not worth my time re-building" utility
this radar exists to catch.

## Problems It Can Remove
Removes the need to build or bolt together: request logging middleware, a query
interceptor for TypeORM/Prisma, an exception-tracking layer, a job/queue monitor for
Bull, and a dashboard UI to view all of it. Also removes ad hoc `console.log`
debugging during local development.

## Practical Uses
- Drop into any NestJS service (including microservices in a fleet) for immediate
  request/query/exception visibility.
- Leave gated behind IP whitelist + role-based access in staging for team debugging.
- Use the automatic data-masking feature to keep sensitive fields out of the dashboard
  even when enabled outside pure local dev.
- Track background job/queue health (Bull) and scheduled task execution without a
  separate observability stack.

## Product Opportunities
Reusable internal debug/admin tooling for any NestJS-based product, including the
MSSP admin-portal SaaS — avoids building a bespoke observability panel per project.

## Agent / Automation Opportunities
None specific to AI agents — this is a human-facing debugging dashboard, not an
MCP/CLI surface.

## Integration
`npm install nestlens`, then `NestLensModule.forRoot({...})` in the root module.
Visit `/nestlens`. Express and Fastify, and TypeORM queries, GraphQL servers, and
scheduled tasks are auto-detected; Prisma, Bull, and Redis each need one line of
config. Low integration effort.

## Architecture Notes
Watcher-based design (one watcher class per concern: request, query, exception, job,
etc.), each independently togglable. REST API served under `/nestlens/__nestlens__/api/*`
as of v0.6.0, kept separate from the app's own interceptors/routes to avoid interfering
with production traffic when accidentally left enabled.

## Maturity
Emerging. v0.14.2, pre-1.0, with documented breaking changes across the 0.4.0 → 0.6.0 →
0.8.0 upgrade path. Single primary maintainer (354 of ~440 commits), automated releases
via semantic-release. Active: last push 2026-09-24.

## License
MIT. No restrictions on commercial or embedded use.

## Alternatives
- Laravel Telescope (the direct inspiration, PHP-only, not usable from Node).
- Hand-rolled Winston/Pino logging plus a custom dashboard.
- Sentry/Datadog APM — paid, heavier, and not built around per-request/per-query
  inline tracing the way Telescope-style tools are.

## Risks / Limitations
- Single maintainer — bus-factor risk.
- Pre-1.0 with real breaking changes between minor versions; pin the version and read
  the upgrade guide before bumping.
- Must be explicitly gated before any production exposure — the documented quick-start
  defaults to non-production only (`enabled: process.env.NODE_ENV !== 'production'`).

## Recommendation
USE NOW — low integration effort, direct stack match (NestJS), and immediately useful
for local/staging debugging with low downside given the MIT license and self-contained
deployment (no separate service to run).

## Change History
### 2026-09-25
Discovered via GitHub topic search (`topic:debugging`) during slot 3 (developer
utilities, debugging, testing) discovery. Catalogued as USE NOW, 8.6/10.
