# narwhal

## Summary
narwhal is a single static Rust binary combining a vim-mode terminal SQL editor,
DBA-grade tooling (dialect-aware schema diff with DDL emission, append-only audit
log, capability-gated plugin sandbox, streaming cancellable queries, ER diagrams,
SSH tunnels, a secret vault), and a built-in MCP server that exposes saved
connections, schema, and query execution to Claude, Cursor, Zed, and other
MCP-aware clients. Supports Postgres, MySQL, SQLite, DuckDB, ClickHouse, and SQL
Server.

## Why I Should Care
It collapses two separate tools — a terminal DB client and an agent-facing
database bridge — into one binary with no adapter or proxy to maintain. For the
Postgres side of the stack specifically, this replaces both a GUI DB client and a
hand-rolled MCP-to-database bridge with one install.

## Problems It Can Remove
- Running a separate GUI DB client (DBeaver/TablePlus) alongside a terminal
  workflow.
- Hand-writing or wiring a database-access MCP server for an AI agent to safely
  query a real database.
- Hand-writing dialect-aware ALTER statements from a manual schema comparison.

## Practical Uses
- Day-to-day Postgres/MySQL querying and schema browsing from the terminal
  instead of switching to a GUI tool.
- Point an AI coding agent at a real database read-only via `narwhal mcp
  --read-only` for schema exploration during feature work.
- Generate a dialect-aware DDL diff before writing a migration by hand.
- SSH-tunnel into a production-adjacent database for a one-off query without a
  separate tunneling tool or port-forward script.

## Product Opportunities
None identified — this is a personal/team developer tool, not embeddable product
infrastructure.

## Agent / Automation Opportunities
Built-in MCP server over stdio with a documented read-only mode and per-connection
guards — the most direct fit on this radar's AI-agent-leverage axis for the
database-tooling category: `narwhal mcp --read-only` and it's wired into Claude
Desktop, Cursor, Zed, Continue, or Aider with no separate proxy.

## Integration
Single static binary via install script, Homebrew, AUR, Nix flake, or
`cargo install narwhaldb`. Integration effort: **Low** — no runtime dependencies,
connects directly to existing database connections.

## Architecture Notes
The plugin system is capability-gated and sandboxed (Lua + WASM), which is a
reasonable reference point if ever building a similarly extensible internal tool
that needs to run untrusted or semi-trusted user scripts against sensitive
connections without granting them the whole process's privileges.

## Maturity
Emerging: created 2026-05-27 (~3.5 months old at review), 43 stars, latest tagged
release v2.3.0 (2026-06-09), 6 open issues, green CI badge, distributed through
multiple real channels (crates.io, Homebrew, AUR, Nix) rather than just a GitHub
release page.

## License
MIT OR Apache-2.0 (dual-licensed, standard Rust-ecosystem convention) — no
restrictions. The GitHub API reports the license as `NOASSERTION` because it
doesn't parse the dual `LICENSE-MIT` + `LICENSE-APACHE` file pair; confirmed
genuine by inspecting the repo's file listing directly.

## Alternatives
DBeaver, TablePlus, pgAdmin (GUI-first, no built-in MCP surface). LibreDB Studio,
already catalogued (2026-09-06, USE NOW), covers a broader range of data formats
(Parquet, Excel, Arrow) as a desktop app but also lacks MongoDB support.
thorstenfoltz/octa (desktop app, 20+ data formats, also ships an MCP server) is a
closer direct competitor surfaced in the same search pass but wasn't
independently deep-reviewed this run.

## Risks / Limitations
- No MongoDB support at all — the primary database in the MERN stack this radar
  optimizes for falls outside its scope; only useful for the Postgres/MySQL side
  of the work.
- Young and small (43 stars, appears to be a single-maintainer project) — real
  bus-factor risk.
- The MCP read-only guard is the safety net for pointing an agent at it; worth
  confirming it's actually enforced before using it against anything
  production-adjacent.

## Recommendation
PROTOTYPE — worth trying as the terminal Postgres client plus MCP bridge for
agent-assisted schema work, with the clear caveat that it doesn't cover MongoDB
and so is a partial, not full, database-tooling replacement.

## Change History
### 2026-09-11
Initial discovery and review. Slot 3 (developer utilities, debugging, testing) run.
