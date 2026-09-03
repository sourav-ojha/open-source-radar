# Rebase

## Summary
Rebase is a self-hosted, Postgres-native backend-as-a-service: point it at a Postgres
database and get REST APIs, auth, S3-compatible storage, realtime, backups, and row-level
security, with a schema-driven admin panel and an MCP server layered in only if wanted.
Ships as modular npm packages across three adoption modes — BaaS, CMS, Full.

## Why I Should Care
It targets the same job as Supabase but stays modular and Postgres-only: no bundled
control plane, no forced admin UI, no dependency on a dozen containers to get REST + auth
+ storage. The "adopt only what you want" positioning matters for a small team that
doesn't want to operate a full Supabase stack just to get auto-generated REST endpoints
over an existing database.

## Problems It Can Remove
Removes hand-writing REST/auth/storage/realtime glue on top of a Postgres database for a
new product or internal service, and removes the operational overhead of running a full
Supabase self-hosted stack (Kong, GoTrue, PostgREST, Storage, Realtime, postgres-meta, Studio)
when only a subset is actually needed.

## Practical Uses
- Stand up a Postgres-backed REST + auth + storage layer for a new micro-SaaS MVP without
  hand-rolling any of the three.
- Use the typed SDK generation (`@rebasepro/codegen`) to keep frontend and backend types in
  sync automatically as the schema evolves.
- Run the static RLS audit (`@rebasepro/rls-check`) before trusting row-level security on a
  customer-facing table.
- Point an AI coding agent at a live project through the built-in MCP server for schema
  discovery and migrations instead of hand-writing SQL migrations.

## Product Opportunities
A lighter, Postgres-only BaaS layer is a plausible foundation for spinning up new
micro-SaaS ideas faster — the pitch ("own your data, own your code," no proprietary
control plane) is also a sellable feature if a product built on it needs to tell customers
their data isn't locked into a vendor's hosted plane.

## Agent / Automation Opportunities
Built-in Model Context Protocol server (`@rebasepro/mcp`) exposes schema discovery, entity
management, and data migrations to AI coding agents directly against a live database —
this is a first-party agent-tool interface, not a bolted-on wrapper.

## Integration
`docker compose up -d db` plus `rebase dev`/`rebase db push` via the CLI; npm package
`@rebasepro/app`. Self-hosted only (no hosted-cloud option mentioned). **Integration
effort: Medium** — modular adoption keeps footprint small, but it is still new
infrastructure to run and operate versus a pure library.

## Architecture Notes
Three modes share the same packages wired differently: **BaaS** (REST + auth + storage +
realtime + backups, no admin UI, no React in the dependency tree), **CMS** (BaaS + a
schema-driven admin UI from collection definitions, comparable to Payload/Directus), and
**Full** (CMS + Studio: SQL editor, schema visualizer, RLS editor). Admin panel and Studio
are opt-in layers, not the default.

## Maturity
Early/emerging. Created 2026-03-30 (~5 months old), 8 stars, 1 fork, 7 open issues, but
active development — release v0.17.3 shipped 2026-08-31, frequent 0.x releases visible in
the tag history. Has CI badge, npm downloads badge, and a Discord, suggesting more than a
weekend project, but still pre-1.0 and low community size.

## License
MIT. No commercial-use restrictions identified.

## Alternatives
Supabase self-hosted (heavier stack, ~12 containers even for a single project — see
Powabase's architecture diagram for a concrete example of that footprint), PocketBase
(single-binary but SQLite-based, not Postgres-native), Directus and Payload CMS
(CMS-first rather than BaaS-first), NHost. Rebase differs by staying Postgres-only, modular
by default, and MCP-native out of the box.

## Risks / Limitations
- Pre-1.0 (0.17.x), 8 stars — genuinely early; treat as an experiment, not infrastructure
  to depend on for anything production-critical yet.
- Small community (1 fork, 7 open issues) means slower third-party bug discovery than an
  established alternative.
- Self-hosted only; no managed-cloud fallback if operating it becomes a burden.

## Recommendation
**PROTOTYPE** — worth standing up for a small side project or internal tool to evaluate
the BaaS-mode REST/auth/storage layer and the MCP server hands-on before considering it
for anything customer-facing.

## Change History
### 2026-09-03
Initial discovery and review. GitHub API confirmed MIT, 8 stars, created 2026-03-30,
pushed_at 2026-09-02, not archived, latest release v0.17.3. README verified via
raw.githubusercontent.com, including the modular-architecture table and MCP server
section.
