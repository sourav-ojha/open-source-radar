# TypeGraph

## Summary
TypeScript-first embedded knowledge-graph library on top of an existing SQLite or
PostgreSQL database. Nodes/edges defined with Zod schemas, queried via a fluent
type-safe API, with hybrid vector+fulltext search (pgvector, sqlite-vec, libSQL/Turso
native vectors), runtime schema evolution from agent-proposed JSON, and bitemporal
(valid-time + recorded-time) reads.

## Why I Should Care
The only entrant found today that brings graph semantics (ontology relations,
bitemporal history, typed traversal) natively into a TypeScript/SQL stack rather than
requiring a separate graph database — a direct fit for a MERN/Postgres profile.

## Problems It Can Remove
Hand-rolled adjacency tables and recursive SQL for relationship-heavy domain models;
standing up a dedicated graph database service for a feature that doesn't need one.

## Practical Uses
- Model a knowledge graph or RAG context store directly inside an existing Node/Postgres
  or SQLite app.
- Build auditable agent memory with decision replay via bitemporal reads (what did the
  agent believe at time T, versus what we now know).
- Model relationship-heavy domains (identity/permissions, ontologies) with
  compile-time-checked traversal queries.
- Let an agent evolve the graph schema at runtime from proposed JSON instead of
  requiring a redeploy.

## Product Opportunities
A reusable agent-memory/RAG-context building block for a Node/TS product that avoids
standing up a dedicated graph database.

## Agent / Automation Opportunities
Runtime schema evolution from agent-proposed JSON is explicitly designed for
agent-driven data modeling rather than human-authored migrations.

## Integration
npm package (`@nicia-ai/typegraph`), embeds directly into an existing SQLite or
PostgreSQL database. Low effort.

## Architecture Notes
Supports fixed and recursive graph traversal with per-hop constraints and
completed-match filters, plus bitemporal reads for reconstructing historical state —
notably more graph-theory-aware than a typical "adjacency table plus recursive CTE"
approach.

## Maturity
Experimental: created 2026-02-08 (~9 months), 93 stars, 3 forks, 1 subscriber, 37 open
issues. Actively released — v0.76.0 published the same day as this review
(2026-11-21). No independent production-usage evidence beyond the project's own docs.

## License
MIT — no restrictions.

## Alternatives
Neo4j/Dgraph/NebulaGraph — dedicated graph databases, more capable at scale but a
separate service to run and operate. Hand-rolled adjacency tables in Postgres — what
this replaces for a TypeScript team, at the cost of writing and maintaining the
traversal/ontology logic by hand.

## Risks / Limitations
Very early and thin community (93 stars, 1 subscriber); a solo/small-team project with
an actively evolving API.

## Recommendation
PROTOTYPE — worth a small spike modeling one real relationship-heavy feature (e.g.
agent memory or a permissions graph) before relying on it for anything load-bearing.

## Change History
### 2026-11-21
First reviewed and catalogued.
