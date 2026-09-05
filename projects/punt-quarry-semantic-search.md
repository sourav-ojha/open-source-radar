# Quarry (punt-quarry)

## Summary
A local semantic search daemon for AI coding agents and humans. Indexes 20+ document
formats with a local ONNX embedding model (snowflake-arctic-embed-m-v1.5) into LanceDB, and
serves search over CLI, MCP, and a local HTTP API from one long-running `quarryd` process
per machine. Auto-indexes the current project at Claude Code session start via a bundled
plugin.

## Why I Should Care
It's purpose-built for exactly the Claude Code-heavy workflow this profile already runs in
— a daemon-plus-thin-clients design that loads the embedding model once, rather than a
generic RAG toolkit retrofitted with an MCP wrapper afterward.

## Problems It Can Remove
Removes the gap between "grep the codebase" and "stand up a real RAG stack" for a coding
agent that needs semantic (meaning-based) recall over the current project, entirely
offline.

## Practical Uses
- Semantic (not just grep) search over the current project directly from Claude Code, auto-indexed at session start
- A local knowledge-search layer for any MCP client (Claude Desktop, Cursor via HTTP) without sending code to a cloud embedding API
- One daemon serving multiple thin clients (CLI, MCP, hooks) instead of reloading an embedding model per invocation

## Product Opportunities
None identified — this is a personal developer-tooling utility, not a product-shaped component.

## Agent / Automation Opportunities
Ships a Claude Code plugin (`claude plugin install quarry@punt-labs`) and session-start
hooks for automatic project indexing — built specifically for an agent-native dev workflow.

## Integration
Homebrew tap (`brew install punt-labs/tap/quarry`) or a `curl | sh` installer, then `quarry
install` for the daemon, TLS certificates, and MCP config. Medium effort mainly due to
platform scope: Apple Silicon macOS and Linux only, no Windows, no Intel macOS wheel (the
gap is in upstream lancedb/onnxruntime, not fixable by this project alone).

## Architecture Notes
One `quarryd` daemon per machine loads the embedding model once; the CLI, MCP server, and
Claude Code hooks are thin clients over it, reachable directly too via a TLS-secured local
HTTP API. Ships with an install/doctor/health CLI and a "Working Backwards" PRFAQ in the
repo — an unusually disciplined process for a 3-star project.

## Maturity
Emerging. Created 2026-02-08, already at v3.2.1 (matching the PyPI release), CI configured,
3 contributors (one human, one Claude-Code-attributed committer, plus dependabot).

## License
MIT. No restrictions on commercial use, redistribution, or embedding.

## Alternatives
ripgrep/plain grep for lexical-only search; ctx (catalogued 2026-09-04 — indexes past agent
session transcripts for recall, not project documents/code for semantic search;
complementary rather than competing); Cursor's built-in codebase indexing (cloud-based,
IDE-locked); free-search-mcp/RivalSearchMCP (web/document search, not local project
semantic search).

## Risks / Limitations
- macOS Apple Silicon and Linux only — no Windows support
- Very low visibility: 3 stars, effectively a two-person effort, not yet community-validated
- Aggressive semver (already v3.2.1 after ~7 months) suggests active iteration but also API churn risk

## Recommendation
USE NOW — low-risk to install for personal use (local-only, MIT, doesn't touch product
code); directly improves an existing Claude Code workflow today.

## Change History
### 2026-09-05
Discovered via GitHub search (rotation slot 4: data, search, documents, RAG). Catalogued
at USE NOW, 7.8/10.
