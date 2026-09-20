# Pad

## Summary
A single-binary project-management tool ("Linear/Jira for the agent era") that bundles a CLI, a local web UI, and a first-class AI-agent skill, all backed by local SQLite. Coding agents (Claude Code, Cursor, Codex, Windsurf, OpenCode, GitHub Copilot, Amazon Q, JetBrains Junie) read and write tasks, specs, and decisions through natural language via an installed `/pad` skill; humans get a real Kanban/list/table web UI at `localhost:7777` with live updates. Optional hosted "Pad Cloud" adds sync and multi-user access on top of the same product.

## Why I Should Care
The recurring cost of daily coding-agent use isn't a missing task board, it's that every fresh agent session starts blank — goals, prior decisions, and current task state all have to be re-explained. Pad exists specifically to give agents a place to read and write that state themselves, with a human-visible UI on top, rather than requiring a markdown TODO file or a from-scratch explanation every session.

## Problems It Can Remove
- Re-explaining project context, goals, and past decisions to a fresh agent session.
- Ad hoc `AGENTS.md`/`TODO.md` files that agents can't query, update, or claim work from structurally.
- Losing track of what an AI agent actually changed vs. what a human did, across a project with multiple agents involved.
- The setup overhead of Linear/Jira for a solo dev or small team that just wants a task board with agent read/write access.

## Practical Uses
- Local project tracker for personal or client projects, with a CLI (`pad item create task ...`) usable directly from scripts or agent tool calls.
- Drop-in `/pad` skill for Claude Code/Cursor sessions so an agent can check "what should I work on next" and log completed work without leaving the terminal.
- Wiki-linked specs (`[[Title]]`) and a "Specify" workflow to turn a rough idea into a spec before an agent starts building.
- Multi-agent audit trail: every write is attributed to a named agent identity, useful when running more than one coding agent against the same project.
- Idea Canvas for early brainstorming that promotes directly into tasks/specs.

## Product Opportunities
The open-core model (free local SQLite tool + paid Pad Cloud for sync/teams) is a directly reusable template for an internal tool Sourav could productize the same way. The "agent-attributed activity feed" pattern (who — human or which named agent — made each change) is also a reusable idea for any admin panel where both humans and automation write to the same records.

## Agent / Automation Opportunities
This *is* an agent-automation product: a CLI, an HTTP/SSE-backed web UI, and a distributable skill/plugin (ships a `.claude-plugin` directory and a `plugin` dir) that gives any of eight supported coding-agent tools direct read/write access to project state. Genuinely closer to "MCP-adjacent agent tool" than "project management app with an API bolted on."

## Integration
Single Go binary, `brew install PerpetualSoftware/tap/pad` or Docker (Dockerfile + `docker-compose.yml`/`docker-compose.prod.yml` present). `pad init` handles setup end to end; `pad agent install` auto-detects and configures the skill for whichever coding agent is present. No account or server required for local use. **Integration effort: Low.**

## Architecture Notes
Go backend (`internal/`, `cmd/`), Svelte + TypeScript web frontend (`web/`), everything embedded into one binary (`embed.go`). SQLite storage, event-sourced activity feed, SSE for live updates between terminal and browser. Agent identity resolution has an explicit precedence order (session registration > `.pad.toml` > `$PAD_AGENT` env > runtime auto-detection) so multi-agent writes stay attributable — a well-thought-out piece of design worth studying even independent of adopting the tool.

## Maturity
Emerging. Created 2026-03-26 (~6 months old), 181 stars, 24 forks, latest release v0.16.0 (2026-09-14) — weekly release cadence and active CI (GitHub Actions, golangci-lint, pre-commit). One dominant contributor (`xarmian`, 1,825 commits) plus several smaller contributors (`mattfaltyn`, `b4rk13`, and others including a `claude` co-author entry) — single-maintainer-dominant, a real adoption risk for a tool holding project state.

## License
Apache-2.0. No commercial-use restrictions on the open core; Pad Cloud is a separate hosted offering, not a license gate on the local tool.

## Alternatives
Beads (`gastownhall/beads`, catalogued) — a Dolt-backed distributed task *graph* for agents, no human web UI, closer to a database than a product. `ctx` (catalogued) — indexes past agent session transcripts for recall, doesn't manage tasks going forward. Linear, Jira, Notion — mature cloud PM tools with only third-party/bolted-on agent integrations, not agent-native by design. `rasimme/FlowBoard` — a similar "project context layer for agents" concept, but built and packaged specifically as an OpenClaw plugin (`clawhub` install spec, `minHostVersion` pin in its `package.json`), so it is tied to that platform even though its README also mentions external agents; narrower fit than Pad's explicit multi-tool support.

## Risks / Limitations
- Single-maintainer-dominant project at 6 months old — verify continuity before depending on it for anything business-critical.
- Local-first by default; the multi-user/sync story only exists via the proprietary-hosted Pad Cloud, not in the open-source binary.
- No independent security audit found; SQLite file and SSE server are local-only by default, which limits blast radius, but self-hosting `pad server open` beyond localhost would need review.

## Recommendation
PROTOTYPE — install it against one real project already using an AI coding agent daily, and see whether the `/pad` skill actually gets used unprompted after the novelty wears off. The core idea (agents and humans sharing one project record, not a document one side writes and the other never reads) is worth testing regardless of whether this specific project matures.

## Change History
### 2026-09-20
Discovered and reviewed. GitHub API verified: Apache-2.0, 181 stars, created 2026-03-26, latest release v0.16.0 (2026-09-14), 24 forks. README and repo contents (`.claude-plugin`, `plugin`, `skills` dirs) confirm direct Claude Code plugin support in addition to the CLI/web UI.
