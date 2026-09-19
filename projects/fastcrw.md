# fastCRW (crw)

## Summary
Self-hostable Rust web scraper/crawler/search engine exposing a Firecrawl/Tavily-compatible API (/scrape, /crawl, /search) as a single ~6 MB binary. Turns any URL into clean Markdown or structured JSON. Runs fully local and free, or against a managed cloud with paid credits for JS rendering and proxy rotation.

## Why I Should Care
Drop-in API compatibility with Firecrawl/Tavily means an existing integration can point at a self-hosted binary instead of a metered API with no code changes beyond the base URL, for the common case (non-JS-heavy pages) that doesn't need the managed cloud's rendering/proxy layer.

## Problems It Can Remove
A correct, fast HTML-to-Markdown/JSON extraction pipeline plus crawl-frontier and rate-limit handling is a multi-day build; this ships it as a 6 MB binary with published benchmarks against the two services it's compatible with.

## Practical Uses
- Replace a paid Firecrawl/Tavily API subscription for scrape/crawl/search calls in an agent pipeline with a self-hosted, drop-in-compatible local binary
- Give a coding agent a local `crw search`/`crw scrape` tool with no per-call API cost
- Turn a URL into clean Markdown for a RAG ingestion step without a hosted scraping service
- Site-mapping/crawling for a small product's content-indexing job

## Product Opportunities
Removes a recurring per-request SaaS cost (Firecrawl/Tavily billing) for any product feature that needs to fetch and clean web content at scale.

## Agent / Automation Opportunities
Ships an official MCP server (crw-mcp) and auto-registers with Claude Code/Cursor/Codex/Gemini CLI/OpenCode/Windsurf on install.

## Integration
Deployment: single binary, Docker, managed cloud (fastcrw.com)
Interfaces: CLI, REST API (Firecrawl/Tavily-compatible), MCP server
Integration effort: **Low**

## Architecture Notes
Single Rust binary bundling scrape/crawl/search behind a Firecrawl/Tavily-shaped REST surface, so existing client code written against those APIs works with only a base-URL change for the non-JS-rendering path.

## Maturity
Emerging. GitHub API verified: AGPL-3.0, 1,051 stars, pushed_at 2026-09-19, created 2026-03-02. Contributors skew heavily to a single author (997 commits vs next-highest human contributor at 4).

## License
AGPL-3.0. **Flagged loudly**: self-hosting a modified version as a network service (e.g. embedding it inside a product others query over the network) requires releasing source under AGPL, or negotiating a commercial license. Running it internally/locally has no such obligation.

## Alternatives
- Firecrawl (hosted, assumed known) and Tavily (hosted) — the paid services this is API-compatible with; crw trades their managed JS-rendering/proxy infrastructure for a free local binary
- browserbase/stagehand (24,442★) — browser-automation-first, heavier weight, different use case (interaction, not just extraction)

## Risks / Limitations
- AGPL-3.0 — see License section; do not embed inside a hosted product without checking the license implications first
- Single-maintainer-dominant project — verify bus-factor before deep production dependency
- Local mode skips JS rendering and the managed proxy layer, so JS-heavy or bot-protected sites likely still need the paid cloud tier
- Young (created March 2026)

## Recommendation
**PROTOTYPE** (score 7.5/10) — trial against real scrape/crawl targets before relying on it; watch the license and bus-factor risks.

## Change History
### 2026-09-19
First discovered and reviewed. Verified via GitHub API and LICENSE file: AGPL-3.0, current version v0.36.0 (2026-09-19).
