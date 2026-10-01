# Vercel Workflow SDK

## Summary
A TypeScript/JavaScript library that makes server-side functions durable: it persists workflow progress, retries failed steps, and lets workflows suspend without consuming compute while waiting. Ships a Next.js integration (`withWorkflow`), a CLI observability UI (`npx workflow web`), and a pluggable storage backend ("World") so it runs with a bundled local backend in dev, a Postgres-backed World for self-hosting, or Vercel's managed backend in production.

## Why I Should Care
Durable execution — retry-safe multi-step server logic that survives deploys and crashes — is a recurring need in any Node/Next.js product, and this is a TypeScript-native, Vercel-backed implementation of it rather than a heavyweight multi-language platform like Temporal.

## Problems It Can Remove
Replaces hand-rolled retry/job-state tracking for multi-step server processes: no more manually persisting "which step did this onboarding flow get to" or building a bespoke saga/compensation system for a chain of API calls.

## Practical Uses
- Multi-step onboarding or signup flows that must survive a deploy or crashed process mid-way
- AI agent pipelines calling multiple models/tools in sequence that need replay-safe retries
- Long-running background jobs (report generation, data sync) without hand-rolled job-state tracking
- Sagas spanning multiple third-party API calls where partial failure needs a defined recovery path

## Product Opportunities
Backbone for any product feature currently relying on a fragile chain of webhook handlers and manual retry logic — a common weak point in early-stage SaaS products.

## Agent / Automation Opportunities
Directly relevant to AI-agent pipelines: suspending a workflow mid-execution without holding compute is useful for agent steps waiting on slow tool calls or human-in-the-loop approval.

## Integration
`npm install workflow`, add `withWorkflow({})` to `next.config.ts`, call `start()` from a server action or API route. Local dev needs no configuration. Self-hosting uses the Postgres-backed World; Vercel's managed backend is an optional convenience. Integration effort: **Low** for local dev, **Medium** for self-hosting outside Vercel.

## Architecture Notes
The "World" abstraction is the key design choice — it decouples the workflow engine from any specific storage/queue backend, with a maintainer-curated list of third-party Worlds (self-hosted and managed) documented separately from the core SDK.

## Maturity
Emerging. Created October 2025 (~1 year old), 2,442 stars, 373 forks. 424 open issues against that star count is a high ratio — worth skimming issue themes for recurring pain points before depending on it in production.

## License
Apache-2.0, verified via GitHub API. No restrictions on commercial use or self-hosting.

## Alternatives
- **Temporal** (mainstream, multi-language, heavier operational footprint) — Workflow SDK trades Temporal's generality for a zero-infra, TypeScript-native developer experience with first-class Next.js integration.
- **Inngest** (close competitor, hosted-first, broader existing ecosystem) — Workflow SDK is lighter-weight and ships framework-level integration out of the box.
- **durable-workflow/workflow** — same durable-execution category but Laravel/PHP-only; reviewed and rejected this run as not applicable to a Node/Next.js stack.

## Risks / Limitations
- Young for a primitive you'd trust with business-critical workflow state.
- High open-issue-to-star ratio worth investigating before depending on it.
- Smoothest experience is tied to Vercel's platform; self-hosting is documented but the less-traveled path.

## Recommendation
**PROTOTYPE** — worth testing for an AI-agent pipeline or onboarding flow in a Next.js project, with the Postgres World if self-hosting is required.

## Change History
### 2026-10-01
Initial discovery and review. Found via GitHub Search API workflow-engine query, slot 2 (product infrastructure) run.
