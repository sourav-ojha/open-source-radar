# OpenNotebookLM

## Summary
A self-hosted alternative to Google's NotebookLM: import PDFs, web pages, and YouTube
transcripts, ask questions, and get answers with citations back to the source. Runs as a
Dockerized Next.js frontend + FastAPI backend with a local SQLite database; hybrid dense
(sqlite-vec) + BM25/FTS5 retrieval (with CJK bigram support) and any LLM provider, or none
(extractive fallback answers).

## Why I Should Care
It's most of the scaffolding for a small research/notebook SaaS already built and
Dockerized: auth with non-enumerable ownership checks (404 not 403 on another account's
project), hybrid retrieval, and four distinct "Studio" output generators (Markdown report,
audio summary, mind map, narrated slideshow) built from the same project summary.

## Problems It Can Remove
Removes the need to build a citation-backed document Q&A tool from scratch — auth,
retrieval, chunking, and multi-format export are already wired together.

## Practical Uses
- Personal or team knowledge base with citation-backed Q&A over PDFs/web pages/YouTube transcripts
- Confidential-document research tool that never has to leave a self-hosted machine
- Base for a small paid research/briefing product — per-account isolation and export are already built
- Auto-generate a mind map + narrated slideshow summary of a document set for onboarding or briefing

## Product Opportunities
The "Studio outputs" feature plus per-account isolation is most of the scaffolding for a
small paid research/notebook product — worth forking and reskinning rather than building
the retrieval+auth stack from zero.

## Agent / Automation Opportunities
No MCP server shipped yet, but the FastAPI backend (with interactive docs at `/docs`) is a
clean surface to wrap as one, or to call directly as an internal API from a coding agent
needing citation-backed document QA.

## Integration
`docker compose up -d --build` after setting `JWT_SECRET_KEY`; optional `with-ollama` and
`with-cache` (Redis) profiles. Needs ~4GB RAM, 10GB disk. Low integration effort — the
Compose file deliberately fails fast if misconfigured rather than starting broken.

## Architecture Notes
Documents, embeddings, and conversations live in local SQLite. Next.js proxies `/api` to
the FastAPI backend over the Docker network so backend URLs never enter the browser
bundle. Data-migration steps between versions are explicitly documented — a level of
operational care unusual for a single-maintainer project.

## Maturity
Emerging. Created 2025-08-11, one tagged release (v0.1.0, 2026-08-25), 243 commits, but
all from a single contributor (tom1030507) — meaningful bus-factor risk despite the
engineering discipline on display.

## License
MIT. No restrictions on commercial use, redistribution, or embedding.

## Alternatives
Google NotebookLM (closed, cloud-only), Khoj, Quivr, Danswer/Onyx, and haiku.rag
(catalogued the same day — library/CLI/MCP-first vs. OpenNotebookLM's full deployable
webapp with its own UI and auth).

## Risks / Limitations
- Single maintainer — all commits from one contributor
- Only 45 stars; unproven at real multi-user scale
- No MCP server yet, so it's a standalone webapp rather than an agent-composable tool today

## Recommendation
PROTOTYPE — worth standing up locally to evaluate as a personal research tool or as a
starting point for a small notebook/research product, before relying on it for anything
important given the single-maintainer risk.

## Change History
### 2026-09-05
Discovered via GitHub search (rotation slot 4: data, search, documents, RAG). Catalogued
at PROTOTYPE, 8.2/10.
