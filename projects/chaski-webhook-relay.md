# chaski

## Summary
A small, stateless webhook relay. Each route (`POST /hooks/{route}`) can be gated by an optional CEL expression and fans out concurrently to one or more targets — Apprise-backed notification channels (Discord, Slack, Telegram, ntfy, Pushover, and more) or generic HTTP requests — with titles, messages, provider parameters, and headers rendered per-request via sandboxed Go templates (no filesystem or network access). Inbound requests can be verified with HMAC, a shared token, or a built-in GitHub preset; optional SMTP ingestion lets email-only devices reach the same routes. Config is a single YAML file or merged `config.d` fragments, with secrets from environment variables.

## Why I Should Care
This is the small-utility pattern the radar explicitly values: a narrow problem (route an external event to the right notification channel, formatted correctly) solved once, well, instead of being re-written as a bespoke relay script every time it comes up.

## Problems It Can Remove
A bespoke webhook-to-Slack/Discord glue microservice, hand-rolled per integration, including the signature verification and templating logic that usually gets copy-pasted and subtly re-broken each time.

## Practical Uses
- Route GitHub webhook events (PR opened by a specific author, deploy failed) to different Slack/Discord channels per rule
- Turn Alertmanager/monitoring alerts into templated, severity-prioritized phone notifications for the MSSP admin-portal's own infra monitoring
- Receive device/printer/NAS/UPS email alerts and route them into the same notification channels as webhook-based alerts
- Preview a routing rule with `?dryRun=1`, or test configs offline/end-to-end with `chaski validate`/`chaski send`

## Product Opportunities
Mostly internal-tooling value rather than a product component — useful for his own infra/monitoring glue, not something to embed in a commercial product.

## Agent / Automation Opportunities
None notable — general-purpose notification routing, not agent-specific.

## Integration
Deployment: Helm (OCI chart, distroless image, rolling updates, restarts pods on config change) or a binary via Homebrew/GitHub releases.
Interfaces: HTTP webhook ingestion, CLI (`chaski validate`, `chaski send`), Prometheus metrics, optional SMTP ingestion.
Integration effort: **Low** — one YAML config file, a single binary or Helm chart.

## Architecture Notes
Templates are explicitly sandboxed from filesystem and network access, which matters for a tool that renders attacker-influenced webhook payloads into outbound messages. Config supports both a single file and a `config.d` directory of merged fragments, which scales better than one growing YAML file as routing rules accumulate. Health/readiness endpoints and structured logs are built in for running it as real infrastructure rather than a script.

## Maturity
Emerging — repo created 2026-06-20, 23 stars, 3 contributors. Actively maintained (renovate bot, recent releases up to 0.5.3) and backed by `home-operations` (the onedr0p-led self-hosted/k8s-at-home community), a credible source for infrastructure-glue tooling.

## License
AGPL-3.0, flagged per policy. Used unmodified as an internal notification-routing component is not a blocker; modifying and redistributing it as a network service would trigger source-disclosure.

## Alternatives
- n8n/Zapier-style automation tools (mainstream, excluded by policy) — far heavier for "route an event to a channel with a template"
- Hand-rolled webhook-to-Slack glue scripts — the status quo this replaces

## Risks / Limitations
- AGPL-3.0 licensing noted above
- Narrow scope — notification routing only, not useful beyond that
- 23 stars, young project (created 2026-06-20)

## Recommendation
**PROTOTYPE** (score 7.0/10) — low-cost to adopt for any existing webhook-to-notification glue script that's currently bespoke.

## Change History
### 2026-10-22
First discovered via Hacker News Show HN coverage of related webhook tooling search. Verified via GitHub API (AGPL-3.0, 23 stars, 0 subscribers, 1 fork, created 2026-06-20, 3 contributors including onedr0p). Selected as today's Small but High-Leverage Utility.
