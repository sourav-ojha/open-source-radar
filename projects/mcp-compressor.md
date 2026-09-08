# mcp-compressor

## Summary
An MCP server wrapper, usable as a CLI proxy or embedded directly from Python, TypeScript,
or Rust, that lets agents use large MCP servers without spending huge amounts of context on
tool descriptions and schemas. The model sees a compact tool surface first, then asks for
the full schema only for the tool it actually selects. Built and maintained by Atlassian
Labs.

## Why I Should Care
Any MCP server with dozens or hundreds of tools (Atlassian's own products are the
motivating example, but any sufficiently large self-built server hits the same wall) can
burn thousands of tokens on tool descriptions before an agent does anything useful. This is
a direct, recurring cost for anyone building on top of multiple MCP servers, not a
hypothetical one.

## Problems It Can Remove
Removes the need to hand-roll tool filtering or a bespoke progressive-disclosure layer in
front of a large MCP server, and removes the "adding one more MCP server blows the context
budget" failure mode entirely.

## Practical Uses
- Front any large third-party MCP server (Atlassian, GitHub, etc.) to cut the fixed
  per-request token overhead of its tool list before an agent takes a single action.
- Compress a self-built MCP server's tool surface as it grows, without changing how the
  server itself is implemented.
- Embed compression directly into a TypeScript/Python app's own in-process tool
  definitions, AI-SDK style, rather than only proxying external servers.
- Use CLI Mode / Code Mode to get shell or Python/TS function equivalents of MCP tools for
  non-agent automation.

## Product Opportunities
Ship a product that exposes many MCP tools without forcing every integrating agent to pay
the full schema cost up front — relevant if any MERN/AWS-adjacent tool built here is ever
exposed as an MCP server with a non-trivial number of tools.

## Agent / Automation Opportunities
Directly reduces per-request token cost and dollar cost for any agent workflow that talks
to MCP servers with large tool surfaces. Supports OAuth for remote streamable-HTTP
backends and adjustable compression levels (low/medium/high/max).

## Integration
Low effort: run as a CLI proxy in front of an existing MCP server's launch command, or
import the SDK directly in Python/TypeScript/Rust. No infrastructure beyond the proxy
process itself.

## Architecture Notes
Two patterns: (1) a compressed MCP proxy exposing mainly `get_tool_schema` and
`invoke_tool` to any MCP client, and (2) a local session proxy for embedding compression
directly into an app's own SDK-driven tool calls. Both funnel through the same
`CompressorClient` concept across languages.

## Maturity
Emerging as a repo (created 2026-03-04), but backed by Atlassian Labs, which meaningfully
lowers abandonment risk relative to a typical solo project. 118 stars, 11 forks, 5
contributors, v0.31.10 released 2026-09-07.

## License
Apache-2.0 — no restrictions on commercial use, redistribution, or embedding.

## Alternatives
Hand-rolled tool filtering or manual schema trimming; building progressive-disclosure
patterns bespoke to each MCP server integrated.

## Risks / Limitations
Adds a proxy hop between client and backend MCP servers that must stay in sync with
backend schema changes. The repo's own history is short despite the reputable backing org.

## Recommendation
USE NOW — low-risk given the Atlassian Labs backing, and solves a problem that gets worse,
not better, the more MCP servers get integrated.

## Change History
### 2026-09-08
Initial discovery and catalog entry.
