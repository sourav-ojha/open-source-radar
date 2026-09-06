# OneUptime

## Summary
An all-in-one open-source observability platform: uptime monitoring, status pages,
on-call/incident management, APM/traces/metrics, log management and error tracking,
positioned to replace Pingdom, StatusPage.io, PagerDuty/Opsgenie, Datadog and Sentry with
one self-hosted app.

## Why I Should Care
Given the MSSP admin-portal SaaS and PTaaS partnership, a mature self-hosted
incident/status/monitoring stack is directly usable infrastructure, not just a curiosity —
it can replace a shelf of paid point tools with one Apache-2.0 platform.

## Problems It Can Remove
Removes the need to stitch together and pay for separate uptime monitoring, status page,
on-call/paging, APM and log-management SaaS tools.

## Practical Uses
- Self-hosted incident-response and status-page stack for the MSSP SaaS or PTaaS
  partnership, avoiding several per-tool SaaS bills
- Uptime/SSL/DNS/synthetic monitoring for client-facing services
- On-call scheduling and escalation for a small security-ops team
- Root-cause correlation across traces/logs/metrics for a product's own backend

## Product Opportunities
Could be white-labeled as the status-page/incident layer offered to MSSP clients, or used
as internal SRE tooling for a new micro-SaaS to avoid an early Datadog/PagerDuty bill.

## Agent / Automation Opportunities
The README highlights an AI agent that opens a pull request with a fix for a diagnosed
incident, verified against the repo's configured build/test commands before opening it —
a notable but newer, more speculative capability worth independently verifying.

## Integration
Docker Compose or a Kubernetes Helm chart (listed on Artifact Hub). Medium effort — it's a
multi-service platform (probes, workers, dashboards, etc.), heavier to stand up and keep
updated than a single-container tool.

## Architecture Notes
Correlates uptime probes, incidents, status-page updates, and trace/log/metric data in one
data model so an outage can be detected, routed to on-call, communicated to subscribers,
and root-caused without manually stitching together separate tools' outputs.

## Maturity
Mature. Created 2021-06-27 (4+ years), 100+ contributors (capped at the API's 100-per-page
limit — likely more), releases roughly weekly, current version 12.0.33 (2026-09-04).

## License
Apache-2.0. The README states the self-hosted app is "100% open source and free to
self-host"; a separate hosted OneUptime Cloud with its own pricing exists alongside it but
doesn't appear to gate self-hosted features.

## Alternatives
Cachet / Gatus (status pages only, no monitoring/on-call); Uptime Kuma (much lighter,
monitoring-only); Zabbix (mature but heavier, less product-shaped); the paid combination
of Pingdom + PagerDuty + Datadog + Sentry.

## Risks / Limitations
- Heavier to operate than a single-purpose tool — multiple services to run and keep
  updated
- The AI "auto-fix PR" feature is newer and more speculative than the core monitoring/
  incident features — verify independently before relying on it
- Adopting an all-in-one platform is a bigger commitment than a narrow point tool

## Recommendation
PROTOTYPE — worth standing up against a real service (even a side project) to evaluate the
on-call/status-page flow before committing it to the MSSP SaaS's production incident path.

## Change History
### 2026-09-06
Discovered during slot 5 (self-hosted SaaS alternatives) run. Verified license and version
via the GitHub API; README claims (feature list, "100% open source") read directly from
the repository, not summarized secondhand.
