# Markdown Vault MCP

## Summary
An MCP server that turns a personal markdown vault (Obsidian-style notes, YAML frontmatter, Git-tracked) into a hybrid keyword+semantic search and write surface for an LLM. SQLite FTS5 (BM25) fused with optional embeddings (OpenAI/Voyage/Ollama/FastEmbed) via reciprocal rank fusion; 34 LLM-visible tools.

## Why I Should Care
Production-grade packaging (CI/CD, codecov, semantic versioning, edge/pre-release/stable channels, systemd hardening) for what's normally a weekend-project category of tool — it's ready to point at a real notes vault today.

## Problems It Can Remove
Hybrid BM25+semantic search with reciprocal rank fusion and 34 well-scoped MCP tools is more polish than a quick personal script would deliver.

## Practical Uses
- Turn an existing personal notes vault into a Claude-queryable knowledge base without standing up a vector DB
- Give a coding agent write access to a scratch/notes vault for research capture during a task
- Embeddings are optional — degrades gracefully to keyword-only search if no embedding provider is configured

## Product Opportunities
None identified beyond direct use.

## Agent / Automation Opportunities
- Ready-made MCP server for Claude Desktop/any MCP client — no build required, just point at a vault directory

## Integration
Deployment: pip install, Docker, Linux packages (.deb/.rpm), Claude Desktop bundle (.mcpb), embeddable Python library
Interfaces: MCP server (stdio or HTTP), Python library
Integration effort: **Low**

## Architecture Notes
GitHub API verified: MIT, 32 stars, created 2026-03-07, pushed_at 2026-09-02, not archived.

## Maturity
Emerging. GitHub API verified: MIT, 32 stars, created 2026-03-07, pushed_at 2026-09-02, not archived.

## License
MIT. MIT, no restrictions.

## Alternatives
- Obsidian's own plugins (not MCP-native)
- zcag/tela (team-oriented, requires standing up a full Docker stack, vs. this being a single-user local tool)

## Risks / Limitations
- 32 stars — small maintainer base for a tool that would hold write access to a personal knowledge base
- Only useful if Sourav maintains a markdown notes vault worth indexing — no such vault currently referenced in the catalog's prior context

## Recommendation
**PROTOTYPE** (score 7.1/10) — see Why I Should Care above.

## Change History
### 2026-09-02
First discovered and reviewed. Verified via GitHub API: license MIT, current version v4.1.0 (2026-08-30).
