# Atomic

## Summary
Self-hosted, AI-augmented personal knowledge base. Markdown notes ("atoms") are automatically chunked, embedded, tagged, and semantically linked; a force-directed canvas visualizes the resulting graph, an LLM can synthesize cited wiki articles from your own notes, and an agentic chat interface does RAG over the whole base. Ships as a Tauri desktop app, a headless Docker/Fly.io server, an iOS app, a browser extension, and an MCP server.

## Why I Should Care
Most "AI notes app" pitches stop at semantic search. Atomic adds LLM-generated, cited wiki synthesis and a spatial canvas on top of that, as a genuinely self-hostable product with a mature multi-platform client story (desktop, iOS, browser extension, MCP) rather than a demo repo.

## Problems It Can Remove
Replaces a Notion/Obsidian-plugin-stack/Mem/Reflect subscription with a self-hosted, semantically-linked notes vault, including the LLM-synthesis and graph-visualization layers those usually require bolting on separately.

## Practical Uses
- Daily-driver personal knowledge base, replacing scattered markdown notes or a paid notes SaaS.
- Feed in RSS subscriptions, call transcripts, and research notes and get LLM-synthesized, cited wiki articles back.
- Point an MCP-aware coding agent at a project's knowledge base instead of re-explaining context every session.
- Scheduled "reports" (daily briefings, contradiction scans, open-question tracking) over an accumulating notes corpus.

## Product Opportunities
The compile-notes-into-cited-wiki pattern is a reusable architecture for any internal "second brain" or client-knowledge feature.

## Agent / Automation Opportunities
Ships a native MCP server exposing the knowledge base to Claude and other MCP-aware tools — a concrete pattern for giving an agent durable, queryable memory over a personal or project corpus.

## Integration
Docker Compose self-host (`docker compose up -d` with an `ATOMIC_SETUP_TOKEN`) starts API server, web frontend, and an nginx reverse proxy. Also deployable to Fly.io, or built standalone via `cargo run`. Desktop app available as a pre-built binary. Low integration effort; requires an external AI provider (OpenRouter, Ollama, or any OpenAI-compatible endpoint) for embeddings/LLM features.

## Architecture Notes
Rust server core with sqlite-vec for vector search; semantic similarity drives the canvas's force-directed layout rather than a fixed hierarchy. Multi-database support with a shared registry allows several independent knowledge bases per install.

## Maturity
Emerging but notably more proven than most entrants in this niche: 1,995 stars, 142 forks, 9 subscribers, 10 contributors, created 2025-11-25 (~11 months old), latest release v1.45.0 (2026-08-09), pushed 2026-09-29. Has a paid "Atomic Cloud" hosted tier alongside the free self-hosted path — a sustainability signal most side-project PKM tools lack.

## License
MIT — unrestricted commercial use.

## Alternatives
Obsidian + plugins (no built-in semantic graph or LLM wiki synthesis); Notion AI and Mem/Reflect (closed-source SaaS, no self-host). Among already-catalogued tools, Graphify and sage-wiki skew agent-first; Atomic is the more complete human-first PKM app with real desktop/mobile clients.

## Risks / Limitations
- Requires an external AI provider — not a zero-dependency tool.
- Adopting a new daily-driver notes app carries a bigger personal-workflow switching cost than a drop-in dev-tool swap.
- 36 open issues against 10 contributors is normal for an active indie project but worth a skim before trusting it with irreplaceable notes.

## Recommendation
PROTOTYPE — worth a real trial as a daily-driver PKM tool before committing notes to it long-term.

## Change History
### 2026-10-25
Discovered via GitHub search for self-hosted knowledge base tools (slot 5: self-hosted SaaS alternatives, productivity). First-time review.
