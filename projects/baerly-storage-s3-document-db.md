# baerly-storage

## Summary
A document database with no server, daemon, or database runtime — the entire execution layer
runs as library code inside your own request handler, and the data lives directly in an S3/R2
bucket you already own. Commits rely on S3's strong read-after-write consistency plus one
conditional create (`If-None-Match`) as the sole load-bearing operation. Ships a small TypeScript
API (~8 verbs) and a React `useQuery` hook.

## Why I Should Care
This directly matches the AWS S3 surface already in use and removes a whole category of
infrastructure — a docker-compose.yml, migrations, a connection pool, a pager rotation — for
exactly the class of small, single-collection internal tools and MVPs that make up a large share
of micro-SaaS experiments.

## Problems It Can Remove
- Standing up a managed Postgres/Mongo instance (and its connection pool, migrations, and on-call
  burden) for a small internal tool that only ever needs one collection
- The idle-cost floor of a resident database process for a tool nobody is actively using

## Practical Uses
- Small internal tools or single-tenant features — a kanban board, an event's RSVP list, one
  channel's chat — that don't justify a full database server
- Micro-SaaS MVPs where a managed-database bill isn't yet justified; graduate via
  `baerly export --target=postgres` once it is
- Agent-built software where an LLM needs a database API small enough (~12k tokens) to hold in
  context without hand-writing SQL or migrations
- Idle-cost-sensitive internal admin tools, since no resident process means zero cost when nobody
  is using the tool

## Product Opportunities
A credible "start here, graduate later" pattern for the idea-to-shippable-product path this radar
is built around — ship the MVP on baerly-storage, export to Postgres only once a feature actually
needs joins or cross-tenant queries.

## Agent / Automation Opportunities
The API surface is deliberately small enough to hand to an LLM and walk away — no DDL, no raw
SQL, 8 verbs — making it a plausible backend for agent-generated internal tools where a model
needs to both write and reason about the data layer.

## Integration
`pnpm create @gusto/baerly-storage@latest` scaffolds a project; runs on Cloudflare Workers + R2 or
AWS Lambda + S3. **Integration effort: Low** — no server to provision, though the design is
narrow by intent (see Risks) and the fit should be checked against the "can the app's most
important work be done from one collection?" test the project itself poses.

## Architecture Notes
The commit protocol is the interesting part: a write drops immutable objects into the bucket, then
creates the next numbered log entry for that collection with create-if-absent. Two writers racing
the same slot cannot both win; the loser reads the winner and retries at the next slot — that
create *is* the commit, with no resident coordinator process required. Shares its immutable-
artifact lineage with table formats like Iceberg and Delta Lake, but commits with a narrower step
(no metadata-pointer swap, no separate coordinator).

## Maturity
Experimental. Created May 2026, v0.7.0, 25 stars, 0 subscribers, 2 forks. Published by Gusto
(payroll/HR platform) engineering with an accompanying engineering-blog rationale, which adds
credibility beyond a typical solo side project — but the zero-subscriber count makes it unclear
whether this is a maintained open-source commitment or a one-off artifact from that blog post.

## License
Apache-2.0. Unrestricted for commercial use, modification, and redistribution.

## Alternatives
Firebase/Supabase/Convex (all require a running backend service), Cloudflare D1 (a real SQL
database, heavier setup), hand-rolled S3 JSON blobs (no commit/consistency guarantees) — the
project's own docs explicitly compare itself against the first three rather than leaving that to
the reviewer.

## Risks / Limitations
- No SQL, no joins — explicitly not a general-purpose database; the wrong fit the moment a screen
  needs a cross-collection view ("my pull requests," a cross-org dashboard)
- Writes are per-collection linearizable only; cross-collection writes are unordered and
  non-atomic by design, not a bug to be fixed later
- Very early, with zero subscribers despite corporate backing — unclear long-term maintenance
  commitment
- Realtime is long-poll first; no WebSocket tier yet

## Recommendation
PROTOTYPE — a strong fit to trial on the next small internal tool or admin utility that would
otherwise get its own throwaway Postgres instance; not a fit for anything needing cross-collection
queries.

## Change History
### 2026-11-26
Discovered via GitHub search (slot 2: product infrastructure, APIs, backend components). First
catalog entry.
