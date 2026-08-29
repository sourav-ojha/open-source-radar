# website2markdown

## Summary
Converts any web page — JS-heavy SPAs, paywalled content, Chinese platforms (WeChat, Zhihu, Feishu) — to clean Markdown via a Cloudflare Worker with a 5-layer fallback pipeline and 14 site-specific adapters. Ships as a hosted API, an MCP server, and a matching Agent Skills package.

## Why I Should Care
The 5-layer fallback + 14 site adapters solve the annoying long tail of 'this one converter works on 95% of pages' that eats real debugging time.

## Problems It Can Remove
Site-specific adapters for paywalled/JS-heavy/non-Latin platforms are exactly the kind of unglamorous, time-consuming edge-case work worth not doing yourself.

## Practical Uses
- Give a coding or research agent a reliable webpage-to-Markdown tool call without hand-rolling scraping fallbacks
- Handle sites that defeat generic converters (SPA rendering, paywalls, non-Latin platform quirks) out of the box
- Self-deploy on your own Cloudflare account for a zero-maintenance, near-zero-cost scraping utility

## Product Opportunities
_None identified this review._

## Agent / Automation Opportunities
- Already ships as an MCP server — zero integration work to expose to a coding agent
- Agent Skills package is directly usable

## Integration
Deployment: Cloudflare Worker (self-deployable), hosted API, npm package (MCP server)
Interfaces: REST API, MCP server, Agent Skill
Integration effort: **Low**

## Maturity
Emerging. GitHub API verified: Apache-2.0, 8 stars, pushed_at 2026-07-15. Featured as today's Small High-Leverage Utility.

## License
Apache-2.0. Apache-2.0 — permissive.

## Alternatives
- xberg-io/html-to-markdown (catalogued — a library, not a scraping/fallback service)
- supermemoryai/markdowner (catalogued as rejected/abandoned)
- Firecrawl (commercial)

## Risks / Limitations
- Only 8 stars and a small team — verify the hosted API's reliability before depending on it for anything production-critical; self-host the Worker if so
- Narrower scope than html-to-markdown (fetch+convert service vs. pure converter library) — not a like-for-like substitute

## Recommendation
**WATCH** (score 6.2/10) — see Why I Should Care above.

## Change History
### 2026-08-29
First discovered and reviewed. Verified via GitHub API: license Apache-2.0, current version v1.0.0 (2026-03-26).
