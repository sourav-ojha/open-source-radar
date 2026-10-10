# MuninnDB

## Summary
MuninnDB is a single-binary agent-memory server that models memory strength as a decaying/reinforcing quantity — Ebbinghaus-style decay, Hebbian-style reinforcement, Bayesian confidence — as engine-native primitives, rather than flat vector similarity with a recency filter. It's exposed over MCP, REST, and gRPC, with auto-configuration for Claude Desktop, Claude Code, Cursor, OpenClaw, Windsurf and others.

## Why I Should Care
Most agent-memory tools are a vector store with a recency heuristic bolted on. MuninnDB makes decay and reinforcement first-class engine primitives: memories that keep getting used strengthen, ones that don't fade, and relevant memories can be pushed to the agent proactively rather than only retrieved on query. It's a working single-binary implementation with a real multi-contributor team behind it, not just a research idea — but the licensing terms require real consideration before using it.

## Problems It Can Remove
- Removes the need to manually re-establish context for an agent at the start of every session.
- Removes the all-or-nothing retrieval problem of flat vector similarity, where old-but-relevant and old-but-stale memories are indistinguishable without extra logic.

## Practical Uses
- Persistent memory for a coding agent that surfaces relevant past incidents/decisions automatically given the current working context.
- Architecture reference for a decay/reinforcement-based memory model, independent of whether MuninnDB itself is adopted.

## Product Opportunities
The patent-plus-BSL combination (see License) makes this unsuitable to embed into a commercial product without a license from the author right now. Treat it as an architecture reference rather than a build-on candidate until that's resolved.

## Agent / Automation Opportunities
`muninn init` auto-detects and configures Claude Desktop, Cursor, OpenClaw, Windsurf, OpenCode, VS Code and others for MCP access. Memory is written and queried over a simple REST API (`/api/engrams`, `/api/activate`) in addition to MCP.

## Integration
Single binary, zero dependencies, installed via a one-line curl/PowerShell script (`curl -sSL https://muninndb.com/install.sh | sh`); starts a local HTTP API (port 8475), web UI (port 8476), and MCP endpoint (port 8750). Integration effort: **Low** to try, but note the quickstart ships default admin credentials (`root`/`password`) that must be changed immediately.

## Architecture Notes
Go-based single binary (1.25+). "Engrams" (memory units) are created via the REST API and activated by context strings; the engine scores relevance using its decay/reinforcement/confidence model rather than pure vector distance. No external database or vector store dependency — self-contained.

## Maturity
Alpha, per its own status badge. Created February 2026, 332 stars, 76 forks, 13 subscribers, pushed September 2026. A real team: 924 commits from the primary author plus several contributors at 7-26 commits each — more community than a typical solo alpha project.

## License
Business Source License 1.1 (BSL), converting to Apache-2.0 on 2030-02-26. **Flagged loudly per AGENT.md section 14:** free for individuals/hobbyists/researchers/open-source projects, organizations under 50 employees and $5M annual revenue, or strictly internal use not offered to third parties. Anything beyond that — e.g. embedding it in a hosted product offered at scale — requires a commercial license from the author. The repository also states a provisional patent was filed (2026-02-26) on "the core cognitive primitives." Patenting the mechanism behind a BSL-licensed project is an unusual combination that narrows reuse further than BSL alone. Get explicit legal clearance before embedding this in any commercial product.

## Alternatives
Mem0, Zep, and other already-known agent-memory services; already-catalogued agent-memory projects from recent runs (Hippo, okf-agent-memory, and similar) — this one's differentiator is an explicit decay/reinforcement engine rather than a vector-store wrapper.

## Risks / Limitations
- BSL 1.1 plus a filed provisional patent on the core mechanism — this is the dominant risk, not the technology itself.
- Alpha status.
- Heavy marketing framing (Ebbinghaus decay, Hebbian learning, Bayesian confidence as headline selling points) — verify the substance behind the terms against actual behavior before trusting it in practice.
- Default admin credentials shipped in the quickstart — a real operational risk if not changed immediately.

## Recommendation
WATCH — the architecture is genuinely differentiated and worth tracking, but the BSL-plus-patent combination and alpha maturity mean this isn't ready to adopt or build on yet. Revisit if licensing terms change or the patent situation clarifies.

## Change History
### 2026-10-10
Discovered via GitHub search (slot 4: data, search, documents, RAG). First catalog entry.
