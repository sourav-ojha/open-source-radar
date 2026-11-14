# RushDB

## Summary
Self-describing, graph-native database for AI agents. As of v3.0 (2026-10-28), rewritten
from a Neo4j-backed layer to a native Rust storage engine (LMPG) with Postgres-wire, Bolt,
and gRPC protocol support, TLS, and tenant-bound capability tokens. Push arbitrary JSON and
get back typed, searchable, relationship-aware records with no schema or migrations. Ships
a JS/TS SDK, REST API, and an MCP server.

## Why I Should Care
Combines graph relationships and vector search behind one JSON-in/JSON-out API — most agent-memory stores pick one or the other. Could replace a combination of ad-hoc MongoDB collections plus a separate vector DB for agent-facing data.

## Problems It Can Remove
Removes the need to hand-roll schema-inference-on-write and relationship extraction from raw JSON, and removes the need to run separate systems for structured storage, graph relationships, and vector search.

## Practical Uses
- Schema-less structured memory store for an AI agent — push JSON, get relationships back for free.
- Replace ad-hoc MongoDB collections plus a separate vector DB with one relationship-aware store.
- Power a "record explorer" admin UI without hand-writing migrations for every new data shape.

## Product Opportunities
Candidate backing store for agent-memory features in his own products without standing up Neo4j, a vector DB, and a graph layer separately.

## Agent / Automation Opportunities
Ships an MCP server — usable directly as an agent's structured memory backend.

## Integration
Single Rust binary, no external graph database required as of v3.0. Docker image
available; speaks Postgres wire protocol, Neo4j Bolt protocol, and gRPC directly. Low
effort — the prior Neo4j dependency that made this Medium effort is gone.

## Architecture Notes
Through v2.x, built entirely on top of Neo4j. v3.0 (2026-10-28, "RushDB 3.0 — Rust engine
(LMPG)") replaced that with a native Rust storage engine exposing the same data model over
multiple wire protocols (Postgres, Bolt, gRPC) — worth studying as an example of a project
graduating from "API layer over an existing database" to its own storage engine without
breaking its external data model.

## Maturity
Emerging — created December 2024, 347 stars, 29 forks, 2 subscribers. Rust engine is three
weeks old at time of review (v3.0 shipped 2026-10-28) with near-daily patch releases since
(v3.0.10 by 2026-11-13).

## License
**Apache-2.0** (simplified from the prior split license). v3.0 retired the Neo4j-backed
`platform/core`/`platform/dashboard` that had been licensed under Elastic License 2.0;
the whole repository is now a single Apache-2.0 license. No restrictions on commercial
use, SaaS deployment, or embedding — the prior ELv2 hosted-service caveat no longer
applies.

## Alternatives
Catalogued mex (different niche — living codebase wiki), Zep, Memgraph-based agent-memory
stacks.

## Risks / Limitations
The Rust engine (v3.0) is three weeks old at time of review — audit durability/correctness
claims directly before trusting it with anything production-sensitive, despite the
architecture improvement. Small project (347 stars, 2 subscribers) — verify
upgrade stability release-to-release given the near-daily release cadence.

## Recommendation
PROTOTYPE — the Neo4j dependency and split license that warranted caution before are both
gone; worth a fresh, low-stakes trial of the new Rust engine directly, with attention to
how young it is.

## Change History
### 2026-09-12
Initial discovery and review.

### 2026-11-14
v3.0 (2026-10-28) replaced the Neo4j-backed platform with a native Rust engine (LMPG),
dropped the split Elastic License 2.0 / Apache-2.0 licensing for a single Apache-2.0
license across the repo, and added Postgres-wire, Bolt, and gRPC protocol support plus TLS
and tenant-bound capability tokens. This resolves both limitations ("hard dependency on
Neo4j" and the split license) the initial review flagged — a meaningful architecture and
licensing update.
