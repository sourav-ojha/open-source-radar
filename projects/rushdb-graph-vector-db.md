# RushDB

## Summary
Graph + vector database and "memory layer" for AI agents, built on Neo4j. Push arbitrary JSON and get back typed, searchable, relationship-aware records with no schema or migrations. Ships a JS/TS SDK, REST API, and an MCP server.

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
Docker Compose provided (bundles Neo4j). Requires running Neo4j, which a MERN-stack team doesn't already operate. Medium effort.

## Architecture Notes
Built entirely on top of Neo4j — worth studying as an example of layering a schema-inference and typed-record API over a graph database rather than building a bespoke storage engine.

## Maturity
Emerging — created December 2024, 323 stars, active (latest MCP server release 2026-08-31).

## License
**Split — flag loudly.** `platform/core` and `platform/dashboard` (the actual database engine and admin dashboard) are licensed under Elastic License 2.0 (source-available, not OSI-approved). Docs, website, and the JavaScript SDK are Apache-2.0.

## Alternatives
Catalogued mex (different niche — living codebase wiki), Zep, Memgraph-based agent-memory stacks.

## Risks / Limitations
ELv2 permits self-hosting and embedding RushDB inside your own product freely, but specifically forbids offering RushDB itself as a hosted/managed database service to third parties — read the license before any commercial deployment decision that involves reselling it. Hard dependency on Neo4j.

## Recommendation
PROTOTYPE — interesting architecture for agent memory, but the Neo4j dependency and split license warrant testing in a low-stakes project first.

## Change History
### 2026-09-12
Initial discovery and review.
