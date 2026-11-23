# OpenMeter

## Summary
API-first, composable usage-metering and billing infrastructure. Ingests CloudEvents-
format usage events, aggregates them in real time into meters, drives access-limit
enforcement from those meters, and carries the result through pricing, subscriptions,
prepaid credits and invoicing — designed to be adopted end-to-end or only for the pieces
(e.g. just metering, or just access enforcement) a stack is missing, working alongside
existing payment/tax providers.

## Why I Should Care
Directly matches the micro-SaaS "billing" building block called out in this radar's own
operator profile — a credible metering/enforcement layer to drop into a usage-priced
product instead of building event aggregation and limit enforcement from scratch.

## Problems It Can Remove
Replaces a hand-rolled event-counter system (Postgres/Redis counters) that has to be kept
in sync with whatever drives both usage-limit enforcement and the eventual invoice — two
systems that silently disagree is the normal failure mode here.

## Practical Uses
- Usage-based or hybrid pricing for a feature (API calls, AI tokens, storage) without
  building an event-aggregation pipeline.
- Real-time usage-limit enforcement (hard caps, soft warnings) fed by the same meters used
  for billing.
- Prepaid credits and grandfathered pricing for an existing micro-SaaS billing model.
- Metering-only adoption: feed it usage events, query aggregated consumption via API, and
  leave invoicing to Stripe or another existing payment provider.

## Product Opportunities
Core billing infrastructure for any usage-priced micro-SaaS product — the explicit target
of the "billing" building block in this radar's mission.

## Agent / Automation Opportunities
None identified; this is billing infrastructure, not agent-facing.

## Integration
Self-hosted via Docker/docker-compose or Kubernetes; a hosted OpenMeter Cloud also exists.
Medium effort — it's a real platform (event ingestion, aggregation, API) rather than a
single binary.

## Architecture Notes
The explicit "what it is not" table in its own README (not a bundled UI, payment
processor, tax engine, or general accounting system) is a useful honesty signal — it's
scoped as infrastructure to compose with existing payment/tax tooling, not a full billing
product.

## Maturity
Emerging — releases are explicitly beta (v1.0.0-beta.238) with possible breaking changes,
though the underlying metering engine is reported to have run in production for years
before this OSS core existed. 246 forks, 92 open issues.

## License
Apache-2.0. No restrictions on commercial or internal use. Self-hosted OSS core is the full
engine, not a crippled version of the hosted OpenMeter Cloud offering.

## Alternatives
- **Lago** (10.9k stars) — more complete end-to-end billing/invoicing platform, better
  known, likely a better fit for a billing-first (rather than metering-first) use case.
- **Meteroid** (catalogued, STUDY) — subscription/invoicing-first; OpenMeter is
  metering/enforcement-first and more composable with an existing payment stack.
- Hand-rolled event counters — the status quo this replaces.

## Risks / Limitations
- Beta releases with possible breaking changes — read migration guides before upgrading.
- Not a bundled operator UI, payment processor, or tax engine — still need those around it.
- Crowded usage-based-billing niche (Lago, Lotus, Meteroid); evaluate OpenMeter specifically
  for its metering/enforcement-first angle, not as a general billing platform pick.

## Recommendation
PROTOTYPE/STUDY — worth prototyping against one usage-priced feature before committing,
given the beta status and the crowded niche.

## Change History
### 2026-11-23
Initial discovery and review. Catalogued PROTOTYPE, 7.4/10.
