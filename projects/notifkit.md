# notifkit

## Summary
Self-hosted notification infrastructure for Node/TypeScript products. One typed API call routes to email, SMS, push, and webhooks, with preferences, quiet hours, retries, fallback chains, scheduling, multi-step workflows, and delivery logs — backed by PostgreSQL and Redis.

## Why I Should Care
Notification plumbing (retries, quiet hours, fallback channels, delivery logs, unsubscribe handling) is exactly the kind of undifferentiated infrastructure worth not rebuilding per project, and it's a direct match for the existing Node/Postgres/Redis-adjacent stack.

## Problems It Can Remove
- A hand-rolled email/SMS/push dispatch layer scattered across an Express/NestJS app.
- Building timezone-aware quiet hours, fallback-on-failure logic, and a dead-letter queue from scratch.
- One-off provider integrations (Resend/SES/Postmark, Twilio/MessageBird, FCM) reimplemented per project.

## Practical Uses
- Centralize product notifications (order updates, alerts, OTPs) behind a single `notify()` call.
- Give an MSSP admin-portal one call for alert delivery across channels instead of separate provider integrations.
- Track delivery logs and consent/unsubscribe state in one place instead of per-provider dashboards.

## Product Opportunities
Directly the kind of building block that shortens the path from idea to shippable product — notification infra is a recurring need across side projects and client work alike.

## Agent / Automation Opportunities
Its architecture diagram lists an MCP server alongside the REST API and TypeScript SDK, so an agent could trigger notifications directly as a tool call.

## Integration
npm package (`notifkit`), runs as a single Node process (monolith topology) for small apps or as distributed API + worker services for scale. Requires standing up PostgreSQL and Redis. Medium integration effort — not just an `npm install`, needs infra and provider credentials.

## Architecture Notes
Pipeline: API ingests to Postgres (state) and Redis (priority queues/streams/scheduling) → background workers (enricher → engine/quiet-hours → scheduler → delivery) → provider transports. Claims chaos-testing against worker kills (SIGKILL), connection loss, and message bursts.

## Maturity
Emerging. npm v0.1.3, ~364 weekly downloads, single-repo org (devkitshq has no other public repos), created 2026-07-16 (~6 weeks old). No GitHub Releases published — version tracked only via npm tags.

## License
MIT. No restrictions.

## Alternatives
Novu (larger, more established open-source notification infra), Courier/Knock (SaaS), hand-rolled provider-specific integrations (status quo).

## Risks / Limitations
- Very early stage — validate the README's "battle-tested in production" claim independently before depending on it; no public evidence beyond the claim itself.
- Reintroduces a Redis dependency that other recent finds (e.g. Sidequest) are specifically removing.
- Single-repo, single-org project with modest adoption signals (364 weekly npm downloads) — bus-factor risk.

## Recommendation
PROTOTYPE — worth a small pilot given the direct stack fit, but too early (v0.1.x, unverified production claims) for anything load-bearing yet.

## Change History
### 2026-08-30
Discovered and reviewed. GitHub API verified: MIT, 103 stars, pushed_at 2026-08-29, created 2026-07-16. npm registry confirms v0.1.3.
