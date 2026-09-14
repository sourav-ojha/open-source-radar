# maintenant

## Summary
A single Go binary (idles under 30MB RAM) that auto-discovers Docker containers,
Kubernetes workloads, and uptime/TLS/cron endpoints, then monitors and alerts on all
of it — collapsing what would otherwise be a Prometheus + Grafana + Alertmanager +
node-exporter + cAdvisor + blackbox-exporter + Trivy stack into one container with
zero PromQL or dashboard authoring required.

## Why I Should Care
For a solo/small-team operator running a handful of VPS-hosted products plus an MSSP
admin-portal, standing up and maintaining a full Prometheus stack is disproportionate
effort for "is my stack up, and what is burning?" This answers that with a single
`docker compose up`.

## Problems It Can Remove
- The ten-odd-component observability stack (Prometheus, Grafana, Alertmanager,
  node-exporter, cAdvisor, blackbox-exporter, a cert exporter, Loki, Promtail, Trivy)
  most self-hosters wire together by hand.
- Manual `docker ps` / SSH-and-check monitoring habits on a small server fleet.
- A separate CVE/stale-image-update watcher (Trivy/Watchtower-equivalent) bolted on
  separately.

## Practical Uses
- Monitor the MSSP admin-portal's uptime, TLS expiry, and container health without a
  Prometheus deployment.
- Bare-metal or air-gapped host monitoring, where even installing a container runtime
  just for observability tooling is undesirable — maintenant runs as a static
  systemd-managed binary with no container runtime required.
- Kubernetes-native monitoring via read-only RBAC and in-cluster API auto-detection.

## Product Opportunities
The Personal tier (€149 once, one host, unlimited endpoints) is cheap enough to simply
buy for a small client-facing deployment rather than engineer an internal equivalent —
worth considering directly rather than building "yet another dashboard."

## Agent / Automation Opportunities
Exposes both a REST API and a Prometheus metrics endpoint — either is a reasonable
backend for an incident-triage MCP server that lets a coding agent check "is the prod
container healthy" before or after a deploy.

## Integration
Docker (recommended, with documented hardening: read-only filesystem, no-new-privileges,
tmpfs), Kubernetes via `kubectl apply` or Helm, or a bare-Linux static binary installer
for systemd hosts. **Low** integration effort — a docker-compose snippet with a
read-only Docker socket mount is the entire setup for the common case. Note the README's
explicit security warning: the UI/API ships with no built-in authentication, so a
reverse proxy or localhost binding is required before exposing it.

## Architecture Notes
Auto-discovery reads the Docker socket (group permissions handled automatically on
both Compose and Swarm), the in-cluster Kubernetes API, and `/proc` for bare-metal
hosts — the same binary adapts to whichever runtime it finds itself in, switching on
container monitoring the moment a runtime appears. Paid tiers (Personal/Pro) are the
*same* AGPL binary with features unlocked via a signed, offline-tolerant license-key
check against a remote server, not a separate proprietary codebase.

## Maturity
Emerging — created February 2026, already at 503 stars and a real release cadence
(v1.5.0, 2026-09-09). Effectively single-maintainer (483 of 484 commits), so no
multi-year track record yet, but the documented security posture and edition model
suggest a deliberate, non-toy project.

## License
**AGPL-3.0** for the Community edition (free, fully functional on 1 host, not a
crippled trial) — flagged per policy. Personal (€149 once) and Pro (€29/mo) editions
unlock multi-host/team features via a license-key check but ship the same source.
AGPL-3.0 requires source disclosure if the code itself is modified and offered as a
network service to others; using it as-is to monitor your own infrastructure carries
no such obligation. Pro edition is explicitly required if reselling monitoring for
other people's infrastructure (relevant to the MSSP context).

## Alternatives
- **Uptime Kuma** — broader community and longer track record, but checks-only, no
  container/CVE discovery layer.
- **Beszel** — lightweight server monitoring, lacks the uptime/TLS/cron layer.
- **Netdata** — heavier, broader metrics scope, steeper configuration.
- Full Prometheus/Grafana/Alertmanager stack — what maintenant explicitly targets
  replacing for small deployments.

## Risks / Limitations
- Single maintainer, young project — no multi-year reliability track record.
- No built-in authentication by default; must be placed behind a reverse proxy or
  bound to localhost, per the project's own security guidance.
- AGPL-3.0 source-disclosure obligations apply if modified and redistributed as a
  service.

## Recommendation
**USE NOW** — low setup cost, addresses a real recurring pain (assembling a monitoring
stack from scratch), and the free Community tier is enough to evaluate immediately on
a real VPS.

## Change History
### 2026-09-14
Initial discovery and review. Catalogued as USE NOW, 8.0/10.
