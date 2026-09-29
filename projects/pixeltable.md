# Pixeltable

## Summary
A Python library (Apache-2.0) that collapses object storage, a vector index,
orchestration, and an HTTP serving layer into one file: multimodal (text/image/video/
audio/document) tables where a transform is a computed column, an index is a
declaration, and inserting a row runs the whole pipeline. Ships a CLI (`pxt`), an MCP
server, a Claude/Cursor/ChatGPT agent skill, and an optional hosted "Pixeltable Cloud"
(invite-only beta).

## Why I Should Care
A multimodal AI feature normally needs object storage, a vector database, a task
orchestrator, and API code binding them together — four systems to provision and keep
in sync. Pixeltable is one Python file that does all four, which is a real reduction in
infrastructure surface for the price of running a Python service alongside a Node/
Express stack.

## Problems It Can Remove
- Standing up Postgres + pgvector + S3 + a FastAPI service just to store and search
  multimodal content
- Hand-wiring a task orchestrator to recompute derived fields (thumbnails, transcripts,
  summaries, embeddings) whenever source data changes

## Practical Uses
- Multimodal RAG pipeline — embeddings and vector search over documents/images/video
  defined in one schema
- Media-processing pipeline where thumbnails/transcripts/summaries are computed columns
  that recompute automatically on insert/update
- Backend for an agent that needs a queryable knowledge table without a separate
  vector database
- Generating an OpenAPI-described HTTP service directly from a Python table schema for
  a Next.js frontend to call

## Product Opportunities
Removes the "stand up four systems" boilerplate for any AI feature needing multimodal
storage, search, and serving — a genuine shortcut from idea to shippable product for a
document/media-heavy feature.

## Agent / Automation Opportunities
Ships a Claude/Cursor/ChatGPT-consumable Agent Skill and an MCP server specifically so
a coding agent writes the single `app.py` correctly on the first try — the docs
explicitly assume an agent-authored workflow.

## Integration
`pip install 'pixeltable[serve]'`, then `pxt init` / `pxt schema update` / `pxt service
update`. Self-hostable entirely; Pixeltable Cloud is optional. Medium integration
effort — it's a full Python service, not a drop-in Node library, and the CLI-driven
workflow (`pxt schema update` vs `pxt service update` are separate steps) has some
learning curve.

## Architecture Notes
Computed columns with automatic dependency-graph recomputation on insert/update is the
core idea worth studying — it's a small incremental-computation engine sitting under a
table abstraction. The one-file-per-app pattern (schema + UDFs + FastAPI routes all in
`app.py`) is also a deliberate design choice to keep an entire AI backend reviewable in
a single diff.

## Maturity
Mature. Created 2023-05-10, 1,629 stars, 223 forks, pushed same day as this review.
Healthy multi-contributor team (aaron-siegel, mkornacker, and several others with real
commit counts) with a weekly release cadence — not a bus-factor risk project.

## License
Apache-2.0. Unrestricted for commercial/SaaS use and embedding. The hosted Cloud tier
is a separate paid product but does not gate the open-source library.

## Alternatives
- **Supabase + pgvector** — general-purpose, more moving parts to wire together.
- **LlamaIndex/LangChain data connectors** — orchestration only, no storage layer.
- **Weaviate/Qdrant + separate object storage** — multiple systems to run and keep in
  sync.

## Risks / Limitations
- Python-only — runs as a separate service alongside Node/Express APIs, not inside
  them.
- Hosted Cloud tier is invite-only beta; verify the self-hosted path's production
  support before depending on it.
- Framing assumes an AI-agent-authored `app.py`, a different adoption model than a
  traditional ORM.

## Recommendation
STUDY — the one-file collapse of storage/vector-index/orchestration/serving is worth
understanding even without adopting the whole Python-centric stack; PROTOTYPE if a
specific upcoming feature is media- or document-heavy enough to justify a Python
service.

## Change History
### 2026-09-29
Initial discovery and review. Catalogued as STUDY, 7.7/10.
