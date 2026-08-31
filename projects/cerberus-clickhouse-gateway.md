# Cerberus (ClickHouse observability gateway)

## Summary
Cerberus is a read-only HTTP gateway that speaks the Prometheus, Loki, and Tempo wire
protocols but translates PromQL/LogQL/TraceQL queries into ClickHouse SQL. Grafana is
pointed at cerberus as three datasources instead of the real Prometheus/Loki/Tempo
backends, so existing dashboards and alert rules keep working unchanged while the
underlying storage/query engine becomes ClickHouse.

## Why I Should Care
If a self-hosted observability stack is ever stood up for personal infra or the MSSP
admin portal's internal monitoring, this collapses "run Prometheus + Loki + Tempo + Mimir"
into "run ClickHouse + cerberus" — fewer moving parts, cheaper long-retention storage, and
zero migration cost for dashboards/alerts already built in Grafana.

## Problems It Can Remove
Removes the need to run and operate three or four separate specialized time-series/log/
trace databases just to keep Grafana-based tooling working.

## Practical Uses
- Swap a Loki+Tempo+Mimir stack for ClickHouse without rewriting Grafana dashboards or
  alert rules.
- Get cheaper, longer-retention storage for metrics/logs/traces than the native stack.
- Study the protocol-translation-gateway pattern itself — it generalizes beyond
  observability to any case where a cheaper backend needs to impersonate an existing
  wire protocol.

## Product Opportunities
None directly — this is infrastructure tooling, not a customer-facing capability.

## Agent / Automation Opportunities
None identified.

## Integration
Single Go binary or Docker container; Helm chart listed on Artifact Hub. Add as three
Grafana datasources. Integration effort: **Low** to add on top of an existing setup, but
requires already running an OpenTelemetry Collector that writes into ClickHouse — this is
not a turnkey full observability stack by itself.

## Architecture Notes
Purely a read-side query translator — "cerberus never ingests or stores anything." Writers
(the OTel Collector) keep writing to ClickHouse exactly as before; cerberus only
translates incoming PromQL/LogQL/TraceQL queries into ClickHouse SQL and reshapes the
result back into the expected Prometheus/Loki/Tempo response format. The maintainer
publishes live compatibility-percentage badges (per protocol) rather than just claiming
compatibility.

## Maturity
Experimental. Created 2026-05-05 (~4 months old), rapid iteration (v1.19.0 as of
2026-08-30), 64 stars, 53 open issues (largely the maintainer's own compatibility-tracking
issues based on issue-title sampling). The README itself states: "1.0 — stable wire API,
young project... try it against your own data before you rely on it in production."

## License
Apache-2.0 — no commercial-use restrictions.

## Alternatives
Native Loki+Tempo+Mimir stack (status quo), Grafana's own ClickHouse data-source plugin
(per-datasource query building, not a full protocol-translation gateway), SigNoz,
Monoscope (also catalogued today — a different approach: S3-backed rather than
ClickHouse-backed, and a full platform rather than a Grafana-compatible gateway).

## Risks / Limitations
- Young (4 months old); maintainer explicitly labels it pre-production.
- Primarily one human maintainer (tsouza) with AI-assisted commits (GitHub Copilot, Devin)
  visible in the contributor list — bus-factor risk not yet proven otherwise.
- Not a full observability stack — requires an existing OTel Collector → ClickHouse
  ingestion pipeline already in place.

## Recommendation
**STUDY** — the protocol-translation-gateway architecture is worth understanding and
piloting against a non-critical dataset, but the maintainer's own maturity caveat means it
is not yet a production swap-in.

## Change History
### 2026-08-31
Initial discovery and review. GitHub API confirmed Apache-2.0, 64 stars, created
2026-05-05, pushed_at 2026-08-31, not archived. README verified via
raw.githubusercontent.com, including the maintainer's own maturity disclaimer.
