# ParadeDB

## Summary
A Postgres extension suite ("Just use Postgres") that adds BM25-scored full-text search,
vector retrieval, aggregations, facets, and joins directly inside Postgres, so application
data and search live in one database with standard SQL and ACID transactions instead of
Postgres plus a bolted-on Elasticsearch/OpenSearch cluster.

## Why I Should Care
Any hybrid-search RAG setup eventually needs keyword ranking (BM25) alongside vector
search. Postgres's built-in full-text search is weak at real ranking; the usual fix is
standing up a second system (Elasticsearch, Typesense, Algolia) and keeping it in sync.
ParadeDB puts a real BM25 engine — Block-Max WAND, top-k, facets — in the same database
the application data already lives in.

## Problems It Can Remove
Removes the operational burden of running and syncing a second search cluster for
products where Postgres is already the system of record. Removes the need to hand-build
BM25 ranking on top of Postgres's native `tsvector` search, which lacks proper relevance
scoring at scale.

## Practical Uses
- Add BM25-ranked full-text search plus vector/hybrid retrieval to an existing Postgres
  database without standing up Elasticsearch/OpenSearch.
- Run RAG retrieval entirely in Postgres: keyword + vector search, filters, facets, and
  joins against the same tables as the application data.
- Replace a bolted-on Algolia/Typesense/Elasticsearch integration for a product whose data
  already lives in Postgres.
- Self-host on an existing Kubernetes cluster via CloudNativePG for an EKS-based
  deployment.

## Product Opportunities
Removes a second search-infra dependency for a micro-SaaS or admin panel that needs
search/facets but doesn't want to operate two systems. Candidate retrieval backend for a
RAG feature inside a commercial product, with the AGPL hosting caveat priced in up front.

## Agent / Automation Opportunities
No MCP server; accessed purely through SQL over the standard Postgres wire protocol, so
any agent or tool that already talks to Postgres can use it with no new interface to
learn.

## Integration
Deployment: Docker, Kubernetes (CloudNativePG operator), self-hosted (add the extension to
an existing Postgres cluster), PaaS (Railway, Render, Fly.io, Dokku, DigitalOcean), BYOC, or
managed ParadeDB Cloud. Interfaces: SQL only (the `@@@` BM25 operator and vector operators).
Integration effort: **Medium** — it's a Postgres extension, which means managing a custom
Postgres build or extension loading rather than dropping in a sidecar library.

## Architecture Notes
Built as a native Postgres extension (not a layer on top of Postgres like most
"Postgres + search" products) — the index lives inside Postgres's own storage and
transaction model, so writes to application tables and the search index stay consistent
without a separate sync pipeline. Worth studying as a reference for how to implement a
real BM25 index (not `tsvector`) as a C extension with parallel index builds.

## Maturity
Mature. Created 2023-06-30 (~3.3 years), 9,511 stars, 512 forks, 43 subscribers, active
Slack community, Docker Hub images, regular releases (latest v0.26.6, 2026-11-08). Not
archived.

## License
**AGPL-3.0 — flag loudly.** Self-hosting or internal-only use is unaffected, but offering
ParadeDB itself as a network-accessible hosted service to third parties triggers AGPL's
source-disclosure requirement unless a commercial license is purchased from ParadeDB.
ParadeDB Cloud exists as the managed path for teams that want to avoid the question
entirely.

## Alternatives
- pg_textsearch (catalogued) — lighter, BM25-only, permissively licensed, no vector search.
- pgvector — vector-only, de facto standard, no BM25 text ranking.
- Elasticsearch/OpenSearch/Typesense/Algolia — separate infrastructure to operate and keep
  in sync with Postgres.
- Infino (catalogued) — embedded Parquet-backed retrieval library, a library not a
  database extension.

## Risks / Limitations
- AGPL-3.0 — do not deploy as a hosted service for third parties without a commercial
  license; read the license before any SaaS decision that involves exposing it over a
  network to customers.
- A Postgres extension, not a drop-in library — more operational surface than a sidecar
  service.
- No Docker/bare-metal self-hosting path is documented on the main deploy-overview page;
  self-hosting means Kubernetes via CloudNativePG or adding the extension to a cluster
  already being operated.

## Recommendation
PROTOTYPE — stand it up against a non-production Postgres database with a real
full-text+vector workload and compare the BM25 ranking quality against the current setup
before deciding whether it earns a production role.

## Change History
### 2026-11-14
Initial discovery and review. GitHub API verified: AGPL-3.0, 9,511 stars, 512 forks, 43
subscribers, created 2023-06-30, pushed_at 2026-11-14, not archived. Self-hosting confirmed
via paradedb.com/docs/operate/deploy/overview.
