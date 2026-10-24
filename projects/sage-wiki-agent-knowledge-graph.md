# sage-wiki

## Summary
A graph memory and knowledge base that AI agents and humans build and query together. Drop in
documents (papers, notes, code, email); an LLM compiler extracts entities and relations into an
interlinked wiki with a knowledge graph. Agents query it over MCP (19 tools); humans browse the
same data as plain, Obsidian-compatible markdown. An optional "evidenced graph" mode adds typed
relations, provenance, confidence scores, and per-fact citations. One Go binary scales from a
personal vault to a Postgres/pgvector-backed team or company hub.

## Why I Should Care
Most agent-memory tools are either a vector store with no relational structure, or a graph
database with no human-readable surface. sage-wiki tries to be both at once: a markdown wiki a
human can read in Obsidian, and a compiled knowledge graph an agent can traverse and cite through
MCP. For a personal knowledge base that both Claude Code sessions and I need to consult, that
dual interface is the actual value — not another vector DB to stand up.

## Problems It Can Remove
- Re-explaining the same project context to an agent every session instead of letting it query a
  persistent, structured memory.
- Building entity resolution, citation tracking, and multi-hop graph traversal from scratch for an
  internal "ask your docs" feature.
- The all-or-nothing choice between a human-friendly wiki and an agent-queryable knowledge store.

## Practical Uses
- Overlay an existing Obsidian vault (`init --vault`) and run entirely on local models for a
  zero-cost personal knowledge base.
- Ask multi-hop relational questions through `wiki_graph_query` (e.g. "what depends on X") that
  plain vector search answers badly.
- Share one wiki via git or a self-hosted server as a lightweight, agent-writable team knowledge
  hub; review entity-resolution proposals before they merge.
- Turn on the evidenced-graph passes when an answer needs to cite which specific document and
  span actually support it.
- Scale storage to PostgreSQL/pgvector and front the server with auth once it moves from personal
  to team/company use.

## Product Opportunities
The compile-once/query-many architecture — ingest, LLM-extract a graph, serve cited answers — is a
reusable pattern for any internal "ask your docs" feature, without re-building entity resolution
and provenance tracking from scratch each time.

## Agent / Automation Opportunities
19 MCP tools plus generated agent-skill files teach an agent when to search vs. capture vs.
compile new sources. Review-gated entity resolution ("K8s" and "Kubernetes" merge only after
approval) is a concrete, safe pattern for agent-driven data cleanup rather than silent merging.

## Integration
Single Go binary, no required external services for personal use. Self-hosted server mode and a
PostgreSQL/pgvector backend are available for team/company scale. Docker-deployable. Low
integration effort for personal use; medium once moving to the shared-server/Postgres setup.

## Architecture Notes
The knowledge graph is a *compile output*, not a second database kept in sync — each compile pass
extracts entities and typed relations and writes them alongside the markdown. Tiered compilation
indexes fast and only spends LLM budget where it matters, which is how it claims to scale past
100K documents. Evidenced edges carry `evidence` (supporting span), `confidence`, and `source_doc`
so a conclusion traces back to a specific sentence rather than a whole document.

## Maturity
Emerging. Created 2026-04-04 (~6.5 months old), 637 stars, 105 forks, 10 contributors, frequent
real releases (v0.2.10 → v0.2.12 across two months). Built on top of the author's own "Sage"
agent framework.

## License
MIT — unrestricted commercial use.

## Alternatives
- **Graphiti** (getzep/graphiti, 32k stars) — mainstream real-time knowledge-graph library for
  agents; more of a general graph-memory SDK than a human-browsable wiki.
- **Plain vector-store RAG** over a docs folder — fine for single-hop lookups, can't do multi-hop
  relational queries or carry citations the way the evidenced graph does.
- **Notion/Confluence + bolted-on AI chat** — no native graph structure or agent-write access.

## Risks / Limitations
- stars:watchers ratio is 637:4 (159:1) — elevated versus a typical small project. Real
  contributor count (10), genuine release cadence, and substantive non-templated docs argue
  against the fake-engagement pattern flagged elsewhere in this catalog, but it's worth
  re-checking on the next review.
- Young and emerging rather than proven at scale.
- Full evidenced-graph mode costs one or more LLM calls per document; cost scales with corpus size
  unless run on local models.

## Recommendation
PROTOTYPE — worth running on a real personal vault (notes, code, docs) to see whether the
evidenced-graph citations hold up in practice before trusting it as a team knowledge hub.

## Change History
### 2026-10-24
Discovered. Catalogued at PROTOTYPE, 8.2/10.
