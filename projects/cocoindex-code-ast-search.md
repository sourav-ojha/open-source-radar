# CocoIndex Code (ccc)

## Summary
CocoIndex Code is an AST-based semantic code search CLI built on the Rust transformation engine behind the already-catalogued CocoIndex project. It's packaged specifically for coding agents: a Claude Code/Grok skill plus an MCP server that incrementally index a codebase and answer natural-language code search queries, with a claimed ~70% token reduction versus grep or full-file reads.

## Why I Should Care
AST parsing across many languages, plus incrementally re-indexing a codebase on every file edit, is exactly the kind of infrastructure most teams never build for internal tooling. CocoIndex Code wraps CocoIndex's existing Rust transformation engine in a CLI with a stated 1-minute setup and zero config, and ships first-party integration (Claude Code skill, plugin marketplace entry, MCP server, Grok plugin) rather than requiring a separate indexing pipeline wired up per agent.

## Problems It Can Remove
- Removes the need to hand-build an embeddings index for agent-facing code search.
- Reduces token spend on "find how X is done in this codebase"-style agent queries compared to grep or reading full files.
- Removes the maintenance burden of keeping a code-search index fresh — a bundled hook re-indexes incrementally as files change.

## Practical Uses
- Drop-in semantic code search for a Claude Code session via the `ccc` skill, with no manual indexing step.
- Keep a large repo's code-search index incrementally current as an agent edits files, via a `SessionStart`/`PostToolUse` hook.
- Give a non-Claude agent (Grok, or anything MCP-compatible) the same semantic search through its MCP server.

## Product Opportunities
Could be embedded as the code-search layer inside an internal coding-agent tool or developer portal, rather than building a bespoke embeddings pipeline for that purpose.

## Agent / Automation Opportunities
Ships as a Claude Code Skill (`npx skills add cocoindex-io/cocoindex-code`) and a Claude Code plugin marketplace entry with bundled hooks + MCP server; also available as a Grok plugin. This is the clearest agent-native packaging seen in this niche so far — install once, the agent decides when to use it.

## Integration
`pipx install 'cocoindex-code[full]'` (batteries-included, pulls in `sentence-transformers` for local embeddings) or the slim variant (`cocoindex-code`, LiteLLM-only, requires a cloud embedding API key). Integration effort: **Low** — CLI install plus a skill/plugin registration, no new service to operate beyond the local index files.

## Architecture Notes
Built on CocoIndex's Rust data-transformation engine (already catalogued separately for general incremental RAG/data indexing). This product narrows that engine's scope specifically to AST-based code chunking and semantic search, wrapped in a CLI and agent-facing packaging (skill, plugin marketplace, MCP server) that the underlying engine doesn't provide on its own.

## Maturity
Emerging — created February 2026, 2,823 stars, real multi-contributor team from the CocoIndex org (not a solo project). Active release cadence (v0.2.42 as of 2026-10-05).

## License
Apache-2.0, confirmed via the repository's LICENSE file and matching the PyPI package metadata.

## Alternatives
grep/ripgrep (free, but no semantic ranking), already-catalogued agent code-search tools (code-index-mcp, octocode, open-codebase-index) — the differentiator here is reusing the catalogued CocoIndex engine plus first-party Claude Code/Grok packaging, not a novel retrieval approach.

## Risks / Limitations
- New, separate repo from the core CocoIndex engine already in the catalog — worth tracking whether it stays a first-class product or folds back into the main repo.
- Crowded niche: several agent-facing code-search MCP servers already exist in the catalog; this one's edge is packaging and engine reuse, not retrieval novelty.
- The "full" install pulls roughly 1 GB of torch/transformers dependencies for local embeddings; "slim" mode requires a cloud embedding API key instead.

## Recommendation
PROTOTYPE — worth trying directly in a Claude Code session given the low-friction skill install and the token-reduction claim; the CocoIndex engine pedigree and first-party agent packaging make it a credible pick over a from-scratch code-search setup.

## Change History
### 2026-10-10
Discovered via GitHub search (slot 4: data, search, documents, RAG). First catalog entry. Related to, but a separate repo and product from, the already-catalogued CocoIndex (cocoindex-incremental-indexing) incremental data/RAG engine.
