# mcp-memory-service

## Summary
A self-hosted, framework-agnostic persistent memory backend for AI agents — REST API,
MCP (local and remote with OAuth 2.0 + Dynamic Client Registration), CLI, and a
dashboard over one service — with a typed causal knowledge graph (causes/fixes/
contradicts edges) and local ONNX embeddings, so memory never leaves your
infrastructure.

## Why I Should Care
It is the most mature entrant seen so far in an otherwise fast-saturating "agent memory"
niche: roughly two years old (created 2024-12-26) against a wave of competitors that are
weeks-to-months old, with 2,056 stars and 369 forks, and production-oriented features
(remote MCP with real OAuth, SSE events, per-agent scoping) the newer entrants don't yet
have.

## Problems It Can Remove
Re-explaining project context and architecture decisions to a coding agent every new
session; hand-wiring Redis + a vector DB as a shared memory store across multiple agent
frameworks; building OAuth-gated remote access to a memory service from scratch.

## Practical Uses
- Give Claude Code / Claude Desktop cross-session memory of architecture decisions and
  code patterns
- Share one memory backend across LangGraph, CrewAI, AutoGen, and Claude via the same
  REST API
- Scope memories per agent identity in a multi-agent setup via the `X-Agent-ID` header
- Query typed causal relationships between past decisions (not just flat facts)
- Expose remote MCP access to teammates' agents behind real OAuth 2.0 + DCR instead of a
  bare unauthenticated endpoint

## Product Opportunities
A shared-memory layer behind an internal multi-agent tool, with OAuth-gated remote
access for a team's coding agents.

## Agent / Automation Opportunities
Native MCP server with remote MCP + OAuth 2.0/DCR support, so it can be safely exposed
beyond localhost. SSE events give real-time notification when any agent stores or
deletes a memory, useful for coordinating multiple agents sharing one memory space.

## Integration
`pip install mcp-memory-service`, running in under 60 seconds per the project's own setup
guide. Embeddings run locally via ONNX, so no cloud dependency for the core memory
function. Low integration effort.

## Architecture Notes
Knowledge graph stores typed causal edges between memories (causes, fixes, contradicts)
rather than just flat key-value facts, letting an agent reason about relationships
between past decisions. Autonomous consolidation compresses older memories to manage
context-window limits.

## Maturity
Mature relative to the niche. Created 2024-12-26 (~2 years old), latest release v12.4.0
(2026-10-26) — actively released. 2,056 stars, 369 forks, 12 subscribers at review time.

## License
Apache-2.0. No restrictions on commercial or SaaS use.

## Alternatives
Hippo, CortexDB, deja-vu, MuninnDB (already catalogued agent-memory tools, all
weeks-to-months old). This is the oldest and most-starred entrant found in the niche so
far, with features (OAuth2+DCR remote access, typed causal edges) none of the others
currently have.

## Risks / Limitations
- The project's own marketing (dedicated website, demo video of a "3D knowledge graph")
  is more polished than most entries in this niche — worth confirming the
  causal-consolidation claims hold up under real multi-agent load before relying on it
  for anything business-critical
- Saturated niche — evaluate directly against already-catalogued Hippo/CortexDB/deja-vu
  for the specific workflow rather than adopting more than one memory backend at once

## Recommendation
USE NOW — low adoption risk given the track record, permissive license, and a 60-second
local install; the clearest "try this first" pick among the catalogued agent-memory
tools.

## Change History
### 2026-10-31
Initial discovery and cataloguing (slot 4: data/search/documents/RAG).
