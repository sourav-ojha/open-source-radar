# Open Walnut

## Summary
Open Walnut is a self-hosted, local-first web app that runs multiple Claude Code (and
Codex/Gemini/OpenCode/Goose) sessions as an "org": a CEO agent oversees per-project leads
that dispatch worker sessions, with durable sessions across SSH/browser disconnects,
per-session diff review, and notes/tasks/memory bundled into one page.

## Why I Should Care
This targets the exact failure modes of running several unattended coding-agent sessions
at once — a dropped SSH connection losing a session, no single place to see what needs a
human decision, no durable diff review — that this very radar's own daily Claude Code run
is itself vulnerable to.

## Problems It Can Remove
Replaces ad hoc tmux/terminal-tab juggling for multiple agent sessions, a separate Notion
workspace for agent-related tasks/notes, and manual patch review of agent diffs.

## Practical Uses
- Monitor several Claude Code sessions side by side instead of terminal tabs.
- Keep a long agent session alive across a dropped SSH connection or closed laptop lid.
- Review an agent's diff and send line comments back into the same session.
- Dispatch a lead + worker hierarchy against a multi-part project instead of one flat
  session.

## Product Opportunities
The "org chart of agents" dispatch pattern (CEO → project lead → workers, with a single
"needs you" surface) is a reusable idea for any internal agent-orchestration tooling,
independent of whether Open Walnut itself is adopted.

## Agent / Automation Opportunities
This is itself an agent-orchestration layer: every AI turn runs through a Claude Code (or
other supported CLI) session, with scheduled routines, a quick-question personal
assistant, and a plugin system for adding custom apps.

## Integration
One-line installer (`curl ... | sh`), Homebrew, npm, or a Mac app; no server to
provision, data lives locally under `~/.open-walnut/` in JSON/YAML/Markdown/SQLite. Low
integration effort. Requires a working Claude Code (or other supported agent CLI) login;
an Anthropic API key or Bedrock credentials are optional.

## Architecture Notes
Durable sessions across browser disconnects, dropped SSH, and even Walnut server
restarts is the most interesting engineering claim here — worth verifying directly if
adopted, since it is the main thing a lighter tmux-based setup can't offer.

## Maturity
Emerging: created 2026-02-24 (~9 months), 142 stars, 11 forks, 0 subscribers, 1 open
issue. v0.6.37 released 2026-11-20, two days before this review — fast release cadence.

## License
MIT, no restrictions on self-hosting, modification, or embedding.

## Alternatives
Bernstein (already catalogued, CLI-only orchestrator), agentOS/Rivet (already
catalogued), plain tmux + Claude Code CLI, Claude Code's own native session handling.
Open Walnut differentiates with a full web UI, durable remote sessions over SSH, and a
bundled notes/tasks/memory layer rather than a CLI-only orchestration tool.

## Risks / Limitations
- The README's "USED BY ENGINEERS AT Amazon, DoorDash" claim could not be independently
  verified and should be read as an unverified marketing claim, not evidence of
  enterprise adoption.
- Early (142 stars, 0 subscribers) — production reliability for multi-day unattended runs
  is unproven.
- It is an orchestration shell around Claude Code (or other agent CLIs), not an
  independent agent — all capability depends on the underlying CLI working correctly.

## Recommendation
PROTOTYPE — worth a real multi-day trial running this radar's own kind of unattended
agent workflow through it, specifically to test the durable-session claim, before relying
on it for anything load-bearing.

## Change History
### 2026-11-22
Initial discovery and review.
