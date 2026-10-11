# Velox

## Summary
Velox is a self-hosted, MIT-licensed billing engine built specifically for AI and usage-heavy SaaS. It owns the billing layer above the card charge — pricing, subscriptions, usage metering, invoicing, credits, and dunning — while Stripe still executes the underlying charge as a plain PaymentIntent. Pricing is natively multi-dimensional (`model × token_type`), so one meter can carry every LLM pricing dimension instead of needing a separate meter per model/role combination.

## Why I Should Care
Billing for AI-feature usage inside a commercial product is a real, recurring problem: generic usage-billing engines (Lago, Meteroid, OpenMeter, Flexprice — all already catalogued) treat tokens as just another numeric usage dimension and leave margin tracking to a data-warehouse project. Velox stamps provider cost onto every event at ingest, so "which customers lose us money?" is one API call. It also has a direct LiteLLM proxy integration, so an existing LLM gateway gets billed usage with zero SDK work.

## Problems It Can Remove
- Hand-rolled token-usage metering and pricing-rule logic for AI features
- A custom per-customer margin/COGS spreadsheet or warehouse query
- Stripe Billing's percentage fee on top of the underlying PaymentIntent
- Building dunning (failed-payment retry/escalation) from scratch

## Practical Uses
- Bill customers for LLM/agent token usage by model and token type
- Sell prepaid usage commits with automatic drawdown
- Surface per-customer margin to catch unprofitable pricing before it compounds
- Point an existing LiteLLM proxy's spend callback at Velox for instant dimensioned billing
- Use the Stripe-grade subscription primitives (proration, dunning, credit notes, hosted invoice pages) for billing that isn't AI-specific too

## Product Opportunities
- Billing/metering backbone for any AI-feature add-on sold inside an existing product
- Reference billing layer for a new micro-SaaS built around LLM usage

## Agent / Automation Opportunities
The LiteLLM spend-callback integration turns any existing coding-agent or LLM-gateway deployment into billed, margin-tracked usage automatically, with no SDK or schema work on the agent side.

## Integration
Single Go binary, Docker Compose for self-hosting (adds Postgres, Redis for distributed rate limiting, the static dashboard, and an nginx front door — five containers total, one of which is Velox). README claims ~5 minutes from clone to a finalized invoice. No Kubernetes/Helm packaging yet — deliberately deferred until a design partner names the k8s flavour they run. Integration effort: **Low**.

## Architecture Notes
- Multi-dimensional meters: one meter, N pricing rules, dimensions carried on every usage event; highest-priority rule claims each event (no double counting)
- Prepaid commits fund atomically on invoice finalization; usage draws down the balance (promotional credits first)
- Append-only audit log enforced by database triggers, not application convention
- Row-Level Security for serving multiple internal tenants from one deployment
- Transactional outbox: webhook/email side-effects commit in the same transaction as the state change that caused them
- Published, reproducible benchmarks: a leader process is SIGKILLed at five chosen kill points and a 4-leader race condition is forced; the project reports 0 duplicate/lost invoices and 0 cents of drift, with a documented negative control (drop one specific index and the same test produces 103 invoices for 40 periods) that makes the result a measurement rather than a marketing claim.

## Maturity
Experimental/emerging. Pre-1.0 (current tag v0.3.2); the README states the public API is stabilising but not frozen, and breaking changes can land on MINOR releases until 1.0.0. 19 open issues, active CI. Effectively single-maintainer: 1,640 of ~1,649 commits are from one author (sagarsuperuser), plus dependabot. Only 4 GitHub stars at review time — the engineering rigor (reproducible failure-correctness and throughput benchmarks) is unusually high for a repo this undiscovered, which is the hidden-gem signal, but there is no independent community validation yet.

## License
MIT. Nothing is gated — dunning, the audit log, and row-level-security multi-tenancy all ship unlocked, with an explicit stated commitment that nothing currently shipped will ever move behind a paid key. No AGPL/SSPL/BSL concerns. Self-hosting shifts billing-data compliance obligations (PCI, data residency) onto the operator — the project says this directly rather than implying self-hosted equals compliant.

## Alternatives
Lago, Meteroid, OpenMeter, Flexprice (all already catalogued, general-purpose usage-billing engines) — none ship AI-token-dimensioned pricing, a LiteLLM ingestion path, or per-customer margin/COGS as first-class unlocked features. Stripe Billing (proprietary, percentage fee on top of the card charge).

## Risks / Limitations
- Pre-1.0 API, not yet frozen
- Single-maintainer bus-factor risk despite the engineering quality
- No independent community adoption signal (4 stars)
- No Kubernetes/Helm deployment path yet
- Self-hosting transfers compliance obligations to the operator

## Recommendation
PROTOTYPE — the engineering rigor (reproducible failure-correctness benchmarks, explicit negative controls, transactional outbox, RLS multi-tenancy) is well above what a 4-star, largely-solo repo usually shows, and the AI-token-native billing shape is a real gap in the already-catalogued alternatives. Worth testing against a real AI-feature billing need before trusting it with production revenue, given the pre-1.0 status and single-maintainer risk.

## Change History
### 2026-10-11
First reviewed and catalogued. Discovered via GitHub topic search (`topic:invoicing`), confirmed via GitHub API (MIT, 4 stars, pushed 2026-10-11) and direct README/docs inspection, including the published benchmark methodology.
