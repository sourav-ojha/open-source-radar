# Outpost (Hookdeck)

## Summary
Self-hostable outbound-webhooks and event-destinations infrastructure, written in Go.
Takes events published via an API or a source queue and fans them out to per-tenant
destinations — HTTP webhooks, Amazon EventBridge/SQS/Kinesis/S3, GCP Pub/Sub, Azure
Service Bus, RabbitMQ, Kafka, Cloudflare Queues, or Hookdeck's own Event Gateway — with
retries, delivery observability, and a customer-facing portal, out of the box.

## Why I Should Care
Every product that notifies other systems ends up building a bespoke webhook sender:
retry/backoff logic, per-tenant endpoint management, delivery logs, and eventually a
status page for customers asking "did my webhook fire?" Outpost is that subsystem,
already built, tested, and maintained by a company (Hookdeck) that also sells a managed
version of the same thing — so the self-hosted path is a first-class product, not an
afterthought.

## Problems It Can Remove
Removes building: a retry/backoff engine, per-tenant destination management, a delivery
status API/portal for end users, topic-based pub/sub fanout, and webhook signing/
idempotency header conventions. Also removes the common mistake of hand-rolling "exactly
once" delivery semantics — Outpost is explicit about at-least-once guarantees instead.

## Practical Uses
- Add outbound webhooks to a SaaS product without building retry/delivery-status
  infrastructure from scratch.
- Offer customers destinations beyond plain HTTP (SQS, EventBridge, Kafka) without
  writing an adapter per destination type.
- Replace a homegrown webhook sender that has become a maintenance burden.
- Give customers self-serve visibility into webhook delivery health via the built-in
  portal, instead of fielding "is your webhook down?" support tickets.
- Use the shipped MCP server to let a coding agent inspect delivery status or configure
  destinations conversationally.

## Product Opportunities
Could underpin the notification/event layer of a micro-SaaS product from day one —
multi-tenant isolation and a customer portal are built in, which is normally the part
teams skip until a customer asks for it.

## Agent / Automation Opportunities
Ships an official MCP server alongside Go/Python/TypeScript SDKs — a coding agent can
query delivery status or manage destinations without a custom wrapper. OpenTelemetry
traces/metrics/logs are standard, so it drops into an existing observability stack.

## Integration
Self-hosted via binary or Docker (one-click Railway template provided), or consumed as
Hookdeck's managed service. Requires Redis (or Redis Cluster), PostgreSQL, and a
supported message queue as runtime dependencies — not a single static binary, so budget
for those three pieces of infra. SDKs cover Go, Python, and TypeScript. Integration
effort: Medium — the webhook-sending logic itself is zero-effort, but the backing
services add deployment surface versus a pure library.

## Architecture Notes
Multi-tenant by design: each tenant gets isolated topics/destinations and portal access.
Publish via API or by pointing Outpost at an existing queue, decoupling "how events enter
the system" from "how they fan out." Topic-based pub/sub (a message to a topic fans out
to every subscribed destination) is the core primitive, with OpenTelemetry wired through
end to end.

## Maturity
Mature for its niche: created August 2024, 1,086 stars, actively released (v1.10.0 two
days before this review), backed by a company with a commercial incentive to keep it
working. Contributor base of ~10 regular contributors beyond the two lead maintainers —
not a single-person project.

## License
Apache-2.0. No restrictions on commercial use, self-hosting, or embedding in a product.

## Alternatives
- **Convoy** (open-source, Go) — closest direct open-source competitor, similar scope.
- **Svix** — has an open-source server component, but the hosted product is the primary
  offering.
- Hookdeck's own managed **Event Gateway** — a different, non-self-hosted product from
  the same company, positioned upstream of Outpost rather than as a substitute.
- Homegrown webhook senders built on a queue + HTTP client — what this replaces.

## Risks / Limitations
- Needs Redis, Postgres, and a message queue running alongside it — not a zero-dependency
  single binary.
- 64 open issues against 1,086 stars suggests active but not fully caught-up maintenance;
  check issue age/triage cadence before depending on a specific destination type.
- Vendor-adjacent: the project exists to fund Hookdeck's managed offering, so verify
  self-hosted feature parity holds over time (no sign of deliberate feature-gating as of
  this review).

## Recommendation
PROTOTYPE — strong architecture and real maintenance backing make this worth testing in
a side project's webhook layer before committing it to a production multi-tenant
product; the Redis+Postgres+queue dependency footprint is the main reason it isn't
USE NOW outright.

## Change History
### 2026-11-19
Discovered via GitHub topic search (`topic:webhooks`) during slot 2 (product
infrastructure, APIs, backend components) discovery. Catalogued as PROTOTYPE, 8.6/10.
