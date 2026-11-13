# Piwi

## Summary
A self-hosted dashboard for Playwright test results. CI normally discards
every report it generates; Piwi keeps them, clusters failures by root
cause, scores tests by flakiness, and suggests the locator you should have
used when a selector breaks. Ships as an npm reporter package, a
self-hosted server/dashboard (Docker image available), a browser extension,
and a desktop companion app.

## Why I Should Care
Flaky E2E tests are a recurring, silently-accepted tax in any
Playwright-based CI setup — nobody tracks them over time, so the same
flaky test gets re-investigated from scratch every time it fails. Piwi
turns that into a self-hosted, zero-telemetry alternative to paid
test-analytics SaaS (BuildPulse, Currents) for a MERN-stack project already
using Playwright.

## Problems It Can Remove
- Losing Playwright run history every time CI finishes.
- Manually re-investigating the same flaky test every time it resurfaces
  instead of seeing a flakiness score and history.
- Re-debugging a broken locator by hand instead of getting a suggested fix.

## Practical Uses
- Keeping Playwright CI history to see flaky-test trends instead of only
  the latest run.
- Triaging a batch of failing E2E tests by root-cause cluster instead of
  reading each failure individually.
- Deciding what to quarantine vs. actually fix using a flakiness score.
- Getting a locator-healing suggestion instead of re-selecting an element
  by hand after a UI change breaks a test.

## Product Opportunities
None directly for his own products, but the free-SDK / paid-or-restricted-
hosted-app split (MIT reporter/core, FSL dashboard/server) is a pattern
worth remembering for a future dev-tool micro-SaaS.

## Agent / Automation Opportunities
No MCP server yet, but the structured failure-cluster output is a plausible
input for a coding agent tasked with fixing a batch of flaky tests.

## Integration
`@piwitests/reporter` npm package wired into an existing Playwright config;
self-hosted server via the `phenx/piwitests-server` Docker image. Browser
extension and desktop companion are optional add-ons.
**Integration effort: Medium** — needs both the reporter package and a
running server, unlike a pure CLI tool.

## Architecture Notes
Split license boundary doubles as an architecture boundary: the
reporter/core/picker-dom packages that run inside your own project are MIT;
the dashboard, server, editor service, desktop app, browser extension, and
IDE plugins are FSL-1.1-MIT. That is, the SDK you embed is fully permissive;
the hosted application you self-host is time-delayed-open.

## Maturity
Created 2025-12-07 (~1 year old). 50 stars, 4 forks, 1 subscriber — thin
traction so far. 47 open issues against only 4 forks suggests a largely
single-maintainer roadmap tracked via issues rather than broad external
contribution yet. Latest npm release (reporter and server both at 0.69.0)
published 2026-11-12, so actively maintained day-to-day despite low
adoption numbers.

## License
MIT for `packages/reporter`, `packages/core`, `packages/picker-dom`,
`integrations/`, and `examples/` — the parts embedded in your own project.
**FSL-1.1-MIT** (Functional Source License, converts to MIT after a future
date set by the licensor) for everything self-hosted: the dashboard, server
package, editor service, desktop app, browser extension, and VS
Code/JetBrains plugins. Free to use, including internally at a company, but
restricted from being resold as a competing hosted product during the FSL
window — flagged per AGENT.md §14 since the pieces actually self-hosted
here are the FSL ones, not the MIT ones.

## Alternatives
- Playwright's own built-in HTML reporter (no history, no clustering, no
  locator healing).
- Paid CI test-analytics add-ons (BuildPulse, Currents).
- Manually grepping CI logs for recurring failures.

## Risks / Limitations
- Very small community (1 subscriber) despite real engineering depth —
  bus-factor risk if the single maintainer stops.
- FSL licensing on the dashboard/server means re-check terms before any
  resale or hosted-reselling plan; internal/personal self-hosting is fine.
- Locator-healing and failure-clustering claims not independently
  benchmarked by this review.

## Recommendation
**PROTOTYPE** — self-host it against one existing Playwright suite with a
known flaky test and see whether the root-cause clustering and flakiness
score actually save triage time.

## Change History
### 2026-11-13
Initial discovery and review. Rotation slot 3 (developer utilities,
debugging, testing).
