# SherlockOps

## Summary
SherlockOps is a self-hosted, MIT, Docker-Compose alert-triage agent: it receives webhooks from Alertmanager, Grafana, Zabbix, Datadog, ELK, or Loki, posts an immediate acknowledgment, then has an LLM investigate the firing alert using tool calls against Prometheus, Loki, Kubernetes, AWS CloudWatch, GCP Monitoring, Azure Monitor, PostgreSQL, MongoDB, VMware vSphere, Yandex Cloud, DigitalOcean, or any MCP server — and delivers the root-cause analysis back into the original Slack/Telegram/MS Teams thread within seconds.

## Why I Should Care
This automates the exact first-pass triage work that otherwise falls on a human the moment an alert fires, across precisely the monitoring stack already in use (CloudWatch/AWS, Prometheus/Grafana/Loki, Postgres, MongoDB). It supports self-hosted LLM backends (Ollama, vLLM) directly, which matters for an MSSP context where sending client infrastructure data to a third-party LLM API is a real consideration, not a theoretical one.

## Problems It Can Remove
- The first 5-30 minutes of manual "is this real, what's actually happening" investigation after an alert fires.
- Context-switching across six different alert sources and three different chat tools to deliver a diagnosis.
- Building a bespoke alert-to-LLM pipeline from scratch for a single monitoring stack.

## Practical Uses
- Automatic root-cause analysis posted to a Slack thread within seconds of a Prometheus/Alertmanager alert.
- On-call triage assistant that checks pod status, replication lag, or CloudWatch metrics before a human opens a dashboard.
- A first-pass incident-investigation layer offered across an MSSP client's heterogeneous monitoring stack (the tool list spans AWS, GCP, Azure, Yandex, and DigitalOcean).
- Self-hosted-LLM-only configuration (Ollama/vLLM) when client data cannot leave the premises.

## Product Opportunities
Could be white-labeled as an alert-triage add-on inside an existing MSSP admin-portal SaaS — the webhook-in, LLM-investigate, chat-out pipeline is generic enough to sit behind a branded front end.

## Agent / Automation Opportunities
Consumes any MCP server as an investigation tool ("MCP Servers: Any MCP tool" in its tool table), so its investigation surface extends without forking the project — point it at an internal MCP server and its alert-triage agent gains that tool automatically.

## Integration
`docker compose up -d --build`, configure `.env` (LLM API key, messenger tokens) and `config.yaml` (which tools/environments/messengers are enabled), point existing alert sources at its webhook endpoints. **Integration effort: Low** — no code changes to the systems being monitored, only webhook configuration.

## Architecture Notes
Two-phase design: Phase 1 (<100ms) posts the raw alert immediately so on-call is never left waiting on an LLM call; Phase 2 (5-30s) runs the LLM-with-tool-calls investigation and updates the same message (Slack thread reply, Telegram message edit, Teams card update) rather than spamming a second message. Slack supports a Socket Mode listener so the bot can also react to `@bot analyze` mentions without exposing a public webhook endpoint.

## Maturity
Very early and effectively unvalidated: created 2026-04-01, 18 stars, 1 fork, 0 subscribers, 1 open issue, last pushed 2026-10-05. Tagged releases exist (latest v1.2.8, 2026-09-17) showing active iteration, but there is no independent community signal beyond the single maintainer (Duops) at review time.

## License
MIT, confirmed via GitHub API. No restrictions.

## Alternatives
PagerDuty and Opsgenie's AI-assisted triage features (proprietary, per-seat, locked to their own alerting), Rootly/Blameless AI incident-response add-ons (proprietary), or the status quo of manual on-call triage.

## Risks / Limitations
- 18 stars / 0 subscribers / single maintainer — treat as an unproven experiment, not an on-call dependency, until it accumulates track record.
- The breadth of integrated tools (12+ systems) is documented but not independently verified here beyond the README's own tool table — depth of each integration is unconfirmed.
- Defaults to sending alert and investigation context to a third-party LLM API (Claude/OpenAI) unless explicitly configured against a self-hosted Ollama/vLLM endpoint — a real consideration for MSSP client data.
- No stated production-readiness claim either way.

## Recommendation
PROTOTYPE — the design (tool-calling LLM investigation across a wide, already-relevant monitoring stack, with a credible self-hosted-LLM escape hatch) is worth a real trial against a non-critical alert channel before trusting it with anything on-call-critical.

## Change History
### 2026-11-02
First reviewed and catalogued during slot 6 (infrastructure/observability/deployment) research. Confirmed via GitHub API (MIT, 18 stars, 1 fork, 0 subscribers, created 2026-04-01) and the project README's tool/feature tables.
