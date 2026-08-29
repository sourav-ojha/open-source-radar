# Laminar

## Summary
Open-source, OpenTelemetry-native observability platform purpose-built for AI agents:
tracing, plain-English behavioral "Signals" with Slack alerting, evals, MCP/CLI access
for a coding agent to query its own traces, and a dashboard builder. Rust core,
ClickHouse-backed for high trace-compression. Self-hostable via Docker Compose. YC S24.

## Why I Should Care
It closes a real gap: once you have more than a couple of agent workflows running
(coding agents, automation scripts, or this very radar), "what did the agent actually
do and why" becomes hard to answer from logs alone. Laminar gives that a queryable,
self-hosted trace store plus natural-language alerting, with an MCP interface a coding
agent can query directly to self-diagnose failures.

## Problems It Can Remove
- Ad-hoc print-statement/log-scraping debugging of agent runs
- Building a custom OTel trace store and Slack-alerting layer from scratch

## Practical Uses
- Trace and debug any OTel-instrumented agent run with a self-hosted dashboard
- Define plain-English "Signals" (e.g. "agent is stuck in a loop") and get pinged in
  Slack instead of manually reviewing traces
- Run evals in CI against traced agent behavior before shipping a prompt/workflow change
- Let a coding agent query its own traces via MCP to self-diagnose an issue

## Product Opportunities
Audit-trail/observability layer for any agent-driven feature — relevant to the
compliance instincts of the MSSP/PTaaS side of the work if agent behavior ever needs to
be demonstrably reviewable.

## Agent / Automation Opportunities
Purpose-built for instrumenting and debugging agent workflows generally, including
this radar's own future runs — the MCP/CLI trace-query interface is designed
specifically so a coding agent can investigate its own past behavior.

## Integration
`git clone` + `docker compose up -d` for a lightweight quickstart; a fuller
`docker-compose-full.yml` for production. **Medium** integration effort: this is a real
multi-service stack (Postgres + ClickHouse + app-server), not a single container, and
needs an LLM provider configured for the chat-with-trace/SQL-with-AI features.

## Architecture Notes
Claims 20x average trace compression (up to 50x on long runs) via message hashing and
deduplication — worth reading the linked writeup as an architecture reference for
anyone building trace storage at scale, independent of adoption. Custom realtime engine
for viewing traces as they happen, plus full-text search over span data.

## Maturity
Emerging, but not a flash in the pan: created 2024-08-29 (exactly 2 years old as of
this review), 3,211 stars, pushed 2026-08-29 (same day as this review), latest release
v0.2.2 on 2026-08-25. Consistent activity over 2 years rather than a recent spike.

## License
Apache-2.0 for the self-hosted image — unrestricted for commercial use. Managed cloud
(laminar.sh) is the paid alternative but not required to use the OSS stack.

## Alternatives
- **Langfuse** (MIT) — most-adopted comparable tracer, lacks built-in Signals/Slack
  alerting.
- **MLflow tracing** (Apache-2.0) — broader ML platform, heavier footprint if only
  agent tracing is needed.
- **LangWatch**, **OpenLLMetry** (both Apache-2.0) — narrower scope than Laminar's
  combined tracing+evals+Signals+MCP surface.

## Risks / Limitations
- Self-hosting is a real stack, not a single container — budget Medium, not Low, setup
  effort.
- Self-hosted deployments phone home anonymized telemetry by default (opt out via
  `LAMINAR_TELEMETRY_DISABLED`) — check before pointing at anything sensitive.
- YC-backed startup — roadmap could tilt toward the managed cloud product over time.

## Recommendation
**PROTOTYPE** — worth standing up against one real agent workflow (a good first
candidate: this radar's own runs) before deciding whether it earns a permanent place in
the stack.

## Change History
### 2026-08-29
First catalogued. GitHub API verified: Apache-2.0, 3,211 stars, pushed_at 2026-08-29,
not archived, created 2024-08-29.
