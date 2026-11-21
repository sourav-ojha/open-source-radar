# SeekStorm

## Summary
Sub-millisecond lexical, vector, and hybrid search engine in Rust. Embeddable as an
in-process library, runnable as a multi-tenant server, and exposable to agents via
MCP. Real community traction (245-point Hacker News thread, active Discord,
crates.io/docs.rs distribution, Docker images).

## Why I Should Care
A lighter-weight self-hosted search building block than Elasticsearch/OpenSearch for
product-building work that needs hybrid full-text + vector search but not distributed
cluster scale.

## Problems It Can Remove
Standing up and operating Elasticsearch/OpenSearch for a workload that doesn't need
that much infrastructure; writing a bespoke BM25+vector fusion layer by hand.

## Practical Uses
- Self-hosted hybrid search behind a product feature without an Elasticsearch
  deployment.
- Embed search directly into a Rust service as an in-process library.
- Give an agent fast hybrid search over a document/code corpus via the MCP interface.
- Multi-tenant deployment for a SaaS product needing per-customer search indices.

## Product Opportunities
A lighter self-hosted search building block for a micro-SaaS than
Elasticsearch/OpenSearch, with less operational overhead.

## Agent / Automation Opportunities
Native MCP support for giving coding/research agents hybrid search over a corpus.

## Integration
Rust crate (`cargo add seekstorm`) for in-process embedding, or the packaged server via
Docker for a standalone deployment. Low effort; non-Rust consumers use the REST API or
MCP server rather than a native binding.

## Architecture Notes
In-process library and multi-tenant server share the same core engine — the same code
path serves both an embedded single-app use case and a hosted multi-tenant one.

## Maturity
Emerging with real independent validation: created 2024-02-27 (~21 months), 1,938
stars, 75 forks, 9 subscribers, 13 open issues, active Discord, front-page Hacker News
thread. Latest release v5.1.0 (2026-11-15).

## License
Apache-2.0 — no restrictions on commercial use, SaaS deployment, or embedding.

## Alternatives
Meilisearch/Typesense — more mainstream, broader out-of-the-box feature set, but
heavier runtimes than an embeddable Rust library. Elasticsearch/OpenSearch — far more
mature but substantially more operational overhead for a workload that doesn't need
distributed scale.

## Risks / Limitations
Smaller community than Meilisearch/Typesense; longevity and support responsiveness
unverified beyond the GitHub activity itself. Rust-only embedding path.

## Recommendation
PROTOTYPE — try the REST/MCP server against one real search use case before
committing to the Rust embedding path.

## Change History
### 2026-11-21
First reviewed and catalogued.
