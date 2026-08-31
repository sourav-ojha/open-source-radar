# Monoscope

## Summary
Monoscope is a self-hosted, OpenTelemetry-native observability platform that stores logs,
traces, and metrics in S3-compatible object storage instead of a specialized time-series
database. It layers natural-language querying (via LLMs) and scheduled AI agents that
detect anomalies and email daily/weekly reports on top of that storage.

## Why I Should Care
Storage cost is the usual reason self-hosted observability stacks end up with short
retention windows. Putting the data in S3 removes that constraint, and the NL query
layer removes the LogQL/PromQL learning curve for anyone on the team who isn't fluent in
query languages. Directly applicable to monitoring the MSSP admin-portal's own backend
services.

## Problems It Can Remove
Replaces (or reduces the need for) a Grafana Loki/Tempo/Mimir self-hosted stack, or a
Datadog/Better Stack subscription, for general service observability.

## Practical Uses
- Multi-year log/trace retention at S3 storage prices instead of trimming to 30 days.
- Ask "what happened around the time this customer reported an error" in plain English.
- Schedule an anomaly-detection agent to email a digest instead of hand-writing alert rules.
- Correlate logs, traces, metrics, and session replay for one service in one UI.

## Product Opportunities
Could underpin an internal observability capability for the MSSP SaaS — either by
self-hosting under AGPL terms (fine as long as it stays internal-only) or via
Monoscope's managed cloud tier if the tool is ever exposed to customers directly.

## Agent / Automation Opportunities
Scheduled AI agents for anomaly detection and report generation are a first-class,
built-in feature rather than something bolted on afterward.

## Integration
`git clone` + `docker-compose up` gets a local instance running; needs an S3-compatible
bucket and OpenTelemetry Collector wiring for real ingestion. CLI (`monoscope send-event`,
`monoscope telemetrygen`) for testing. Integration effort: **Medium** — straightforward to
stand up, more work to route all existing services' telemetry into it.

## Architecture Notes
Haskell backend, OTel-native ingestion, columnar storage on S3. Notable that self-hosted
and cloud modes both keep telemetry in the user's own S3 buckets — the cloud tier only
adds managed compute, SSO, and richer alert channels (Slack/PagerDuty vs. email-only
self-hosted).

## Maturity
Mature relative to most radar entries — created January 2022, actively maintained (v0.6.25
released 2026-08-07), 1,679 GitHub stars, 10+ distinct contributors, weekly-ish release
cadence, not archived.

## License
**AGPL-3.0 — flagged loudly.** Safe for pure internal self-hosted use. If modified and
re-offered as a hosted, customer-facing feature, AGPL's network-copyleft clause requires
releasing the modifications' source, or the managed cloud offering should be used instead.

## Alternatives
Grafana Loki/Tempo/Mimir stack, SigNoz, Better Stack, Datadog, Axiom. Distinct from all of
these by storing directly in S3 rather than a purpose-built time-series/log database, and
by shipping AI agents as a first-class feature rather than a marketplace add-on.

## Risks / Limitations
- AGPL-3.0 network-copyleft (see License).
- Haskell codebase raises the bar for contributing fixes or debugging deeply compared to
  a Go/TypeScript observability tool.
- Self-hosted alerting is email-only; richer channels are cloud-tier only.

## Recommendation
**USE NOW** — mature, actively maintained, low-risk to self-host for internal use, and
solves a real cost problem (long-retention observability storage) that a from-scratch
build would take months to replicate.

## Change History
### 2026-08-31
Initial discovery and review. GitHub API confirmed AGPL-3.0, 1,679 stars, created
2022-01-07, pushed_at 2026-08-31, not archived, 27 open issues. README and quick-start
verified via raw.githubusercontent.com.
