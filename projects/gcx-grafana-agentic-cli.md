# gcx (Grafana CLI)

## Summary
`gcx` is Grafana Labs' own official, Apache-2.0, Generally Available CLI for Grafana OSS, Enterprise, and Cloud, built specifically to also work inside agentic coding environments: it gives shell and agent access to dashboards, alerts, SLOs, metrics, logs, and traces, plus a suite of pre-built agent skills for alert investigation, dashboard-as-code/GitOps, and SLO management.

## Why I Should Care
Grafana itself is a household name excluded from this radar by policy, but `gcx` is a materially new capability on top of it: it turns "investigate this alert" from a task that requires opening a dashboard into a shell command or agent skill that can be run from the terminal or a CI pipeline. For infrastructure already on Grafana/Prometheus/Loki (a common pairing with EKS), this directly extends agent-driven ops rather than requiring a human in the loop for every triage pass.

## Problems It Can Remove
- Manually opening Grafana to check a metric, log line, or trace before answering "is this actually broken?"
- Hand-writing scripts against the raw Grafana HTTP API to manage dashboards as code.
- A missing shell-native path for agents that can run commands but don't have an MCP client configured — Grafana's own MCP server already exists for that case, `gcx` covers the CLI-only path.

## Practical Uses
- A coding agent queries production metrics/logs/traces and proposes a root cause before a human opens a dashboard.
- `gcx resources pull/push` for dashboard-as-code / GitOps workflows instead of clicking through the UI.
- SLO and alert-rule management scripted from CI (`gcx alert rules`, `gcx resources push alertrules`).
- Bundled agent skills for alert investigation and observability setup, usable by any agentic coding tool, not just one vendor's.

## Product Opportunities
None directly — this is an operations tool, not a building block for a product.

## Agent / Automation Opportunities
Ships dedicated, documented agent skills (alert investigation, dashboard creation, SLO management, observability setup) rather than expecting an agent to reverse-engineer the CLI surface from `--help` output — a deliberate design choice the README calls out explicitly ("gcx works with any agentic coding tool").

## Integration
Single Go binary; works against any Grafana 12+ deployment (most features need Grafana 13+). Requires Grafana already being part of the stack — no standalone value otherwise. **Integration effort: Low** for anyone already running Grafana.

## Architecture Notes
Deliberately split from the two other Grafana AI surfaces: `gcx` for shell/scripts/CI, the separate Grafana MCP server for MCP-protocol clients without shell access, and Grafana Assistant for the in-product chat experience. Self-hosted signal queries (`gcx metrics/logs/traces query`) work against Prometheus/Loki/Tempo/Pyroscope directly but require manually configuring data-source endpoints on OSS/Enterprise, unlike Cloud's auto-discovery.

## Maturity
Generally Available per its own status badge, Go 1.26+, active CI. Created 2026-03-23, already at 811 stars / 65 forks / 7 subscribers with an active weekly release cadence (v1.5.0 → v1.6.0 → v1.7.1 within three weeks) and 346 open issues — a lot of real usage and churn for an eight-month-old tool.

## License
Apache-2.0, confirmed via GitHub API. No restrictions. Note that several commands (SLO, Synthetic Monitoring, IRM, k6, Fleet, Assistant) only function against paid Grafana Cloud tiers — a Grafana Cloud billing gate, not a `gcx` license restriction.

## Alternatives
Grafana's own MCP server (same ecosystem, MCP protocol instead of shell commands — complementary, not competing), Grafana Assistant (in-product LLM, Cloud-feature-gated), or hand-rolled scripting against the raw Grafana HTTP API.

## Risks / Limitations
- Entirely dependent on already running Grafana; zero value as a standalone tool.
- The underlying Grafana API surface is mid-migration to "app-platform" (Kubernetes-style) API groups, so some commands need explicit feature toggles on Grafana 12 — a moving target worth re-checking before relying on a specific command.
- Cloud-only feature gating isn't obvious from the CLI name alone; easy to assume a command works self-hosted when it doesn't.

## Recommendation
PROTOTYPE — worth wiring into an agent's toolbelt specifically if Grafana/Prometheus/Loki is already running; no value otherwise.

## Change History
### 2026-11-02
First reviewed and catalogued during slot 6 (infrastructure/observability/deployment) research. Confirmed via GitHub API (Apache-2.0, 811 stars, 65 forks, 7 subscribers, created 2026-03-23, GA status, official Grafana Labs org) and the project README.
