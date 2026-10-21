# ChunkHound

## Summary
Local-first codebase-intelligence MCP server and CLI. Tree-sitter parsing across a dozen+ languages, semantic + regex code search, git-history search (by commit hash, tag, branch, or range), and web research grounded back to the local codebase with cited answers. Commands: `index`, `search`, `research`, `websearch`, plus a `.chunkhound.json` config file.

## Why I Should Care
Most "code search for agents" tools stop at finding relevant code snippets. ChunkHound adds two things that niche is generally missing: git-history-aware search ("did this already get fixed, and when") and web research tied back to the local codebase with citations ("does our auth code still match the library's current docs"). Both are questions that come up constantly when working in an unfamiliar or legacy part of a codebase.

## Problems It Can Remove
Removes the need to manually grep through git log/blame history or separately open docs in a browser while an agent works — both get folded into one tool the agent can call directly via MCP.

## Practical Uses
- Give a coding agent cited, grounded answers about a large unfamiliar codebase instead of letting it guess from partial context.
- Search git history by commit range/tag/branch when tracking down when and why a behavior changed.
- Ground a research question against both local code and a live web search in a single query.
- Index locally with Ollama embeddings when codebase content can't be sent to an external embedding API.

## Product Opportunities
None specific — this is an internal developer-productivity tool, not a product-building block.

## Agent / Automation Opportunities
Ships as an MCP server directly, so it drops into Claude Code, Cursor, or any MCP-capable agent without wrapping. The CLI is also usable standalone in CI or scripts for codebase Q&A without any agent harness attached.

## Integration
`pip install chunkhound`, configure `.chunkhound.json`, point an MCP client at it. Regex search works immediately with no provider; semantic search needs an embedding provider (VoyageAI recommended, or OpenAI, or local Ollama). Integration effort: **Low**.

## Architecture Notes
Tree-sitter-based parsing is the foundation for multi-language structural awareness rather than naive text chunking. The combination of code index + git history + web search as one grounded-answer surface (with citations) is the notable architectural choice here, versus treating each as a separate tool.

## Maturity
Emerging, trending toward established. Created June 2025 (~16 months old at review), steady organic growth, PyPI package, active Discord community, 1,332 commits, v6.1.0 released the day before review. GitHub API verified: MIT, 1,454 stars, 142 forks, 7 subscribers, archived:false. 10 distinct contributors on the contributors endpoint.

## License
MIT — verified via GitHub API license field. No commercial-use restrictions.

## Alternatives
- **mex** (catalogued, PROTOTYPE 8.1) and **Semble** (catalogued, PROTOTYPE 8.1) — both cover local code search for agents; neither offers git-history search or cited web-research grounding.
- **zvec-grep** (evaluated previously, Worth Watching) — narrower, embedding-focused grep replacement.

## Risks / Limitations
- README carries a "100% AI Generated" badge — worth independently verifying code quality and test coverage rather than taking the maturity signals at face value.
- Semantic search's main value proposition depends on a paid embedding provider or a self-managed local Ollama setup.
- Crowded niche; differentiation rests specifically on the git-history and web-research features, not raw search quality, which hasn't been independently benchmarked here.

## Recommendation
**PROTOTYPE** — worth trying as an MCP server on an unfamiliar or legacy codebase specifically to test the git-history-search and cited-web-research features, which are the differentiators over already-catalogued alternatives.

## Change History
### 2026-10-21
Initial discovery and catalog entry. Slot 1 (AI agents, MCP, coding productivity) run.
