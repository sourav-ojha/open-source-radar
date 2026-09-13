# Meteroid

## Summary
Meteroid is an open-source pricing-and-billing platform for SaaS/infra/AI companies:
Rust-based real-time usage metering, flexible pricing models (flat, usage-based, tiered,
hybrid), versioned plans, subscription lifecycle management, and invoicing — deployable
self-hosted or used as Meteroid Cloud.

## Why I Should Care
Billing/metering is exactly the kind of infrastructure that is expensive to build
correctly (proration, plan versioning, usage aggregation at scale) and is a recurring
blocker between "idea" and "shippable paid product" for any micro-SaaS. Meteroid is a
serious, actively developed entrant in that space with a specific edge — high-throughput
Rust-based usage ingestion without pre-aggregation — that matters if a product idea is
usage-billed (API calls, tokens, storage) rather than flat-seat-billed.

## Problems It Can Remove
Removes the need to hand-build subscription/invoicing/usage-metering logic for a new
paid product, and specifically removes the "usage events need pre-aggregation before
billing" bottleneck that simpler billing tools often hit at volume.

## Practical Uses
- Back a usage-billed micro-SaaS (e.g., an API product billed per call/token) without
  writing metering and proration logic by hand.
- Model hybrid pricing (flat + usage-based tiers) for a product idea before committing to
  a specific SaaS billing vendor.
- Study the real-time metering architecture even if Lago (already known, more mature) is
  the one actually adopted.

## Product Opportunities
Usage-based billing infrastructure is itself a candidate building block for any new
micro-SaaS idea — this is a direct hit on the "shorten path from idea to shippable paid
product" axis, provided the AGPL/self-host tradeoffs are acceptable.

## Agent / Automation Opportunities
No MCP server or agent-specific tooling found in the README — integration is via its
own REST/gRPC-style API. Treat any agent-tool claim beyond that as unclear based on
available documentation.

## Integration
Self-hostable (Docker); also offered as Meteroid Cloud for zero-setup evaluation.
**Integration effort: Medium** — a billing engine is inherently something to integrate
carefully (webhooks, plan migrations, payment-provider wiring) rather than drop in
casually, regardless of how turnkey the deploy step is.

## Architecture Notes
Rust core specifically for high-throughput metering ingestion without requiring
pre-aggregated usage data — the architectural bet that differentiates it from
simpler usage-billing tools that need usage pre-summed before ingestion.

## Maturity
The project itself displays a self-declared "status: experimental" badge despite being
created 2023-07-13 (~3 years old) with 1,231 stars and a v1.0.0-rc7 tag (2026-08-24) —
take that self-assessment at face value rather than reading years-old-and-active as
"mature." Contributor base has real depth (gaspb: 268, azhur: 222 commits, several more).

## License
AGPL-3.0. **Flagged per policy, loudly**: billing infrastructure is exactly the kind of
component likely to be embedded in and modified for a commercial product — AGPL's
network-use clause means a modified, hosted version must offer its source. Confirm
license terms carefully before wiring this into anything customer-facing and revenue-
generating.

## Alternatives
Lago (AGPL-3.0, already known/mainstream, more established at 10k+ stars — the safer
default "adopt now" choice), Kill Bill, Stripe Billing (proprietary/hosted). Meteroid's
differentiation is the real-time, no-pre-aggregation metering engine and PLG-oriented
pricing modeling; it is not yet clearly ahead of Lago in maturity or community size.

## Risks / Limitations
- Self-declared experimental status despite 3 years of history — treat pre-1.0 claims
  about production-readiness skeptically.
- AGPL-3.0 (see License).
- Less mature/smaller community than Lago, the closest direct alternative already known
  in this space.

## Recommendation
**STUDY** — the real-time metering architecture is worth understanding in depth before
designing a usage-billed product, but Lago remains the safer immediate adopt if a billing
engine is needed today; Meteroid is a "watch the roadmap" rather than "swap in now" call.

## Change History
### 2026-09-13
Initial discovery and review. GitHub API confirmed AGPL-3.0, 1,231 stars, created
2023-07-13, pushed 2026-09-09, not archived, latest release v1.0.0-rc7 (2026-08-24).
README verified via raw.githubusercontent.com, including the self-declared "experimental"
status badge.
