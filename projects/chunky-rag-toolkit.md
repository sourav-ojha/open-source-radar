# Chunky

## Summary
Local, self-hosted workspace (Python/FastAPI backend, React frontend) for preparing
documents for RAG. Compares PDF-to-Markdown converters (PyMuPDF, Docling, MarkItDown,
LiteParse, VLM, cloud APIs) and chunking strategies (LangChain, Chonkie, Docling
splitters) side by side on real documents, plus LLM-powered chunk metadata enrichment
(titles, summaries, keywords, retrieval questions).

## Why I Should Care
Most RAG failures start before retrieval: broken tables, scrambled reading order, or
chunks that look fine in isolation but fail in context. Chunky makes that visible and
comparable instead of a hidden parameter you tune blind. It directly complements
document-extraction libraries like OpenDataLoader PDF (catalogued the same day) by
answering "which of six converters actually handles *this* document well" before
committing to one in a pipeline.

## Problems It Can Remove
- Guessing at chunk size/overlap/splitter choice with no visual comparison
- A custom one-off eval script for comparing PDF-to-Markdown converters
- Silent RAG retrieval-quality regressions caused by an undetected bad conversion

## Practical Uses
- Visually compare PDF-to-Markdown converters on an actual document before committing
  one to a pipeline
- Compare chunking strategies (size/overlap/splitter) side by side on real content
  instead of guessing
- Catch broken tables or scrambled reading order before they degrade retrieval quality
- Generate LLM-enriched chunk metadata for a search index

## Product Opportunities
Use as an internal QA step before shipping any customer-facing "ask your docs"
feature — cheap insurance against a silent retrieval-quality regression that would
otherwise only surface as "the AI gave a wrong answer" days later.

## Agent / Automation Opportunities
Not itself agent infrastructure, but directly de-risks building a RAG-backed agent tool
(a support or coding agent's document-search backend) by catching bad chunking before
it reaches the vector store.

## Integration
`git clone` + `./start_all.sh` (or `.ps1` on Windows) for local, or `docker compose up
--build`. Exposes a frontend (`:5173`), FastAPI backend (`:8000`), and Swagger docs.
**Low** integration effort — it's a standalone workspace, not something wired into a
production pipeline's runtime path.

## Architecture Notes
Saved chunk sets retain the SHA-256 revision of the Markdown that produced them; if the
source Markdown changes, the saved chunks remain inspectable but must be regenerated
before overwriting the current version — a sensible cache-invalidation pattern for any
tool that lets you compare derived artifacts against a mutable source.

## Maturity
Emerging: created 2026-03-06 (about 6 months old), 170 stars, pushed 2026-07-25,
monthly release cadence (v0.6.0 in June, v0.7.0 in July) — active for its size but
looks single-maintainer.

## License
MIT — no restrictions.

## Alternatives
- **Chunkr** (Lumina AI) — structure-aware chunking, hosted-first rather than a local
  comparison workspace.
- **RAGFlow**'s built-in chunk visualization — bundled into a much larger platform, no
  cross-converter comparison workflow.

## Risks / Limitations
- Small, likely single-maintainer project — bus-factor risk.
- It's a review/QA workspace, not something embedded into a production pipeline
  directly — the value sits upstream of deployment, not inside it.

## Recommendation
**PROTOTYPE** — run it against one real internal document set (e.g. anything feeding a
future RAG feature) before deciding whether to keep it in the toolbox.

## Change History
### 2026-08-29
First catalogued. GitHub API verified: MIT, 170 stars, pushed_at 2026-07-25, not
archived, created 2026-03-06.
