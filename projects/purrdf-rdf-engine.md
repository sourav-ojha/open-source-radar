# PurRDF

## Summary
A single Rust RDF 1.2 engine, shared across Rust, Python, JavaScript/WebAssembly, and C,
that answers BM25 full-text search, exact GeoSPARQL 1.1 topological queries, and exact
or approximate embedding kNN inside the same SPARQL evaluator over one dataset — plus
SHACL/ShEx validation and Datalog/OWL reasoning — with deterministic fixed-point
arithmetic and a published, DOI-citable design.

## Why I Should Care
It is a rare example of academically rigorous infrastructure software solving three
retrieval problems — text, geo, and vector — inside one typed SPARQL evaluator, instead
of the usual Postgres + pgvector + PostGIS combination run as three separate subsystems.
Worth understanding as an architecture reference even though its practical npm/Python
surface is deliberately narrower than the Rust one.

## Problems It Can Remove
Running Postgres + pgvector + PostGIS as three subsystems to get text, vector, and
spatial retrieval together; reimplementing RDF validation/reasoning per language instead
of sharing one engine.

## Practical Uses
- Study the fused SPARQL retrieval model (text + GeoSPARQL + embedding kNN with
  reciprocal-rank fusion and per-producer evidence) as a design reference
- Build a Rust-hosted knowledge-graph application needing RDF + SHACL/ShEx validation +
  OWL/Datalog reasoning + retrieval in one dependency
- Use the lighter npm/Python packages for RDF data-shape and path-witness work where the
  full retrieval fusion isn't needed

## Product Opportunities
None identified — this is infrastructure/architecture, not a product-embeddable
component for a typical MERN stack today.

## Agent / Automation Opportunities
The browser WASM build could back an in-browser, no-server knowledge-graph explorer or
agent-memory demo.

## Integration
Rust crate via crates.io (full feature surface); Python package via PyPI; npm package
(`@blackcatinformatics/purrdf`); WebAssembly in the browser; C via FFI. The npm and
Python packages intentionally expose only data-shaped property functions and path
witnesses, not the full text/geo/vector producer registration — that requires the Rust
host. Medium integration effort for anything beyond the thin bindings.

## Architecture Notes
A Rust host can join graph patterns with text search, spatial relations, and embedding
neighbours in the same SPARQL evaluator by registering each relation under a predicate
IRI. BM25 scores use exact fixed-point arithmetic; GeoSPARQL implements exact rational
WKT/GeoJSON topology; vector retrieval supports exact kNN plus an approximate
deterministic-level HNSW index that never falsely certifies no nearer row exists.
Reciprocal-rank fusion carries plan, fusion-law, and evidence identities for
explainability.

## Maturity
Experimental. Created 2026-07-01 (~4 months old). 52 stars, 2 forks, 28 open issues at
review time. Backed by Blackcat Informatics Inc. with a DOI-registered release, a hosted
docs "book," a browser playground, and CI — unusually polished engineering for its age,
but still very early in real-world adoption.

## License
Triple-licensed: MIT OR Apache-2.0 OR MulanPSL-2.0, caller's choice. All permissive, no
commercial restrictions.

## Alternatives
RDFLib/Apache Jena for RDF-only work (no built-in BM25/vector/geo fusion); a Postgres +
pgvector + PostGIS combination for the same three retrieval modes, at the cost of
running three subsystems instead of one.

## Risks / Limitations
- Very young with modest traction — treat as an architecture study, not production
  infrastructure, until it accumulates more real-world use
- The npm and Python packages deliberately do not expose the text/geo/vector retrieval
  fusion that makes the Rust core interesting, limiting direct value for a Node-first
  stack
- A different, unrelated project (ThemisDB) found the same day in an adjacent search
  carried a Kickstarter funding link in its README — unrelated to PurRDF, but a reminder
  to keep verifying claims independently in this space

## Recommendation
STUDY — the architecture is worth understanding and potentially borrowing ideas from,
but direct adoption on a Node/React stack is unnecessary today given the narrower
non-Rust bindings.

## Change History
### 2026-10-31
Initial discovery and cataloguing (slot 4: data/search/documents/RAG).
