# herdr

## Summary
A single-binary Rust terminal runtime built specifically for coding agents. herdr keeps
agent terminals running in a background server across client detach, SSH drops, and
(with opt-in agent-recovery) a server/machine restart. It unifies local work and saved
SSH machines in one window, marks every pane working/blocked/idle, and exposes a
CLI/socket API so agents can spawn panes and prompt each other. It does not wrap or
replace the agents it runs — Claude Code, Codex, Cursor, OpenCode, Grok, and others run
unmodified inside it.

## Why I Should Care
This radar itself runs as a long, unattended coding-agent session — exactly the workload
herdr targets. A dropped SSH connection or a closed laptop lid killing an in-progress
agent run is a real, recurring failure mode; herdr removes it. It also directly supports
running more than one agent at once across machines, which is the direction multi-agent
workflows are heading.

## Problems It Can Remove
Removes the need for a hand-rolled tmux/screen setup plus custom scripting to know which
pane is stuck waiting on input across several parallel agent sessions, and removes the
risk of losing an agent's progress entirely when a connection drops.

## Practical Uses
- Start a long Claude Code/Codex session over SSH, detach, and reattach later from a
  different machine with the session and layout intact.
- Run agents on several remote hosts from one combined window instead of separate
  terminal tabs per machine.
- Glance at pane status (working/blocked/idle) across a fleet of parallel agent runs
  instead of tabbing through each one to check for a stuck prompt.
- Let one agent spawn a pane and prompt another agent programmatically via the socket
  API — a real building block for multi-agent orchestration.
- Extend the base runtime with marketplace plugins instead of building custom tooling
  from scratch.

## Product Opportunities
The socket API is a usable building block for a custom internal multi-agent-ops
dashboard, if that becomes worth building later.

## Agent / Automation Opportunities
This is itself agent infrastructure: a CLI/socket API designed for agents to drive
directly, plus a documented path for adding herdr support to a new agent harness.

## Integration
Single Rust binary: `curl -fsSL https://herdr.dev/install.sh | sh`, or
`brew install herdr` / `mise use -g herdr`. Windows install is explicitly beta. No
external server dependency — it runs its own background daemon on whatever machine it's
installed on. Low integration effort to try; review what the socket API is allowed to do
before giving it broad access.

## Architecture Notes
One Rust binary with a persistent background server process that owns agent terminal
sessions, independent of any attached client. Session state is restored after a
server/machine restart (the original OS processes do not survive, but supported agents
can resume). Agent integration is explicitly "it owns their terminals" rather than
wrapping/parsing agent output, which is a cleaner boundary than most "agent terminal"
tools attempt. A real third-party plugin marketplace has formed on top of it in under a
year: herdr-core, herdr-app (desktop client), herdr-sidebar (VS Code-style file
explorer/git pane), herdr-nvim, herdr-file-viewer, herdrm (native macOS console),
herdr-remote (menu bar/phone/Telegram control), herdr-mirror (unify local + remote herdr
servers), and herdr-reviewr (review an agent's diff in a pane, send comments back to
Claude Code/Codex/OpenCode/Pi) — several of these plugins have four-digit star counts of
their own.

## Maturity
Emerging but genuinely battle-tested: created 2026-03-27 (~8 months old), 51,963 stars,
4,206 forks, 117 subscribers, 528 open issues — healthy engagement at real scale, not an
inflated-star pattern. Latest tagged release v0.9.5 (2026-10-14), pre-1.0.

## License
Apache-2.0. No restrictions on commercial use, self-hosting, or embedding.

## Alternatives
- manaflow-ai/cmux (28.8k stars) — comparable ambition and scale, but built on Ghostty
  and macOS-only, a platform mismatch for a cross-platform workflow.
- tmux/screen — persistent sessions, but no agent-awareness: no working/blocked/idle
  state, no socket API for agents to drive or prompt each other.
- A broader 2026 wave of smaller "terminal for AI agents" entrants surfaced in the same
  search (Dicklesworthstone/frankenterm, wmux, cornercase-terminal, orchbee, ThinkTerm,
  horizon, keepane) — all far smaller and newer, none with a comparable plugin ecosystem.

## Risks / Limitations
- Pre-1.0 (v0.9.5); breaking changes are possible before a 1.0 release.
- Runs a background daemon with a socket API agents can drive — worth scoping what it's
  allowed to do, especially on a machine with sensitive access, before adopting it fully.
- Third-party marketplace plugins vary in maturity; vet each one before installing.
- Windows support is explicitly beta.

## Recommendation
USE NOW — install it and run one real multi-day Claude Code session through it to see
whether the detach/reconnect/crash-recovery behavior holds up before relying on it for
anything important.

## Change History
### 2026-11-20
Discovered via a `topic:terminal-multiplexer` GitHub search during slot 3 (developer
utilities, debugging, testing) discovery. Not previously in the catalog despite 8 months
of existence and 52k stars — a notable gap filled this run. Catalogued as USE NOW,
9.0/10.
