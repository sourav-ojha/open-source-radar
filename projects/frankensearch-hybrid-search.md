# FrankenSearch

## Summary
Two-tier hybrid local search engine for Rust and a standalone CLI (`fsfs`): a sub-millisecond first-pass (lexical BM25 via Tantivy plus a tiny Potion-128M embedding model) followed by a slower, higher-quality refinement pass (MiniLM-L6-v2), fused with Reciprocal Rank Fusion.

## Why I Should Care
A genuinely embeddable, fully offline, two-tier hybrid search engine with a real checksum-verified installer and extensive troubleshooting documentation is unusual at this star count (86) — a clean fit for the "hidden gem" test in the discovery philosophy rather than a popularity signal. The same author's prior tool, Destructive Command Guard, is already a catalogued USE NOW pick, which raises confidence in this one's engineering quality.

## Problems It Can Remove
Removes the need to stand up Elasticsearch, Meilisearch, or a vector database for a local or embedded search feature where the corpus lives on one machine or inside one Rust process.

## Practical Uses
- Local full-text + semantic search over a personal notes/docs vault or a project's Markdown files.
- Fast local search feature inside a Rust or Tauri desktop tool.
- Search-as-a-library inside a larger Rust service needing hybrid ranking without a network dependency.
- Offline-capable search for privacy-sensitive or air-gapped local tooling.

## Product Opportunities
A local-first search feature (e.g. searching uploaded documents in a self-hosted product) without taking on a hosted search SaaS dependency.

## Agent / Automation Opportunities
None specific to agents — this is a general-purpose search engine/CLI, usable as a building block inside an agent tool but not itself agent-facing.

## Integration
Standalone `fsfs` binary via a one-line, checksum-verifying installer script, or `cargo install`; also embeddable as a Rust library (the `frankensearch` crate) inside another Rust service. Fully local/offline — no server, no external API calls. Effort: Low as a CLI, Medium to embed as a library.

## Architecture Notes
Two-tier design: Tier 1 returns results in under a millisecond using lexical BM25 (Tantivy) plus a 128M-parameter Potion embedding model; Tier 2 refines with MiniLM-L6-v2 within roughly 150ms. Results from both tiers are combined with Reciprocal Rank Fusion. The installer provisions and checksum-verifies both default models (~621MB) and refuses to silently fall back to a lower-capability profile.

## Maturity
Emerging. Created February 2026, actively pushed, tagged release v1.10.0 on crates.io. Single maintainer (Dicklesworthstone / Jeffrey Emanuel) accounting for all commits — the same author behind the already-catalogued Destructive Command Guard.

## License
MIT with a custom "OpenAI/Anthropic Rider": standard MIT terms for everyone except a named list of "Restricted Parties" — OpenAI, Anthropic, their affiliates, and anyone acting on their behalf — who are barred from using, hosting, training on, benchmarking, or otherwise incorporating the software at all. No restriction on Sourav's own use, hosting, modification, or embedding in a commercial product. Same license pattern as Destructive Command Guard.

## Alternatives
- Meilisearch/Typesense — full search servers, heavier to operate for a single-user or embedded use case.
- SQLite FTS5 — lexical only, no semantic re-ranking tier.
- Tantivy directly — FrankenSearch's lexical tier is built on it, but without the semantic refinement pass or the packaged CLI/installer.

## Risks / Limitations
- Single maintainer — full bus-factor risk.
- Custom license rider adds legal-review overhead to track separately from standard MIT dependencies, even though it doesn't restrict Sourav's own use.
- README is extremely dense (1,499 lines) with unusually granular edge-case documentation (glibc version pinning, cross-release model-index compatibility) — signals real production hardening but a steep first read.
- No Windows binary published as of v1.10.0.

## Recommendation
PROTOTYPE — worth trying as the `fsfs` CLI for a local search use case (notes vault, project docs) before considering embedding the Rust crate into a larger service.

## Change History
### 2026-09-26
Initial discovery and review.
