# Kronotop

## Summary
A distributed, transactional document database built directly on Apple's FoundationDB,
speaking the RESP2/RESP3 protocol (so existing Redis/Valkey clients work without a new
driver). Each tenant or app gets its own "namespace" — a FoundationDB key prefix that costs
almost nothing to create — and a single transaction can commit across any number of
namespaces atomically. Buckets (document collections) support a Mongo-style query language
(BQL) and an optional per-field vector index for similarity search alongside normal
document queries.

## Why I Should Care
Multi-tenant isolation is usually bolted on after the fact (a `tenant_id` column everywhere,
or one database per tenant with real provisioning cost). Kronotop pushes isolation into the
keyspace itself — the README demonstrates creating 1,000,001 namespaces in one cluster — and
still gives ACID transactions across tenants when needed (e.g. writing a tenant's order and
a shared billing counter in one commit). That is a genuinely different answer to a problem
(cheap per-tenant databases with cross-tenant transactions) that MongoDB/Postgres multi-
tenancy patterns don't solve cleanly.

## Problems It Can Remove
Hand-rolled tenant isolation logic and the operational cost of provisioning a database per
tenant, for a SaaS product that needs strict per-tenant data boundaries plus occasional
cross-tenant atomic operations (metered billing, shared counters).

## Practical Uses
- Prototype a multi-tenant SaaS data layer where each customer's data lives in its own
  namespace, with existing `redis-cli`/Valkey tooling usable for inspection.
- Store per-agent memory (one namespace per AI agent) with vector search over the agent's
  own documents, as the README itself suggests.
- Combine document storage and simple counters (via `ZMap`) in one atomic transaction —
  e.g. insert an order and increment a usage counter together.

## Product Opportunities
Worth studying for the MSSP admin-portal SaaS specifically: strict per-client data
isolation at the storage layer (not just the application layer) is exactly the property a
security-focused multi-tenant product wants to be able to point to.

## Agent / Automation Opportunities
The namespace-per-agent-memory pattern with built-in vector search is a reasonable fit for
an MCP memory server, though no official MCP server ships yet — it would need to be built
against Kronotop's RESP interface.

## Integration
Self-hosted; ships a Docker image (`ghcr.io/kronotop/kronotop`) and Maven-built JVM
binaries. Requires operating a FoundationDB cluster underneath, which is itself a
nontrivial distributed system to run (Apple-maintained, battle-tested at scale, but not a
single-binary install). Effort: **Medium-High** — the FoundationDB dependency is the real
cost, not Kronotop itself.

## Architecture Notes
FoundationDB provides the strict serializability and transaction guarantees; Kronotop
layers a document model, a Mongo-like query language, and RESP protocol compatibility on
top. This is a different tradeoff than sharding Mongo or running Postgres row-level
security: correctness comes from FoundationDB's proven transaction engine rather than
application-level enforcement.

## Maturity
**Experimental/Emerging** — explicitly labeled "developer preview" in its own README: core
architecture and transaction model are stable, but APIs and operational behavior may still
change before a first stable release. 313 stars, 6 subscribers, 12 forks — organic ratios
consistent with a small, real, pre-1.0 open-source project with a company (kronotop.com)
behind it.

## License
Apache-2.0 — no commercial-use restrictions.

## Alternatives
MongoDB with per-tenant collections/databases, Postgres with row-level security or
schema-per-tenant, FerretDB (MongoDB-wire-compatible on Postgres), EdgeDB. None of these
give FoundationDB's cross-namespace transaction guarantee at this provisioning cost.

## Risks / Limitations
Pre-1.0 and says so; running FoundationDB is a real operational commitment even though
Kronotop itself ships as a container. Small community (6 subscribers) means slower
third-party tooling and fewer answers to operational questions than an established
database.

## Recommendation
**STUDY** — the namespace-per-tenant-with-cross-tenant-transactions architecture is worth
understanding even without adopting it; not mature enough yet to put behind anything that
matters.

## Change History
### 2026-11-28
Initial discovery and review.
