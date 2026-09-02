# Tela

## Summary
A self-hostable, markdown-native team wiki with a built-in MCP server so AI agents read/write/search docs as first-class collaborators alongside humans. Go + pgx (no ORM) + PostgreSQL/pgvector backend, React 19 + Milkdown + Yjs frontend for real-time multiplayer editing.

## Why I Should Care
Most self-hosted wiki alternatives treat AI as a bolted-on chat feature; Tela's MCP-first design (agents get scoped read/write tools, not just a chatbot) is a more honest answer to 'how should a product expose itself to agents' and worth studying even without adopting the whole app.

## Problems It Can Remove
Real-time multiplayer markdown editing (Yjs) plus a working hybrid keyword/semantic search plus a permission-scoped MCP layer is a multi-week build; here it's a reference implementation to read rather than a dependency to add.

## Practical Uses
- Self-hosted Notion/Outline alternative where the knowledge base is also directly queryable/writable by coding agents
- Study the Go+pgx+pgvector+Yjs architecture as a pattern for building agent-writable collaborative apps

## Product Opportunities
- Architecture (hand-written SQL, pgvector semantic search, Yjs realtime, MCP-native) is a reusable pattern for any 'AI-native' collaborative product Sourav might build

## Agent / Automation Opportunities
- MCP endpoint with per-tool write restrictions and PAT/OAuth auth is a concrete example of how to expose a product's data to agents safely

## Integration
Deployment: Docker Compose (self-hosted), one-click deploy (Render, DigitalOcean)
Interfaces: web app, MCP server (/api/mcp), REST-ish API, tela-mcp npm package
Integration effort: **Medium**

## Architecture Notes
GitHub API verified: AGPL-3.0, 57 stars, created 2026-05-18, pushed_at 2026-09-02, not archived.

## Maturity
Emerging. GitHub API verified: AGPL-3.0, 57 stars, created 2026-05-18, pushed_at 2026-09-02, not archived.

## License
AGPL-3.0. AGPL-3.0 — flagged loudly per AGENT.md: fine for internal self-hosted use, but modifying and offering it as a hosted service to others triggers AGPL's source-disclosure requirement. Maintainer offers a separate commercial/Enterprise license for production use beyond that.

## Alternatives
- Docmost, AFFiNE, Outline (self-hosted wikis without native MCP integration)
- Notion, Confluence (SaaS, no self-hosting, no native agent write access)

## Risks / Limitations
- AGPL-3.0 — see commercial_use_notes; do not modify-and-host without checking licensing implications
- Full stack requires Postgres+pgvector, Gotenberg, and a Slidev sidecar — heavier to run than a single binary
- 57 stars, ~3.5 months old — competes in a crowded self-hosted-wiki space without long track record yet

## Recommendation
**STUDY** (score 7.3/10) — see Why I Should Care above.

## Change History
### 2026-09-02
First discovered and reviewed. Verified via GitHub API: license AGPL-3.0, current version v0.8.0 (2026-07-05).
