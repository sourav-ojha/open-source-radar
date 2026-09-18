# coding-agent-search (cass)

## Summary
A unified, high-performance TUI/CLI that indexes and full-text-searches local session history across 20+ coding-agent harnesses (Codex, Claude Code, Gemini CLI, Cursor, Aider, Copilot Chat/CLI, and more) into one searchable timeline, with an explicit `--robot`/`--json` machine-readable mode designed to be driven by another agent.

## Why I Should Care
Working across multiple coding-agent CLIs day to day scatters the useful trail of "what was tried and why" across a dozen incompatible log formats. cass normalizes all of them into one searchable index.

## Problems It Can Remove
Removes manual scrollback-scrolling or per-tool log exports when trying to recall what a past agent session did or decided.

## Practical Uses
- Find "what did I ask Claude Code to do about that performance regression last week" across sessions.
- Point a coding agent at cass in robot mode so it can search its own or a different agent's prior session history for context before starting a new task.
- Audit which agent harness produced a specific past change when juggling several tools on the same machine.
- Exclude a noisy agent harness from indexing (`cass sources agents exclude <name>`) to keep the index relevant.

## Product Opportunities
None — internal productivity tooling.

## Agent / Automation Opportunities
Explicit "Agent Quickstart (Robot Mode)" documentation defines a machine-safe usage contract (`--robot --json`, `capabilities --json`, `robot-docs schemas`) — built from the start to be queried by another agent, not just a human at a terminal.

## Integration
Prebuilt binary via curl/PowerShell installer, Homebrew tap, or Scoop (Windows). Rust-nightly pinned toolchain only needed for building from source. Low integration effort for the common path.

## Architecture Notes
Rust-backed indexer normalizing 20+ divergent session-log formats into one schema; ships both an interactive TUI and a scriptable JSON API for agent-to-agent use.

## Maturity
Experimental — explicitly labeled alpha status, though the same maintainer's other tool (Destructive Command Guard) shows a track record of shipping polished, actively maintained CLIs.

## License
MIT License with an OpenAI/Anthropic Rider (custom) — same clause as Destructive Command Guard, same author. Effectively MIT for ordinary use.

## Alternatives
Grepping raw session log/JSONL files by hand (status quo), or relying on each agent's own siloed, non-cross-searchable built-in history.

## Risks / Limitations
- Alpha status — schema and CLI surface may still change.
- Single-maintainer bus-factor consideration.

## Recommendation
PROTOTYPE — worth trying if regularly switching between more than one or two coding-agent CLIs; low install cost, alpha maturity means treat output as a convenience layer rather than a system of record.

## Change History
### 2026-09-18
Initial discovery and review. Same author and license rider as Destructive Command Guard, both catalogued the same run on independent merit. Rotation slot 3 (developer utilities, debugging, testing).
