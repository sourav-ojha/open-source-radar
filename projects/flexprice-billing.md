# Flexprice

## Summary
Open-source, self-hostable usage-based and credit-based billing infrastructure aimed
specifically at AI-native and SaaS teams: real-time usage metering, prepaid/promotional
credit wallets, flexible pricing plans (seat/usage/hybrid), feature entitlements, and
automated invoicing — composable on top of existing payment processors (Stripe,
Chargebee) rather than replacing them.

## Why I Should Care
Billing is expensive to retrofit once a product has real usage. Flexprice's explicit
AI-native credit-billing focus (prepaid credits, per-token/per-call metering) maps
directly onto how an LLM/agent product would actually want to charge — a gap general
subscription-billing tools don't optimize for.

## Problems It Can Remove
Hand-rolled usage-event aggregation, proration-correct invoicing, credit-wallet
accounting, and plan-versioning logic — all real engineering time sinks the README
correctly identifies as a recurring "developer problem."

## Practical Uses
- Meter and bill an AI/agent product by LLM calls, compute time, or agent runs against
  prepaid credits
- Enforce feature entitlements (per-plan or per-customer limits) without hand-rolling
  gating logic
- Layer real-time usage dashboards and invoicing on top of an existing Stripe/Chargebee
  integration
- Prototype a metered-pricing micro-SaaS without building metering/credits/invoicing from
  scratch

## Product Opportunities
Directly applicable to any future product that bills by usage or LLM-token credits rather
than a flat subscription — the credit-wallet primitive is purpose-built for that pattern.

## Agent / Automation Opportunities
None specific to agent tooling itself; it's billing infrastructure an agent-powered
product would sit behind, not a tool an agent would call directly.

## Integration
Self-hosted via Docker Compose, but the production stack is heavy: Postgres + Kafka +
ClickHouse + Temporal. Appropriate for a real product doing billing at scale; overkill for
prototyping or a small side project. REST API plus Go/Python/JS SDKs. A hosted cloud
option also exists.

## Architecture Notes
Composable rather than a full billing-platform replacement: ingest usage events (direct
API calls or streamed from data warehouses/analytics pipelines), process them in real
time for pricing/credits/entitlements/invoicing, then sync results out to existing
payment, CRM, CPQ, and accounting tools.

## Maturity
Emerging but well-adopted. Created 2024-11-05 (~1.85 years old), 5,510 stars, 494 forks,
173 open issues, Product Hunt and Trendshift traction, active release cadence (latest tag
8 days before this review).

## License
AGPL-3.0, copyright Squirrelly Technologies Private Limited. Same network-copyleft
caveat as any AGPL entry — modifying and hosting it as a service to third parties
triggers source-release obligations. A separate commercial/cloud tier appears to exist
alongside the OSS core (open-core signal worth watching for feature-gating drift).

## Alternatives
Meteroid (catalogued STUDY, 2026-09-13) — lighter stack, broader general
subscription/invoicing focus rather than AI-credit-specific. Lago (~10.5k stars,
mainstream, general usage-based billing). Stripe Billing / Chargebee (paid SaaS, no
self-hosting).

## Risks / Limitations
- Heavy self-hosting footprint (Postgres + Kafka + ClickHouse + Temporal)
- AGPL-3.0 licensing implications for commercial/hosted embedding
- Open-core signals — confirm which features stay in the AGPL core over time

## Recommendation
PROTOTYPE — worth a real trial if a usage/credit-billed product is on the roadmap;
integration effort (High) makes it a poor fit for anything smaller.

## Change History
### 2026-09-17
Initial discovery and cataloguing.
