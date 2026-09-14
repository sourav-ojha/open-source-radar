# Bugsink

## Summary
Bugsink is a self-hosted error-tracking server that speaks the same protocol Sentry
SDKs already use. Point an existing Sentry DSN at a Bugsink instance and events start
flowing in — no SDK changes, no vendored client library, no code migration. It's a
single Python/Django application (not a microservice sprawl), with ~2,000 GitHub stars
and a two-year history of regular releases.

## Why I Should Care
Every one of Sourav's stacks (his own products, the MSSP admin portal) likely already
has a Sentry SDK wired in somewhere, or would benefit from one. Bugsink removes the
main reason people stay on paid Sentry despite wanting to self-host: DSN compatibility
means zero application-code cost to switch.

## Problems It Can Remove
- A recurring Sentry SaaS bill for internal or client-facing projects.
- Sending client MSSP-portal stack traces (which may contain sensitive data) to a
  third party.
- The operational overhead of self-hosting Sentry itself, which is a much heavier
  multi-service deployment.

## Practical Uses
- Drop-in error tracking for the MSSP admin-portal SaaS.
- A throwaway Docker instance to evaluate before committing to persistent storage.
- Small-team single-Postgres deployment, scaling to a dedicated worker/queue setup
  if event volume grows.

## Product Opportunities
Bundling Bugsink as the error-tracking backbone of a self-hosted product stack sold to
clients is fine under its license — as long as the offering isn't itself a competing
error-tracking product (see License below).

## Agent / Automation Opportunities
Its REST API for querying issues/events is a natural fit for an MCP tool that hands a
coding agent the exact stack trace behind a failing deploy, rather than requiring a
human to copy-paste it in.

## Integration
Docker (`docker pull bugsink/bugsink`) is the fastest path — a single container with
`SECRET_KEY` and `CREATE_SUPERUSER` env vars gets a working instance. Also installable
via pip. Integration effort is **low**: existing Sentry SDKs need only a DSN change.

## Architecture Notes
Single Django app ingesting Sentry's "envelope" wire protocol. The `sentry/` directory
vendors BSD-3-Clause code from Sentry itself for protocol compatibility. Scaling
guidance (from docs) covers moving from SQLite/single-process to Postgres/worker setups
as volume grows.

## Maturity
Mature for its niche — 2 years old, 2,069 stars, `pushed_at` current, real release
cadence (v2.5.1, 2026-08-31). Effectively single-maintainer by commit volume
(2,259 of the top contributor's commits vs. 26 from dependabot, the next-highest),
which is a bus-factor risk to weigh against the code's stability.

## License
**PolyForm Shield 1.0.0** for the core (confirmed by reading the raw `LICENSE` file —
GitHub's API reports `NOASSERTION` because Shield isn't a GitHub-recognized SPDX key
by default). This is a source-available license, **not** OSI-approved open source. It
permits any use — including internal commercial self-hosting — *except* offering a
product that competes with Bugsink or with Bugsink B.V.'s own offerings. Self-hosting
for internal error tracking is squarely a permitted purpose; reselling a hosted
Bugsink-based error-tracking service would not be. Notably, Bugsink's own docs and
README never claim to be "open source" — it presents itself honestly as
self-hostable/source-available, unlike some prior catalog rejections (billabear,
Notifuse) that marketed non-OSI licenses as "open source."

## Alternatives
- **GlitchTip** — AGPL-3.0, also Sentry-SDK-compatible, more community-governed
  (multiple regular contributors vs. Bugsink's single-maintainer pattern).
- **Sentry self-hosted** — BSL-licensed, much heavier multi-service deployment.
- Several single-digit-star newcomers (thermite-rs, sentori-selfhosted) with no track
  record yet.

## Risks / Limitations
- Bus-factor risk from single-maintainer commit concentration.
- PolyForm Shield's noncompete clause must be understood before building any
  resale/hosting business model around it.
- License shows as `NOASSERTION` in tooling that reads GitHub's license API field —
  always verify against the actual `LICENSE` file for this repo specifically.

## Recommendation
**USE NOW** — low integration effort, honest licensing, mature codebase, and directly
solves a real recurring cost (Sentry SaaS bill) with zero code changes required for
projects already using Sentry SDKs.

## Change History
### 2026-09-14
Initial discovery and review. Catalogued as USE NOW, 8.1/10.
