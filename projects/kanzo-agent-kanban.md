# Kanzo

## Summary
A kanban board and an MCP server in one 4.5 MB Rust binary: a human works the board in a browser, coding agents take tasks, move cards and comment over MCP, and a terminal CLI is a third equal interface onto the same data. No account, no server process to configure, no database.

## Why I Should Care
Every self-hosted kanban/PM tool already in this catalog (Taskosaur, Kaneo, It's a Plan) is a human-first tool with an API bolted on afterward. Kanzo is built the other way around: the MCP server, the CLI and the browser board are three equal clients of the same store from day one, so an agent taking a task and a human watching it happen are literally the same operation.

## Problems It Can Remove
Replaces a hand-rolled "give my coding agent a task queue" setup — no bespoke MCP wrapper around Linear/Trello/Jira, no database to stand up, no account to create. Also works as a plain zero-setup personal kanban independent of the agent-sharing feature.

## Practical Uses
- A shared backlog a coding agent pulls its next task from while a human watches cards move in the browser.
- A Raspberry Pi as the always-on board host for several laptops' agents to share live.
- Marking tasks "Human" so agents leave them untouched until picked up manually.
- A lightweight personal task tracker with zero setup.

## Product Opportunities
The live-share/sync protocol (direct LAN share, or relay through a Pi or VPN, with auto-reconnect) is a reusable local-first sync pattern worth studying for other small team tools beyond task tracking.

## Agent / Automation Opportunities
Native MCP server (`kanzo mcp`) lets an agent take tasks, move cards and comment directly; it auto-detects the right project from the git remote of the repo it's working in. The CLI gives the same capability for scripts.

## Integration
Single static binary install (curl installer script, Homebrew path implied, or `cargo install kanzo` — published on crates.io). Starts in ~2ms, creates `~/kanzo` on first run, zero configuration. Low integration effort.

## Architecture Notes
The notable design decision is treating MCP, the browser UI and the CLI as peers of one local store rather than building a human UI first and adding an API later. The share/sync feature (direct LAN, Pi relay, or VPN) is a lightweight local-first sync protocol worth studying independent of the kanban use case.

## Maturity
Experimental. Created 2026-10-25 (9 days old at review), v0.3.0, 922 stars / 58 forks / 6 subscribers / 1 open issue. One dominant author (emanuele-em) plus 4 other contributors. Healthy stars:forks ratio (~16:1) for a project this new — not consistent with the inflated-engagement pattern seen elsewhere in this catalog.

## License
AGPL-3.0 — flagged per policy. Running it locally or on a home server for personal/team use carries no obligation. The risk is specific: offering a modified Kanzo as a hosted, multi-tenant service to third parties would require releasing that modified source under AGPL too. Fine for internal/self-hosted use; a blocker only if ever white-labeled inside a paid hosted product.

## Alternatives
Taskosaur, Kaneo, It's a Plan (self-hosted PM tools already catalogued, all human-first); Linear/Trello/Jira (proprietary, no native agent protocol); a bespoke MCP wrapper hand-built around an existing tracker.

## Risks / Limitations
- AGPL-3.0 — revisit if a hosted/multi-tenant version is ever considered.
- Very new (9 days), single dominant author — no long track record yet.
- The share/sync feature moves board data over LAN/VPN; its encryption-at-rest/in-transit specifics were not independently audited here.

## Recommendation
PROTOTYPE — try it as a real task queue for one active coding-agent project for a week before trusting it for anything beyond that.

## Change History
### 2026-11-03
First seen and reviewed. Discovered via GitHub search for recently created tools in the coding-agent space; verified license, stars/forks/subscribers and crates.io package via API directly.
