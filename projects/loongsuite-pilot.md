# LoongSuite Pilot

## Summary
LoongSuite Pilot is a local-first telemetry collector purpose-built for AI coding agents.
It discovers which coding agents (Claude Code, Codex, Cursor, and others) are installed
on a developer machine, installs the required hooks, normalizes their activity into a
shared "GenAI event" schema (token usage, sessions, tool calls, traces), and exports it to
JSONL, OTLP, HTTP, or Alibaba Cloud SLS, with a local dashboard for at-a-glance activity.

## Why I Should Care
Every coding agent logs its own activity in its own format, if at all visibly. There is no
mainstream tool that normalizes this across vendors. Given heavy day-to-day use of coding
agents, having one place to see aggregate token spend, session activity, and what tools an
agent actually invoked is directly useful — especially moving toward CTO-level
responsibilities where justifying tooling spend with real usage data matters.

## Problems It Can Remove
Removes the need to manually dig through each coding agent's own session logs to answer
"how much am I actually spending on agent X vs agent Y" or "what did the agent actually do
in that session."

## Practical Uses
- Aggregate token usage across Claude Code, Cursor, and Codex if using more than one.
- Audit exactly what commands/tools an agent ran during a session.
- Feed normalized agent activity into an existing OTel backend (e.g. paired with Monoscope
  or Cerberus, also catalogued today) for a unified engineering-observability view that
  includes AI agent cost alongside service telemetry.
- Menu-bar app on macOS gives a live glance at token/session/tool counts without opening a
  dashboard.

## Product Opportunities
None identified directly — this is an internal engineering-visibility tool rather than
something embeddable in a customer-facing product.

## Agent / Automation Opportunities
The entire tool is agent-observability infrastructure: hook-based collection across 12+
coding agents, normalized into one schema, exportable to any OTLP-compatible backend.

## Integration
Single curl/PowerShell installer script per platform (macOS/Linux/Windows), auto-detects
installed agents and installs hooks. Integration effort: **Low** to install; slightly more
to wire non-SLS export destinations since the README documents the Alibaba Cloud SLS path
more thoroughly than generic OTLP/HTTP.

## Architecture Notes
Hook/plugin-based collection per agent, normalized into a "shared GenAI event schema,"
with a local-only default (no data leaves the machine unless an export destination is
configured). Local dashboard plus a macOS menu-bar app for live counts.

## Maturity
Emerging. Created 2026-06-04 (~3 months old), backed by Alibaba, Apache-2.0, active weekly
releases (v1.6.0 as of 2026-08-27), 157 stars, 57 open issues, 10+ distinct contributors.

## License
Apache-2.0 — no commercial-use restrictions.

## Alternatives
Manually reading each agent's native session logs (status quo). Langfuse and OpenLLMetry
target LLM-application tracing, not coding-agent activity specifically, so they don't
cover the same gap. Laminar (already catalogued, 2026-08-29) has the same limitation.

## Risks / Limitations
- Installer is a curl-pipe-shell script hosted on an Aliyun OSS CDN — review the script
  before running, as with any curl|sh installer.
- Young project; API/schema may still shift.
- Non-SLS export paths (generic OTLP/HTTP/JSONL) are less thoroughly documented than the
  Alibaba Cloud SLS integration, suggesting SLS is the primary supported path.

## Recommendation
**PROTOTYPE** — install locally and let it run for a week to see whether the token-usage
visibility is worth keeping around before wiring it into any shared infrastructure.

## Change History
### 2026-08-31
Initial discovery and review. GitHub API confirmed Apache-2.0, 157 stars, created
2026-06-04, pushed_at 2026-08-31, not archived. README verified via
raw.githubusercontent.com.
