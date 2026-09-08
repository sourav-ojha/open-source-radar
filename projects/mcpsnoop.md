# mcpsnoop

## Summary
"Wireshark for MCP" — a transparent proxy that sits in the real data path between an MCP
client (Cursor, Claude Code, Codex, Claude Desktop) and an MCP server. It shows every real
JSON-RPC frame live in a terminal UI, unlike the official MCP Inspector, which connects as
its own client and never sees what the actual client sends.

## Why I Should Care
Building or maintaining MCP servers is directly on this profile's primary relevance axis.
When a tool silently isn't called, is called with the wrong arguments, or a call hangs,
the usual response is digging through logs and guessing. mcpsnoop removes that guesswork by
watching the real conversation instead of simulating one.

## Problems It Can Remove
Removes ad hoc `console.error` debugging scattered through MCP server code, and removes the
blind spot of the official MCP Inspector, which can't show what a real client actually sent
because it is not in the real data path.

## Practical Uses
- Wrap any MCP server command (`mcpsnoop -- node build/index.js`) with zero config to watch
  live JSON-RPC traffic while developing a new server.
- Auto-wrap/unwrap a specific server inside Claude Desktop's config for one debugging
  session without hand-editing JSON.
- Run as a reverse proxy in front of a streamable-HTTP MCP server.
- Gate CI on a captured session via the bundled GitHub Action, filing findings as code
  scanning alerts and failing the job on defined thresholds.

## Product Opportunities
Mostly an internal development tool rather than something to embed in a product, but worth
baking into any internal "how we build MCP servers" workflow as the default debugging step
before shipping a new tool.

## Agent / Automation Opportunities
Directly supports building and maintaining MCP servers and MCP-based agent tools — the
debugging layer underneath the agent-orchestration and MCP-server work this radar already
prioritizes. Ships as both a CLI/TUI and a GitHub Action.

## Integration
Single Go binary, installable via `go install`, Homebrew, or a release binary. No
dependencies beyond the target MCP server command itself. Low integration effort: wrap the
existing launch command, or point it at Claude Desktop's config via `mcpsnoop wrap`.

## Architecture Notes
Sits in the real data path as a transparent proxy rather than acting as its own client —
this is the key design decision that lets it see calls the model never made, or made with
malformed arguments, which a client-emulating inspector structurally cannot.

## Maturity
Emerging. Created 2026-06-27, 347 stars, 34 forks, 10 contributors (kerlenton dominant at
110 commits), active CI, v0.21.0 released 2026-08-23.

## License
MIT — no restrictions on commercial use, redistribution, or embedding.

## Alternatives
Official MCP Inspector (structurally blind to real client traffic since it connects as its
own client); manual stdout/stderr logging inside the server itself.

## Risks / Limitations
Young project — verify continued maintenance before deep reliance. Go binary dependency
rather than a pure npm/JS install, so it sits outside a typical Node toolchain.

## Recommendation
USE NOW — low-risk, immediately useful for anyone actively building or debugging MCP
servers, which this profile already does.

## Change History
### 2026-09-08
Initial discovery and catalog entry.
