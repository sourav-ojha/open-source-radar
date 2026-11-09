# Upright

## Summary
Upright is a self-hosted synthetic monitoring engine from Basecamp (37signals). It runs HTTP-status, SMTP-EHLO, MCP sign-in discovery, traceroute, and Playwright-scripted browser-flow probes from one or more sites, and exposes the results as Prometheus metrics (`upright_probe_up`, `upright_probe_duration_seconds`) for AlertManager to alert on — deliberately leaving notification/alerting logic to Prometheus's ecosystem rather than building its own. Built as a Rails engine on SQLite, Solid Queue, and Kamal.

## Why I Should Care
A credible, well-known engineering team choosing to decouple probe execution from alerting — emit metrics, let Prometheus/AlertManager own notification policy — is a cleaner architecture than most self-rolled monitoring scripts. It's not a direct fit for a Node-first stack, but the design is worth studying even without adopting the Rails runtime itself.

## Problems It Can Remove
- A hosted synthetic-monitoring subscription (Checkly-style) for teams that already run Prometheus/AlertManager and Kamal
- Hand-scripted Playwright health checks with no metrics/alerting integration
- Bolting a notification channel onto a probe engine instead of reusing an existing alerting stack

## Practical Uses
- If already running Kamal for deploys and Prometheus/AlertManager for alerting, Upright slots in with no new alerting stack to learn
- Reference implementation for running Playwright browser-flow checks as a first-class probe type alongside simpler HTTP/SMTP checks
- Studying the decoupled-alerting pattern as a design to copy into any internal monitoring tool

## Product Opportunities
None identified directly — this is an internal/ops tool, not a product-building block.

## Agent / Automation Opportunities
Its "MCP sign-in discovery" probe type checks that an MCP server's auth flow is healthy — a narrow but concrete example of treating MCP endpoints as something to synthetically monitor like any other service.

## Integration
Deployed as a Rails app via Kamal to one or more VPS sites (DigitalOcean/Hetzner-style examples given). Each site is a subdomain with its own server; one site is marked `primary` for the admin UI, others can `stores_metrics` to run local Prometheus/Alertmanager. Minimum 2 vCPU/2GB RAM (4GB recommended for heavy Playwright use); port 25 must be open for SMTP probes. Auth is static admin credentials or OIDC. Integration effort: **High** — adopting this means adopting Rails/Kamal/Solid Queue as a runtime, not just running a binary or container.

## Architecture Notes
- Probe engine is metrics-only by design; alerting is explicitly out of scope and delegated to Prometheus/AlertManager
- Playwright probes capture video/log artifacts per run in addition to feeding time-series metrics
- Boot-time checks and a deploy hook refuse to run with missing secrets; pinned container image digests with a documented CVE-fix note

## Maturity
Emerging: created 2026-01-14 (~10 months old), 843 stars, 33 forks, 5 subscribers, 12 open issues, pushed 2026-11-07. Latest tagged release v0.4.0 (2026-09-07) — two months stale relative to ongoing commits, so the tag lags actual development. No adoption-scale signals beyond the Basecamp/37signals org itself backing it.

## License
MIT, no restrictions.

## Alternatives
Checkly is the hosted equivalent this directly substitutes for — the tradeoff is operational ownership/data control vs. managed convenience. Grafana Synthetic Monitoring, cloudprober, and prodzilla are narrower or more infra-heavy alternatives not deep-reviewed this run.

## Risks / Limitations
- Rails/Kamal/Solid Queue is a full foreign ecosystem to operate, against the stated bias toward JS/TS-first, Docker-deployable tools
- No version history or community/adoption signals beyond the README itself — hard to judge long-term maintenance commitment
- Real infra requirements (open port 25, 2-4GB RAM per site) rather than a lightweight add-on
- Only 5 subscribers against 843 stars, though the credible Basecamp/37signals org backing it reduces the usual inflated-engagement concern

## Recommendation
STUDY — the metrics-only, alerting-decoupled architecture and the Playwright-as-first-class-probe-type pattern are worth learning from and could inform a lighter Node-native equivalent, but the Rails/Kamal stack itself doesn't fit Sourav's stack closely enough to adopt directly.

## Change History
### 2026-11-09
First reviewed and catalogued. Slot 6 (infrastructure, observability, deployment) run. Confirmed via GitHub API (MIT, 843 stars, pushed 2026-11-07, under the basecamp org) and README inspection (raw.githubusercontent.com) covering probe types, the Prometheus-only alerting architecture, and the Kamal-based deployment model.
