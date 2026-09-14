# Pane

## Summary
A terminal-first desktop app (Mac/Windows/Linux) for running multiple CLI coding
agents — Claude Code, Codex, Cursor Agent, Aider, Goose, or anything that runs in a
terminal — in parallel, one per git worktree, with the entire worktree lifecycle
(create, clean up, rebase from main) made invisible. "Remote Pane" extends this to a
self-hosted daemon: agents keep running on a home server or VM while controlled from
desktop or a phone browser.

## Why I Should Care
Running several coding agents in parallel via git worktrees is the correct pattern,
but the CLI ergonomics (`git worktree add/remove`, tracking which worktree is on which
branch, cleaning up stale ones) are bad enough that most people skip it and just juggle
terminal tabs instead. Pane makes the correct pattern the path of least resistance,
directly matching the AI-agent-leverage axis of this radar.

## Problems It Can Remove
- Manual git-worktree bookkeeping when running parallel coding-agent sessions.
- Ad hoc tmux/shell-script setups for managing multiple agent terminals.
- Losing track of which agent is blocked waiting on approval vs. still working —
  Pane surfaces per-pane status (blocked/working/done) at a glance.
- Having to be at your desktop to check on or approve a long-running agent task.

## Practical Uses
- Run 3-4 Claude Code sessions in parallel on separate features/branches without
  hand-managing worktrees.
- Kick off a long agent task on a home server or cloud VM via Remote Pane, then check
  and approve it from a phone browser.
- Give an agent itself a CLI contract (`runpane repos add`, `runpane panes create`) so
  it can register a repo and spawn its own follow-up work panes.

## Product Opportunities
The underlying pattern — invisible worktree lifecycle, per-pane isolated port ranges,
status-dot rollups across many parallel agent sessions — is worth studying directly
for any internal multi-agent tooling built for the MSSP/PTaaS side of the business.

## Agent / Automation Opportunities
This *is* an agent-orchestration tool: agent-agnostic by design (anything that runs in
a terminal works, no plugin/SDK needed), and it exposes an agent-operable CLI
(`runpane agent-context`) so a coding agent can discover Pane's own command schema and
drive it programmatically.

## Integration
Quick install via shell script, npx/pnpm/pipx, or platform installers (Mac/Windows/
Linux). Remote Pane adds itself as a systemd-style daemon on a headless VM
(`npx runpane install daemon`) with Tailscale or SSH tunneling for connectivity.
**Low** integration effort for the desktop app; Remote Pane setup is a few more steps
but still scripted end-to-end.

## Architecture Notes
Each coding-agent session gets its own git worktree, its own isolated port range (so
multiple dev servers never collide), and automatic `.env`/secrets mirroring so the
worktree is immediately runnable. Remote Pane separates the host machine (which holds
repos, terminals, git state, files, agent credentials, and compute) from thin clients
that connect via a `pane-remote://` code — desktop app or browser.

## Maturity
Emerging — created February 2026, 460 stars, active Discord, 10+ contributors, and a
fast release cadence (v2.4.105 as of this review, pushed the same day). 106 open issues
against a still-small contributor base is a moderate maintenance-debt signal to watch.

## License
**AGPL-3.0**, confirmed by reading the raw `LICENSE` file directly (GitHub's API
reports `NOASSERTION` for this repo). Flagged per policy. For personal desktop/dev-tool
use this carries no practical exposure; it would only matter if Pane's own code were
modified and redistributed as a hosted service.

## Alternatives
- **captain-claw** (kstevica) — newer (163 stars), focused on ensemble reasoning across
  agent fleets rather than terminal/worktree UX.
- **Vibe Kanban**, **Conductor** — commercial/hosted multi-agent orchestration UIs.
- Plain tmux + hand-rolled git-worktree shell scripts — what Pane directly replaces.
- The broader "coding-agent orchestration" space surfaced dozens of near-identical
  entrants this run (neutron, orchestra, pixcode, maestrus, zimmer, kaos-control,
  threadcells, and more) — Pane stood out on real usage (460 stars vs. single digits
  for most), release history, and a genuinely differentiated remote-control feature.

## Risks / Limitations
- Young, fast-moving project — expect breaking changes between releases.
- Open-issue count relative to contributor base suggests some maintenance lag.
- AGPL-3.0 source-disclosure obligations apply if modified and redistributed as a
  network service.

## Recommendation
**PROTOTYPE** — worth running for a week on real parallel-agent work before deciding
whether it earns a permanent spot over a manual tmux/worktree setup.

## Change History
### 2026-09-14
Initial discovery and review. Catalogued as PROTOTYPE, 8.0/10.
