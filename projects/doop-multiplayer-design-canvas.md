# Doop

## Summary
Doop is an open-source, self-hostable multiplayer design canvas (an alternative to
Paper.design) where humans and AI agents co-design on the same canvas in real time via a
built-in MCP server. Frames render real HTML in sandboxed iframes; everyone sees
cursors, presence, edits, and agent status live over one WebSocket room.

## Why I Should Care
It is a rare example of a real-time collaborative tool built agent-first rather than
retrofitted: agents get the same multiplayer presence, cursors, and MCP-driven edit
surface as a human collaborator. That pattern — not necessarily the product itself — is
the valuable part for a MERN/Postgres builder.

## Problems It Can Remove
Not a direct replacement for anything in Sourav's current stack; its value is as a
collaborative mockup/prototyping surface and as an architecture reference for real-time
agent-collaborator tooling.

## Practical Uses
- Let an AI agent sketch and self-review a UI design live on a shared canvas instead of
  a prompt-screenshot-refresh loop.
- Run a collaborative mockup review for an admin-portal redesign with both a human and
  an agent editing the same canvas.
- Build clickable prototypes (frame-to-frame navigation, overlays, transitions) without
  a separate prototyping tool.

## Product Opportunities
The "design memory distiller" pattern — pin exemplar frames, capture decisions, let the
system propose durable style rules every agent must follow — is a reusable idea for any
product where an agent needs to stay visually consistent over time.

## Agent / Automation Opportunities
Built-in MCP server works with Claude Code, Codex, ChatGPT, Cursor, OpenCode, or any MCP
client; headless agents (n8n, Mastra, CI) can connect via an agent-key bearer header
without a human client at all.

## Integration
`docker compose up` (embedded Postgres, no external services required) or `bun run dev`
for local development. Low integration effort for trying it; a hosted version exists at
doop.design.

## Architecture Notes
The combination worth studying: a WebSocket-based multiplayer layer (cursors, presence,
per-frame editing indicators), sandboxed-iframe frame rendering for arbitrary HTML, and a
general MCP server that treats an agent as a first-class collaborator rather than a
special-cased bot account.

## Maturity
Experimental: created 2026-08-22 (~3 months), 2,256 stars, 286 forks, 9 subscribers, 77
open issues. Desktop app already at v0.14.0 (released 2026-11-16) — fast-moving despite
its age, with CI and an active Discord.

## License
AGPL-3.0. Flagged per AGENT.md §14: self-hosting internally is fine, but offering it as
a hosted/modified service to others, or embedding it in a closed-source product, would
trigger copyleft obligations to release the modified source.

## Alternatives
Paper.design (proprietary), Figma (proprietary, no native agent-as-collaborator model),
tldraw (open-source canvas, no built-in MCP/agent-presence layer).

## Risks / Limitations
- Very young (~3 months) despite a fast release cadence — treat as experimental, not
  production-ready.
- AGPL-3.0 is a real constraint for commercial embedding beyond internal use.
- Not a direct fit for Sourav's current product work — valuable mainly as an
  architecture reference.

## Recommendation
STUDY — the architecture (multiplayer + sandboxed iframes + agent-as-collaborator MCP)
is worth learning from; direct adoption is not an obvious fit for current product work.

## Change History
### 2026-11-22
Initial discovery and review.
