# Convoy

## Summary
Convoy is a webhooks gateway: it accepts events from your backend and reliably delivers
them to subscriber-supplied webhook URLs, handling retries, fanout to multiple
subscribers, and delivery observability so you don't build that infrastructure yourself.

## Why I Should Care
Any product that offers webhooks to its own customers (a common ask for an MSSP
admin-portal SaaS integrating with customer tooling) needs exactly this: durable retry
with backoff, per-subscriber delivery status, and fanout. Building that reliably
in-house is a multi-week project that's easy to under-scope (idempotency, retry storms,
dead-letter handling).

## Problems It Can Remove
- Hand-rolling a retry-with-backoff queue for outbound webhook delivery, including
  handling subscriber endpoints that are slow, down, or rate-limiting you.
- Building delivery-status visibility (did the customer's endpoint actually receive and
  ack the event?) from scratch.
- Managing fanout to multiple subscriber endpoints per event type.

## Practical Uses
- Front the "notify customers when X happens" feature of a SaaS product with a dedicated
  webhook-delivery service instead of a custom queue-consumer.
- Give customers of the admin-portal SaaS a self-service webhook-subscription UI without
  building one from scratch (Convoy ships a dashboard).
- Use internally to fan events out to multiple internal services during a migration,
  without coupling the event producer to every consumer.

## Product Opportunities
- If embedded (not resold as a hosted service — see License), the retry/fanout/delivery-
  status pattern directly maps onto "give customers webhooks" as a sellable feature of
  the admin-portal SaaS.

## Agent / Automation Opportunities
- No agent-specific surface; relevant purely as backend infrastructure.

## Integration
Self-hosted via Docker; requires Postgres and Redis as dependencies (not a single
binary). Integration effort: **Medium** — more infrastructure to stand up and operate
than a single-container tool, but the dashboard and CLI reduce day-2 operational work
once running.

## Architecture Notes
Not deeply inspected beyond licensing and maturity checks this run; worth reading the
retry/backoff and idempotency design specifically before depending on it for
customer-facing delivery guarantees.

## Maturity
Established and active: 2,862 stars, created 2021-04-17 (4+ years old), pushed_at
2026-09-09, latest tagged release v26.7.6 (2026-08-27), 43 open issues. Long track record
relative to most projects surfaced by this catalog.

## License
**Elastic License 2.0 — flagged loudly, not an OSI-approved open-source license.**
Confirmed via the repository's own LICENSE file. Key restriction: "You may not provide
the software to third parties as a hosted or managed service, where the service provides
users with access to any substantial set of the features or functionality of the
software." This directly blocks reselling Convoy itself as a multi-tenant hosted webhook
service — relevant given the MSSP SaaS context. Self-hosting for internal use or
embedding as an internal component of a product (where customers don't get direct access
to Convoy's own feature surface) is permitted under the license, but get this read by
someone with legal context before any customer-facing deployment.

## Alternatives
webhookx-io/webhookx (Apache-2.0, 297 stars, pushed 2026-08-18 — genuinely cleaner
license than Convoy but far less mature/proven; filed as Worth Watching, worth
revisiting if it gains traction), didil/inhooks (Go + Redis, 71 stars, incoming-webhooks
only, not outbound delivery — different problem), or building retry/fanout logic
in-house on top of an existing job queue (e.g. already-catalogued Sidequest or Liteque).

## Risks / Limitations
- Elastic License 2.0 is the primary adoption constraint — see License above.
- Requires Postgres + Redis, adding operational surface area versus lighter-weight
  alternatives.

## Recommendation
PROTOTYPE — evaluate for internal/embedded use only (not as a resold hosted service per
the license), and specifically test retry/idempotency behavior under simulated
subscriber downtime before depending on it for customer-facing delivery guarantees.

## Change History
### 2026-09-10
Initial discovery and review. Slot 2 (product infrastructure, APIs, backend components) run.
