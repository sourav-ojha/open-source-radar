# dánwò

## Summary
An end-to-end testing framework for web, Android, and iOS apps that drives the UI like a user (via locators, or plain-English steps that a model resolves once and then replays from a cached recording with no further model calls), and optionally adds a backend layer: emulated third-party providers (Stripe, Clerk, Resend, Google, Slack, and 20 more) behind a proxy, invariants that check the database/provider state after a test, and built-in fault injection (`danwo matrix` reruns a test once per fault — 500s, declined payments, duplicate webhooks, lost DB commits — and the invariant judges each run).

## Why I Should Care
Most E2E setups stop at "did the UI show the right text." dánwò's fault matrix answers a sharper question that actually matters for anything handling money or side effects: "if Stripe returns a 500 mid-checkout, or a webhook arrives twice, does my idempotency logic actually hold?" Removing the idempotency key from a demo app and watching two fault-matrix trials turn red (a duplicate welcome email going out) is exactly the bug class that's expensive to find in production and cheap to catch here.

## Problems It Can Remove
- Manually simulating provider failures (declined payment, Stripe 500, duplicate webhook) by hand or not testing them at all.
- The "works in the happy path demo, breaks on a real duplicate webhook" gap that idempotency bugs live in.
- Needing real sandbox API keys for Stripe/Clerk/Resend/Slack/Google just to run a CI suite.
- Re-paying a model on every CI run for plain-English test steps (the recording-replay cache means only the first run and spec-changed runs call a model).

## Practical Uses
- Point `danwo matrix` at an existing checkout/signup flow test to get a pass/fail report across 16+ injected faults with zero setup beyond the invariant.
- Migrate an existing Playwright suite over incrementally (documented path) while keeping existing tests.
- Write a plain-English step once (`agent.act("add two blue mugs to the basket and go to checkout")`), commit the recording, and let CI replay it with `--replay-only` so no API key is needed in the pipeline.
- Test a Next.js + Postgres + Stripe flow entirely offline using the built-in emulators instead of sandbox credentials.
- Cross-platform coverage: the same invariant-driven pattern extends to React Native/Expo and native Android/iOS apps on an emulator or simulator.

## Product Opportunities
Directly relevant to anything shipping a paid product with billing, email, or webhooks (his MSSP admin-portal SaaS and PTaaS partnership both qualify) — the fault matrix is a cheap way to pressure-test idempotency and webhook handling before a real customer hits the edge case.

## Agent / Automation Opportunities
Ships a skill and an MCP server for Claude Code, Cursor, and Codex; every run (with or without the backend layer) produces a "flight record" (video, steps, calls) surfaced in a dashboard, a PR comment, and over MCP — built to be read by an agent, not just a human.

## Integration
`npx @danwo/cli init` scaffolds config and a first passing test. Needs Node 22.22.3+ or 24.11.1+; mobile testing needs macOS or Linux plus the relevant emulator/simulator. The backend layer (provider emulation, fault injection) is opt-in — a pure UI-testing start needs no model, no API key, and no backend changes.

## Architecture Notes
The "model on first run, cached replay after" design is the detail that makes plain-English steps viable in CI without either a flaky fully-agentic run every time or an unmaintainable fixed script — a step only goes back to the model when the page has changed enough that the recording no longer fits. Faults are planned from what the test actually called and injected at danwo's own proxy, so there's nothing to configure per-fault and every result is reproducible.

## Maturity
Experimental: created 2026-10-21 (9 days old at review time), 17 stars, 2 forks, actively pushed, backed by a named company (Carbonme Inc.) with a real docs site, example gallery (8 example apps spanning Next.js/Hono/Postgres/Stripe, Clerk, Better Auth, SwiftUI, Jetpack Compose, Flutter), and a Chinese-language README — signals of intent beyond a weekend project despite the age. The project's own docs say plainly: "danwo is early: APIs will change."

## License
Apache-2.0 for the code, verified via GitHub API. The "dánwò" name and logo are trademarks of Carbonme Inc. — standard, not a commercial-use blocker.

## Alternatives
Playwright/Cypress/Detox directly (what this wraps and extends) give UI-driving without the provider emulation or fault matrix. No other self-hostable tool found combining UI-driving, real provider emulation, and systematic fault injection with an invariant-judging step in one framework.

## Risks / Limitations
Nine days old — the own-admitted API instability is real risk for anything beyond experimentation right now. Single-vendor-controlled proxy for provider emulation means new providers or edge-case behaviors depend on the maintainers keeping pace. No independent third-party coverage or adoption signal yet beyond the repo itself.

## Recommendation
PROTOTYPE — worth a spike on one checkout/webhook-handling flow to see whether the fault matrix catches a real idempotency gap, but hold off wiring it into a release-blocking CI gate until the API stabilizes.

## Change History
### 2026-10-30
Initial discovery and review.
