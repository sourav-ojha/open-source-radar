# Bombadil

## Summary
A property-based testing tool for web and terminal UIs from Antithesis (the deterministic-simulation-testing company founded by ex-FoundationDB engineers). Instead of scripting fixed click-paths like Playwright/Cypress, you write TypeScript "properties" and action generators describing what should always hold (no console errors, a form never silently loses data, a modal always has an escape path); Bombadil autonomously explores the UI trying to break those invariants and reports a minimal reproduction when it finds one.

## Why I Should Care
Scripted E2E tests only check the paths someone thought to write. Bombadil's autonomous exploration finds interaction-order bugs (double-submits, race conditions between two async actions, a modal state left dangling after a browser back) that nobody would think to write a Playwright test for. Runs standalone — no Antithesis account or paid platform required for the core testing loop.

## Problems It Can Remove
Reduces the manual effort of writing exhaustive E2E scripts for state-heavy React UIs, and catches the class of bugs that only show up from an unusual sequence of user actions rather than a single scripted flow.

## Practical Uses
- Fuzz a React admin panel or checkout flow for a day in CI with a time limit, catching state-corruption bugs before users do.
- Define "no resource leak" properties (e.g., a WebSocket connection always closes when a component unmounts) and let Bombadil hunt for violations.
- Run against a terminal UI (the tool supports both browser and terminal drivers) — directly relevant if building CLI/TUI tooling.
- Use as a GitHub Action (`antithesishq/bombadil-action`) to gate merges on UI-invariant violations.

## Product Opportunities
None directly — this is a testing tool, not a building block to embed in a product. Its value is purely as internal QA tooling.

## Agent / Automation Opportunities
The specification language and "action generator" model is a pattern worth studying for building an agent that explores an app's UI state space on its own (same underlying idea as autonomous UI testing, applicable to agentic QA tooling). No MCP server; it's a CLI/GitHub Action/npm package.

## Integration
```sh
npm install --save-dev @antithesishq/bombadil
```
Also distributed as standalone binaries (macOS/Linux), a Docker image, a Nix flake, and a GitHub Action. Writing specifications requires learning Bombadil's TypeScript-based property/action-generator DSL — not a drop-in replacement for existing E2E suites, but can run alongside them. Integration effort: **Medium** — install is trivial, but getting value requires writing real specifications, not just pointing it at a URL.

## Architecture Notes
Property-based testing (QuickCheck-style) applied to UI interaction sequences rather than data generation — a genuinely different testing paradigm from the record/replay or hand-scripted approaches that dominate E2E testing today. Worth studying even independent of adoption.

## Maturity
Emerging. Created 2025-12-17, v0.8.0 as of 2026-11-19, actively pushed (same day as review). 3,129 stars / 114 forks / 7 subscribers / 44 open issues. The stars:subscribers ratio (~447:1) is elevated, but Antithesis is a real, independently known company (not a fresh anonymous account), and the fork count plus open-issue activity look organic rather than inflated.

## License
MIT. No commercial-use restrictions. Backed by a company with a commercial paid platform (Antithesis itself), but the OSS CLI, SDK and GitHub Action are fully standalone — confirmed no account or paid service is required to run the core property-testing loop.

## Alternatives
Playwright and Cypress (scripted, not property-based — already excluded from recommendation as mainstream); QuickCheck-family libraries like fast-check (data-level property testing, not UI-interaction-level). Bombadil's distinguishing feature is applying property-based testing to UI *interaction sequences* specifically, which none of the mainstream tools do natively.

## Risks / Limitations
- Requires learning a new specification DSL — not a zero-effort drop-in.
- Elevated stars:subscribers ratio; worth re-confirming organic growth on a future check.
- Young (11 months old) relative to the maturity of the company behind it; the OSS product is clearly newer than the core Antithesis platform.

## Recommendation
PROTOTYPE — worth a focused trial against one state-heavy React surface (e.g. a checkout or admin flow) to see what it actually surfaces before committing specification-writing time more broadly.

## Change History
### 2026-11-24
First seen and reviewed. Discovered via GitHub search for property-based-testing topic tag. Verified standalone installability (no Antithesis account required) by reading the installation docs directly.
