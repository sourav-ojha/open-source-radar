# GEO Optimizer (GeoReady)

## Summary
MIT-licensed CLI, Python library, MCP server, and Astro integration (Auriti-Labs) that audits a website against 47 research-backed methods for AI answer-engine visibility (ChatGPT, Perplexity, Gemini, Claude, Google AI Overviews). Scores AI-search readiness 0–100, generates concrete fixes (robots.txt bot rules, llms.txt, JSON-LD schema), and can query real answer engines to check whether a brand is actually cited.

## Why I Should Care
Answer Engine Optimization (AEO) / Generative Engine Optimization (GEO) is a genuinely new SEO-adjacent discipline in 2026 as AI answer engines take search share. This tool gives a concrete, auditable score for a site instead of vague advice, and is directly applicable to his MSSP admin-portal SaaS marketing site and PTaaS partnership pages.

## Problems It Can Remove
Removes the need to manually reverse-engineer what makes AI answer engines cite a page (robots.txt bot access, llms.txt presence, JSON-LD richness, content citability) — the 47 methods are already encoded and cited to research.

## Practical Uses
- Audit a SaaS marketing site for AI-search/citation readiness.
- Wire into CI to catch AI-visibility regressions on future deploys.
- Auto-generate llms.txt and JSON-LD schema for a new product launch.
- Run as an MCP tool so a coding agent can self-check a site's AI-search readiness during development.

## Product Opportunities
The audit engine is itself a template for a micro-SaaS — GeoReady.dev is exactly that (free CLI, paid hosted monitoring) — worth studying as an open-core pattern for a future product.

## Agent / Automation Opportunities
Ships as an MCP server out of the box, usable directly by Claude Code/Cursor/etc. as an audit tool during a site-building or marketing workflow.

## Integration
`uvx --from geo-optimizer-skill geo audit --url <site>` — zero install. Also installable as a pip package, MCP server, or Astro integration. Low effort.

## Architecture Notes
CLI backed by a Python scoring engine with 1,720+ tests; the same engine backs the CLI, the MCP server, the Astro integration, and the hosted GeoReady.dev product — one codebase, four surfaces.

## Maturity
Mature for its age — created February 2026 (~7 months old), 792 stars, real CI and test suite, academic citations (KDD 2024, ICLR 2026 papers referenced for methodology).

## License
MIT for the CLI/library/MCP server. The hosted GeoReady.dev monitoring tier is a separate paid product built on the same open-source engine (open-core model) — no restriction on the OSS part.

## Alternatives
getcito, eGEOagents, aeo.js, elmo — all part of a same-week AEO/GEO wave of near-identical tools; GEO Optimizer stood out for the most mature test suite, MCP/Astro integration breadth, and clearer academic grounding.

## Risks / Limitations
Young discipline — scoring methodology may shift as answer engines change their citation behavior. 12 open issues against 792 stars and a single dominant author suggest a bus-factor-of-one project.

## Recommendation
USE NOW — low-effort, immediately runnable audit with real utility for any site he wants AI search engines to cite.

## Change History
### 2026-09-12
Initial discovery and review.
