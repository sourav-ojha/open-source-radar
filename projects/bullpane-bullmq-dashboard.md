# Bullpane

## Summary
A self-hosted dashboard for BullMQ (and BullMQ Pro) queues, built to be safe to point at a production Redis: bounded `SCAN` instead of `KEYS`, Lua-script reads (one round trip per query), in-Redis payload truncation, and a resumable cursor-based job search. Supports Redis, Redis Cluster, Valkey, and BullMQ 6's newer PostgreSQL backend. Open-core: the dashboard core is MIT and free; a USD 39/month Pro tier adds auth, roles, and an audit log.

## Why I Should Care
If he's running BullMQ anywhere in his Node stack (a natural fit given his Node/Express/MERN background), `bull-board` is the usual free option but has no search-inside-job-data, no roles, and no alerting, and Taskforce.sh is a paid hosted product that means job payloads transit a third party's servers. Bullpane runs in his own container next to his own Redis, is free for the core read/retry/promote/delete workflow, and is explicit that the free edition ships with **no authentication at all** — worth knowing before exposing it on anything but a private network.

## Problems It Can Remove
Writing ad hoc scripts to inspect stuck/failed BullMQ jobs; `bull-board`'s lack of search and alerting; paying for or routing job payloads through a third-party hosted queue dashboard.

## Practical Uses
- Point-and-click inspection of queues, jobs, failures, progress, logs and BullMQ flows during local development or production debugging.
- Searching inside job payload data for a specific identifier across a large queue, which `bull-board` does not support.
- Monitoring a BullMQ-on-PostgreSQL deployment (BullMQ 6) through the same UI, without a separate tool for the Postgres-backed variant.
- A quick `npx bullpane` session against a staging Redis to retry or clean up a batch of failed jobs.

## Product Opportunities
None directly — this is an internal ops tool, not a component to embed in a product.

## Agent / Automation Opportunities
Not agent-specific, but the same bounded-SCAN/Lua-script read pattern it uses to stay safe against a production Redis is a good template for building any other read-only inspection tool (agent-facing or not) against a live production datastore without risking a `KEYS`-style full scan.

## Integration
`npx bullpane` against an existing Redis connection string, or a single container ("point it at the Redis your workers already use"); `npx bullpane --postgres postgres://...` for the BullMQ-6-on-Postgres mode. No first-run wizard, no account for the free tier.

## Architecture Notes
All Redis reads are owned by one internal package (`packages/redis-inspector`) enforcing: no `KEYS` ever (bounded, cached `SCAN` only); one round trip per read via `EVALSHA` Lua scripts, with multi-queue reads pipelined; payloads truncated inside Redis via `string.sub` rather than fetched whole and truncated client-side; cluster-safe scripts that each touch keys of exactly one queue (single hash tag); a 5-second connect timeout with one retry and no offline queue, so a dead Redis shows as a red badge instead of hanging the dashboard. For the Postgres mode, it reads BullMQ's own schema directly (one SQL statement per read, partial indexes, SQL-side truncation) and never runs migrations.

## Maturity
Emerging. Created 2026-09-06, v0.7.1 released 2026-10-07, 53 stars / 4 forks / 2 subscribers / 5 open issues. Small but focused and recently active (pushed within the last week).

## License
Open core: MIT for the free dashboard core; a commercial license applies to code under any `ee/` directory (the Pro team features — auth, roles, audit log). Releases up to and including 0.3.0 were fully MIT. Standard, non-restrictive split for a solo/small-team open-core project.

## Alternatives
bull-board (free, but no search-inside-job-data, no roles, no alerts); Taskforce.sh (hosted, priced per connection, job payloads transit their servers).

## Risks / Limitations
Free edition has zero authentication — must be kept on a private network or behind the operator's own auth proxy, or upgraded to Pro for built-in login. Small team/early project; no independent production track record beyond the author's own claims.

## Recommendation
USE NOW, as a drop-in replacement for bull-board wherever he's running BullMQ, with the explicit caveat to keep it off any publicly reachable network given the free tier's lack of auth.

## Change History
### 2026-10-29
Initial discovery and review. Slot 2 run (product infrastructure, APIs, backend components), Small but High-Leverage Utility of the day. Found via GitHub Search API `topic:job-queue created:>2026-06-01`.
