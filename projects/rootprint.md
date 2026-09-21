# Rootprint

## Summary
A self-hosted log and trace explorer that stores its indexes directly on S3-compatible
object storage (S3, MinIO, R2, GCS, Azure Blob, or local disk) instead of requiring a
dedicated database cluster. Ingests via OTLP from the OpenTelemetry Collector, Vector, or
Fluent Bit; ships a log explorer, trace waterfalls, and a per-service health dashboard
(request rate, error rate, p95 latency).

## Why I Should Care
Object-storage-backed indexing is the same cost argument OpenObserve makes, but Rootprint
is narrower in scope (logs + traces, not the full logs/metrics/traces/RUM/session-replay
surface), which means less to operate and reason about for a single project's telemetry —
and it plugs directly into S3, which is already part of the AWS stack in use.

## Problems It Can Remove
Running a Postgres/Elasticsearch cluster sized for log volume just to hold telemetry; a
Datadog/Honeycomb trial that turns into a real bill; switching tools to go from a trace to
the logs from the same request.

## Practical Uses
- Personal/small-team logs+traces backend that costs close to raw S3 storage
- OpenTelemetry sink for a Node/Next.js service already emitting OTLP
- Correlate a trace waterfall with the logs from the same request in one UI

## Product Opportunities
None specific — this is an operational/observability tool, not a product-embeddable
component.

## Agent / Automation Opportunities
None built in beyond standard OTLP ingestion; no MCP server or agent-specific surface.

## Integration
Requires Docker, a Postgres instance for metadata, an S3-compatible bucket, and an OTel
Collector/Vector/Fluent Bit in front of it to forward telemetry. Medium effort — more
moving parts than a single binary, despite the storage cost win.

## Architecture Notes
Built on Bun/Hono/SvelteKit with Postgres for metadata and object storage for the actual
log/trace indexes — the interesting part is the index format that makes S3-backed queries
fast enough for interactive use rather than batch-only.

## Maturity
Emerging. Created 2026-03-06 (~6 months old), 401 stars, 14 forks, latest release v0.4.3
(2026-09-10), active near-daily pushes. One dominant contributor (660 of ~680 commits).

## License
Apache-2.0. No restrictions on commercial use, modification, or redistribution.

## Alternatives
OpenObserve (catalogued-class, more mature, broader feature set, own storage engine),
Grafana Tempo + Loki (mainstream, more moving parts), SigNoz (mainstream), Honeycomb/
Datadog (hosted SaaS).

## Risks / Limitations
- Young project with one dominant contributor — verify continuity before depending on it
- No independent adoption signal yet beyond its own star count
- Verify query performance at realistic log volume before relying on it for anything
  production-critical

## Recommendation
PROTOTYPE — worth standing up against a side project's OTel traffic to compare against
OpenObserve on real query latency and storage cost, but not yet a default pick over the
more mature alternative.

## Change History
### 2026-09-21
Initial discovery and cataloguing. Slot 6 (infrastructure, observability, deployment) run.
