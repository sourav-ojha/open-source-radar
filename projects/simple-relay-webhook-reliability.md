# Simple Relay

## Summary
A single Go binary that sits between external services and internal applications as a reliability layer for HTTP and webhook traffic. Inbound requests are committed to embedded SQLite (WAL mode, single-writer with micro-batching) before the caller gets a response, then delivered with exponential-backoff-plus-jitter retries. Ships HMAC verification presets for Stripe and GitHub plus generic HMAC-SHA256, a synchronous proxy mode, an asynchronous job mode, and a dead-letter queue with replay via an embedded web UI. No external database or message broker required.

## Why I Should Care
Webhook reliability — durable receipt, per-provider signature verification, retries, a replayable dead-letter queue — is boilerplate that gets rebuilt from scratch on nearly every SaaS with billing events or partner integrations. This is a dependency-free binary that does the whole thing correctly out of the box, directly applicable to the MSSP admin-portal SaaS.

## Problems It Can Remove
A hand-rolled BullMQ-based (or similar) retry queue plus custom HMAC verification plus a custom DLQ-with-replay admin view — normally a few days of work, duplicated per project.

## Practical Uses
- Ingest Stripe/GitHub webhooks with signature verification and guaranteed delivery
- Durable outbound webhook delivery to a product's own customers/partners, with DLQ and replay instead of a bespoke queue
- Buffer a flaky downstream integration behind the synchronous proxy mode without changing calling code
- Return `202 Accepted` immediately and run slow work in the background via the async job mode

## Product Opportunities
Directly replaces the webhook-ingestion-plus-retry-queue component otherwise custom-built for any SaaS with billing events, partner integrations, or outbound notification delivery — a concrete piece of infrastructure to drop behind a commercial product.

## Agent / Automation Opportunities
None specific — this is general-purpose HTTP/webhook reliability infrastructure, not an agent-facing tool.

## Integration
Deployment: Docker (`ghcr.io/simple-relay/simple-relay:latest`, port 8080, volume at `/data`, `RELAY_DB_PATH` env var) or a standalone `CGO_ENABLED=0` Go binary.
Interfaces: HTTP webhook ingestion, synchronous proxy, asynchronous job API, embedded web UI at `/ui/`, `/healthz`.
Integration effort: **Low** — one container, no external database or broker.

## Architecture Notes
Raw payloads are committed to SQLite before returning `200 OK`/`202 Accepted`. Async jobs are at-least-once (duplicates possible by design). Retries use exponential backoff with full jitter to avoid synchronized retry spikes; Litestream can stream SQLite backups to S3. The web UI is embedded in the binary via `go:embed`, so there's no separate frontend build step. Response bodies in the UI are truncated to 32 KB.

## Maturity
Emerging — repo created 2026-10-07, 3 weeks old at review, 42 stars, 4 contributors, 10 open issues (real usage/engagement, not just stars). Some README claims (e.g. "sub-microsecond" hot reload of routing rules) are not backed by published benchmarks.

## License
AGPL-3.0, flagged loudly per policy. Running it unmodified as internal infrastructure behind a product does not itself require disclosing the product's own source. Modifying Simple Relay and offering the modified version as a network-accessible service would trigger AGPL's source-disclosure requirement — verify with counsel before any plan involving forking or white-labeling the binary itself.

## Alternatives
- Svix (MIT, self-hostable webhooks-as-a-service) — more full-featured (fan-out, customer-facing portal UI) but needs Postgres plus optional Redis; heavier to run than a single dependency-free binary
- Hookdeck — hosted-first, not self-host-primary the way Simple Relay is
- Hand-rolled BullMQ/Node retry queue — the status quo this replaces for a Node stack, at the cost of building and maintaining DLQ/replay/HMAC logic yourself

## Risks / Limitations
- AGPL-3.0 licensing requires care, see License section above
- 3 weeks old, 42 stars — early-stage and unaudited
- Some performance claims unbenchmarked in the repo
- No documented CLI or full API path reference beyond the main endpoints

## Recommendation
**PROTOTYPE** (score 7.5/10) — low-cost to trial for any upcoming feature that needs reliable inbound or outbound webhook delivery.

## Change History
### 2026-10-22
First discovered via GitHub search. Verified via GitHub API (AGPL-3.0, 42 stars, 0 subscribers, 0 forks, 10 open issues, created 2026-10-07, 4 contributors) and confirmed the LICENSE file directly as genuine AGPL-3.0 text.
