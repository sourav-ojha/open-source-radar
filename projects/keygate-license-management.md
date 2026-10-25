# Keygate

## Summary
Self-hosted software license management platform: subscriptions, perpetual/floating licenses, usage metering, Stripe billing end-to-end, auto-updater release feeds (Sparkle/Velopack/Tauri), team seats/entitlements, and an admin dashboard — one Go binary plus PostgreSQL, no Redis, no microservices.

## Why I Should Care
This is the exact scaffolding that eats a week before a side project can charge money: license activation, Stripe webhooks, usage quotas, and signed auto-update feeds. Keygate bundles all of it behind one binary and my own Stripe account instead of a $50–300/month SaaS subscription (Keygen, Cryptlex, LicenseSpring) or a half-built custom version.

## Problems It Can Remove
- Hand-rolled Stripe webhook handling plus a license table, which is what most MERN side projects end up with today.
- Building a signed auto-update feed compatible with Sparkle, Velopack, and Tauri separately for a shipped native app.
- Writing atomic, retry-safe usage-quota enforcement from scratch.

## Practical Uses
- Backend for a paid desktop app, CLI tool, or Electron app needing license-key activation, device limits, and trials.
- Subscription/entitlement management for a SaaS product already on Stripe.
- Floating/concurrent-seat licensing for an internal tool sold to teams.
- Usage-based quota enforcement (API calls, storage, bandwidth) with atomic counting.

## Product Opportunities
Drop-in licensing+billing backend for any future packaged product instead of hand-rolling Stripe webhooks, activation limits, and update feeds. Could underpin a white-labeled "sell your internal tool" offering adjacent to the MSSP/PTaaS work.

## Agent / Automation Opportunities
Server-to-server API with scope-based Bearer tokens makes it straightforward to mint/manage licenses from an automation script or agent (e.g. nightly usage exports, automated license issuance on a Stripe webhook).

## Integration
Docker Compose is the recommended path (`docker-compose.yml` + `.env` with `JWT_SECRET`/`LICENSE_SIGNING_KEY`). Go server + PostgreSQL + optional S3-compatible storage for release artifacts. Auto-migration on startup, setup wizard for first run. Low integration effort.

## Architecture Notes
Three-layer Stripe reliability (webhook + success-page verification + periodic sync) to never miss a payment. Atomic quota/activation enforcement at the database level avoids double-counting under concurrent retries. License-verify endpoints collapse all "license-knowable" failures to a single 404, closing off key-enumeration. Idempotency-Key middleware on writes. Ed25519 per-product signing keys for release feeds, encrypted at rest under AES-256-GCM + HKDF-derived subkeys.

## Maturity
Emerging. v0.1.16, created 2026-03-26, pushed 2026-10-24 (active, roughly weekly releases). 135 stars, 32 forks, 2 subscribers, 7 contributors (lead author ~43/61 commits among top contributors).

## License
AGPL-3.0. The network-use clause does not block the primary use case (self-hosting it as your own licensing backend) — it only applies if you modify Keygate itself and offer that modified version as a hosted service to third parties.

## Alternatives
Keygen, Cryptlex, LicenseSpring (all commercial SaaS). The default alternative today is a hand-rolled Stripe webhook + license table.

## Risks / Limitations
- Pre-1.0 and young (7 months, 135 stars) — pilot on a low-stakes product before trusting it with primary revenue.
- AGPL-3.0 flagged per policy, though not a practical blocker here.
- Only 2 GitHub subscribers; sustained maintenance beyond the current pace is unproven.

## Recommendation
PROTOTYPE — worth standing up against a low-stakes product (e.g. a CLI tool or internal utility sold to a small number of users) before trusting it with primary revenue.

## Change History
### 2026-10-25
Discovered via GitHub search for self-hosted license/subscription management tooling (slot 5: self-hosted SaaS alternatives, productivity). First-time review.
