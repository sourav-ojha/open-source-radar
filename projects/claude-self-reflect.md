# claude-self-reflect

## Summary
A persistent memory layer for Claude Code: one embedded Rust binary (~138MB, bundled
SQLite, no external database, containers, Python, or API keys) that stores session
history and exposes it back to Claude via MCP tools — a start-of-session recap,
"receipts" for completed work, periodic re-checks of old conclusions against git
history ("dreams"), and compacted evidence carried across context resets.

## Why I Should Care
Directly targets the actual failure mode in long unattended or multi-day Claude Code
sessions — including this radar's own daily run — where useful context is lost to
compaction or a new session start, forcing Claude to re-derive state it already worked
out.

## Problems It Can Remove
Re-reading scrollback or re-explaining prior decisions at the start of a new session;
manually tracking whether an earlier conclusion still holds as the codebase changes
underneath it.

## Practical Uses
- Resume a multi-day Claude Code project with a one-paragraph recap instead of
  re-reading scrollback.
- Have Claude periodically re-check old conclusions against the actual git history and
  surface contradictions.
- Carry "receipts" (what got done, what was settled, what's open) across a context
  compaction instead of losing state.
- Audit what Claude decided and why on a past session without reconstructing it from
  memory.

## Product Opportunities
None identified — this is internal tooling for an agent workflow, not a redistributable
component.

## Agent / Automation Opportunities
Purpose-built MCP tool surface for Claude Code specifically, not a generic
vector-memory layer retrofitted onto agents. The "dreams" mechanic (periodic
re-verification of stored conclusions against current repo state) is a pattern worth
copying for any long-running agent with persisted state.

## Integration
npm install, no server to run or API key to configure. Low effort.

## Architecture Notes
Single Rust binary with an embedded SQLite store (the `csr-engine` component) instead
of a client/server vector database — avoids the usual "stand up Qdrant/Chroma plus an
embedding API key" tax that most agent-memory tools impose.

## Maturity
Emerging: created 2025-07-25 (~16 months), 231 stars, 29 forks, 3 subscribers, 13 open
issues. Actively released — v10.1.0 published the same day as this review
(2026-11-21). Thin external validation relative to how central it would become to a
daily workflow.

## License
MIT — no restrictions.

## Alternatives
Planning with Files (already catalogued) — file-based plan/task persistence rather
than a dedicated memory binary with reflection/verification built in. Generic
vector-DB memory layers (mem0, Zep) — heavier, not Claude-Code-specific, require a
server/API key.

## Risks / Limitations
Single-maintainer project; the bundled Rust engine is new enough that breaking changes
across versions are plausible. Not independently verified against a real multi-day
session yet.

## Recommendation
USE NOW — install it and run one real multi-day Claude Code session through it before
depending on it for anything that matters.

## Change History
### 2026-11-21
First reviewed and catalogued.
