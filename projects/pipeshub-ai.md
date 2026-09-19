# PipesHub AI

## Summary
Open-source platform connecting enterprise knowledge sources (drives, wikis, chat, email, etc.) to AI agents with permission-aware retrieval — search results and agent answers are scoped to the querying user's actual access rights, with citations back to source.

## Why I Should Care
Permission-aware retrieval (not just retrieval) is the part most hand-rolled RAG setups skip until a security review forces the issue; this ships it as a first-class concern rather than an afterthought.

## Problems It Can Remove
Building per-source connectors plus a permission-inheritance model that stays correct as source ACLs change is a multi-week project most teams underestimate until an access-control bug surfaces in production.

## Practical Uses
- Internal "ask our docs/wiki/drive" assistant that respects each employee's actual document permissions instead of a flat index anyone can query
- RAG layer for a customer-facing support agent where different source systems have different access boundaries
- Baseline for a permission-aware knowledge-search feature inside a B2B product rather than building ACL-scoped retrieval from scratch

## Product Opportunities
Permission-aware, citation-backed enterprise search is a common ask for security-conscious B2B buyers — directly relevant if a future product needs "AI search over our customers' data without leaking across tenants/roles."

## Agent / Automation Opportunities
Positioned explicitly as the retrieval layer AI agents call, not just a human-facing search UI.

## Integration
Deployment: Docker, self-hosted
Interfaces: REST API, SDK (npm @pipeshub-ai/sdk), web UI
Integration effort: **Medium** — full platform (connectors, indexing, permission engine, UI) rather than a single library.

## Architecture Notes
Connector-based ingestion from enterprise sources with a permission-inheritance layer sitting between raw retrieval and what's surfaced to a given user/agent, plus citation tracking back to the originating document.

## Maturity
Emerging. GitHub API verified: Apache-2.0, 3,758 stars, pushed_at 2026-09-19, created 2025-03-06. Real multi-contributor team (5+ contributors each with 180+ commits sampled), active Discord/roadmap, Docker Hub image with pull counts.

## License
Apache-2.0. Verified via license badge and GitHub API. No restrictions on commercial use, SaaS deployment, or embedding.

## Alternatives
- onyx-dot-app/onyx (32,161★, not deep-reviewed this run) — larger, more established open-source AI chat/search platform with a similar connector model
- Building permission-scoped retrieval by hand on top of pgvector/pg_textsearch — more control, materially more engineering time

## Risks / Limitations
- Full platform (connectors, indexing, permission engine, UI) rather than a single library — meaningfully more to deploy and operate than a narrow tool
- Pre-1.0 (v0.8.0), young for a permission-sensitive system — audit the ACL-inheritance logic directly before trusting it with real access boundaries

## Recommendation
**PROTOTYPE** (score 7.8/10) — worth a small real trial before committing; audit the permission-inheritance logic directly.

## Change History
### 2026-09-19
First discovered and reviewed. Verified via GitHub API: Apache-2.0, current version v0.8.0 (2026-09-14).
