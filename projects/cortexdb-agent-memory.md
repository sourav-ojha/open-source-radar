# CortexDB

## Summary
CortexDB is a pure-Go embedded memory and knowledge-graph engine that lives in a single SQLite file. It bundles vector search, relational memory, RDF/SPARQL, and Cypher graph queries into one process with no external service, and it works without an embedding model (it has its own retrieval path for when one isn't configured). It ships an MCP server with 80+ tools, a Claude Code / Codex plugin that gives either agent a global `~/.cortexdb/cortexdb.db` "brain" with `/remember` and `/recall`, and an optional gRPC server for sharing one memory store across multiple agents or machines.

## Why I Should Care
Every "give my agent memory" approach either wires a vector DB + graph DB + document store together, or writes memory to a markdown file and hopes. CortexDB collapses that into one file and one Go import, with client bindings for Rust, Python, and Node — so it's usable from a Node/Express backend directly, not just from Go tooling. The Claude Code plugin install is a one-liner, which matters directly: this radar and most of my day-to-day agent work already runs through Claude Code.

## Problems It Can Remove
- Removes the need to stand up a vector DB + graph DB combo just to give an internal tool or agent persistent memory.
- Removes the "embedding model is a dependency" problem for small/offline agent-memory use cases.
- Removes the need to hand-roll memory sharing across multiple agent sessions/machines (the `cortexdb-grpc` shared-brain mode does this directly).

## Practical Uses
- Give a Claude Code or Codex session durable cross-session memory without a separate server.
- Embed as a Go library inside an internal tool that needs lightweight semantic + graph + time-series memory.
- Back an internal support/admin tool's "what do we know about this customer" lookup with Cypher/SPARQL queries instead of ad hoc SQL joins.
- Point several coding-agent instances at one `cortexdb-grpc` endpoint to share a team knowledge base.

## Product Opportunities
Could back a lightweight "agent memory as a service" feature inside a product (e.g., a support bot that remembers prior tickets) without taking on a vector-DB + graph-DB infra bill.

## Agent / Automation Opportunities
Native Claude Code and Codex plugins, an MCP server with 80+ tools, and `cortexdb-mcp --doctor --self-test` diagnostics. This is squarely built for agent use, not retrofitted.

## Integration
`go get` for Go, npm/pip/cargo client packages for other stacks, or a one-line Claude Code/Codex plugin install. No external database or service required for the embedded case; the gRPC server is opt-in for multi-agent sharing. Integration effort: **Low**.

## Architecture Notes
Single SQLite file as the storage substrate for multiple query paradigms (vector, relational, RDF/SPARQL, Cypher) is the interesting bet — it trades the usual "best tool per data shape" sprawl for one file you can back up, copy, and reason about. The plugin architecture layers a "mod" (an optional live hook inside Claude Code) on top of the base MCP plugin rather than requiring it.

## Maturity
Emerging. Created August 2025, very actively released (v2.121.1 as of this review, same day), 0 open issues, CI and codecov wired up. 270 stars, 7 forks — small community, effectively a solo-maintainer project at this point despite the release cadence.

## License
MIT. No commercial-use restrictions.

## Alternatives
Standing up a vector DB (Chroma, Qdrant) plus a graph DB (Neo4j) plus a document store; other single-file agent-memory projects in this space (Sibyl-Memory, yantrikdb, NodeDB) take a similar "collapse the stack" approach but with different storage engines and licenses — CortexDB's SQLite-file + no-embedding-model-required combination plus first-class Claude Code/Codex plugin support is the differentiator here.

## Risks / Limitations
- Effectively solo-maintained; the very high version numbers (v2.121.x) suggest a rapid auto-release cadence rather than a large contributor base — bus-factor risk if the maintainer stops.
- Small community (270 stars, 0 watchers reported) means little independent validation of the multi-paradigm query claims yet.
- "Works without an embedding model" needs hands-on testing to judge actual recall quality versus a real embedding-backed setup.

## Recommendation
PROTOTYPE — worth wiring into a Claude Code workflow or a small internal tool to see how the memory quality holds up before relying on it for anything production-critical.

## Change History
### 2026-10-06
Initial discovery and review. Slot 7 (experimental projects and hidden gems) run.
