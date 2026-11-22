# IncidentRelay

## Summary
IncidentRelay is a self-hosted on-call, alert routing, event orchestration, and
incident-response platform — PagerDuty-style building blocks (rotations, escalation
policies, a service catalog, SLI/SLO tracking, OIDC/SAML SSO) run entirely on your own
infrastructure, under the Elastic License 2.0.

## Why I Should Care
PagerDuty/Opsgenie per-seat pricing is real recurring SaaS spend for any team running
on-call, and this reaches comparable feature depth — orchestration builder with
shadow-mode/replay, blast-radius analysis, dependency-aware alert correlation — while
staying fully self-hosted. Directly relevant to the MSSP/PTaaS secondary context as
internal incident-response tooling.

## Problems It Can Remove
Replaces a PagerDuty/Opsgenie subscription for internal on-call, and the ad hoc
Slack-channel incident handling that typically substitutes for it when a team doesn't
want to pay for one.

## Practical Uses
- Run on-call rotations and escalation policies for internal infra.
- Normalize alerts from Alertmanager, Datadog, Grafana, Sentry, Uptime Kuma, and more
  into one deduplicated incident queue.
- Give an MSSP's internal ops team SSO-gated incident response without exposing it
  externally as a hosted product (the license forbids that use anyway).
- Track service dependencies and blast radius for an incident instead of guessing which
  downstream services are affected.

## Product Opportunities
Internal-only: usable as the alerting/escalation backbone for the MSSP
security-admin-portal's own operations. The Elastic License 2.0 explicitly blocks
reselling it as a hosted service to third parties, so it is not a product-opportunity
candidate on its own.

## Agent / Automation Opportunities
Generic webhook intake and a PagerDuty Events API v2-compatible interface make it
straightforward to wire any existing monitoring/agent pipeline into it without a custom
integration; no native MCP/agent surface was found.

## Integration
Docker Compose, Helm/Kubernetes, RPM, or manual systemd install; SQLite for small
single-node installs or PostgreSQL for larger deployments. Medium integration effort —
real alerting infrastructure requires configuring SSO, integrations, and escalation
policies, not a one-command setup.

## Architecture Notes
The orchestration builder's shadow-mode and replay features (test a routing/escalation
change against real traffic before it goes live, with rollback via immutable published
versions) are a pattern worth noting for any rules-engine-shaped system.

## Maturity
Emerging but fast-moving: created 2026-04-29 (~7 months), 231 stars, 20 forks, 7
subscribers, 29 open issues, already at v2.5.1 (released 2026-11-15). From the roxy-wi
org (maintainers of the existing RoxyWI load-balancer admin UI), which likely explains
the unusually fast feature maturity for a ~7-month-old repo.

## License
Elastic License 2.0 — free to self-host, modify, and use internally, but may not be
offered to third parties as a hosted/managed service exposing substantially the same
functionality. Flagged per AGENT.md §14 given the MSSP/PTaaS context: this blocks
turning it into a resellable hosted feature, not internal use.

## Alternatives
PagerDuty, Opsgenie, Grafana OnCall, Better Stack, Cabot (abandoned).

## Risks / Limitations
- ~7 months old with a v2.5.1 release already — fast feature growth, but less runway
  for the stability signals (long uptime track record, large user base) that would
  normally accumulate over that version range.
- Elastic License 2.0 blocks reselling as a hosted service to third parties.
- Modest community size (7 subscribers, 231 stars) relative to the feature surface
  claimed.

## Recommendation
PROTOTYPE — stand it up against one real set of internal alert sources (e.g. existing
monitoring already in place) before relying on it for production on-call.

## Change History
### 2026-11-22
Initial discovery and review.
