# Saccade

## Summary
An MCP server (Node/npm package `@nanlogic/saccade`) plus a Chrome/Edge browser extension that gives an AI agent a leased, single-writer view of one authorized browser tab as a compact semantic object model with revision deltas, rather than repeatedly shipping the agent a full-page DOM/accessibility-tree dump. Actions (click, fill, submit) wait locally for actionability and return a verified semantic transition in the same call, instead of the agent having to re-read the page to confirm an action worked.

## Why I should care
Browser-automation agents built on generic page-scrape-and-click loops burn tokens re-transferring whole pages and are fragile against dynamic content, iframes, and pages that mutate mid-task — exactly the kind of admin-panel and dashboard automation Sourav would plausibly want an agent to handle (e.g., automating tasks inside the MSSP admin portal or third-party SaaS consoles without an API). Saccade's tab-leasing and delta model is a genuine architectural answer to that, not just a Playwright wrapper.

## Problems It Can Remove
Removes the need to hand-build page-state diffing, action-verification, and tab-isolation logic when wiring a coding/browser agent to operate a real logged-in web session — the "did that click actually register" problem that ad hoc browser-agent scripts usually solve badly or not at all.

## Practical Uses
- Automating repetitive tasks inside internal admin panels or third-party SaaS dashboards that lack an API, via an MCP-connected agent.
- Long, dynamic multi-step forms where naive full-page transfer per step is slow and error-prone.
- Any agent workflow that needs to stay attached to a human's already-authenticated browser session rather than spinning up a clean headless context.

## Product Opportunities
Could underpin an internal "agent operates our own admin console" feature for the MSSP portal — using an agent to drive routine console tasks that don't have a first-class API yet.

## Agent / Automation Opportunities
Ships six MCP tools (capabilities, tab list/open, and semantic read/act tools) — usable directly as an MCP server in any MCP-compatible agent client (Claude Code, etc.) via `npx -y @nanlogic/saccade mcp`.

## Integration
`npx -y @nanlogic/saccade install` wires the MCP server into supported local agent clients; requires Node.js 18+ and loading the extension in Chrome/Edge (unpacked during prerelease). Fully JS/TS-native — no foreign-ecosystem tax for Sourav's stack. Integration effort: **Low** for trying it, **Medium** for depending on it given prerelease extension packaging.

## Architecture Notes
Session-owned tab leases (one writer per tab) plus a "current Truth" object with pushed revision deltas instead of polling; actions carry their own postcondition check baked into the response. The project publishes an honest, structured comparison against Playwright (same-model benchmark: Saccade used fewer browser tool calls and less time inside the browser MCP path per call, but Playwright finished the overall task faster end-to-end) rather than claiming a blanket win — a good signal of engineering rigor for a single-contributor project.

## Maturity
Experimental. Created 2026-07-28, ~74 commits from a single contributor by 2026-08-31, npm package at 0.2.0 (registry-confirmed) against a README claiming 0.2.1 — check the exact installed version before relying on version-specific behavior. 2 GitHub stars. Very early but with unusually thorough internal documentation (release gates, comparison reports, control-coverage docs).

## License
Apache-2.0 (GitHub API-confirmed). No commercial-use restrictions.

## Alternatives
Playwright/Puppeteer driven directly by an agent (the dominant status quo — deterministic, mature, but not built for "stay attached to a live user session with verified actions" semantics); browserbase and similar hosted browser-agent infrastructure (cloud-hosted, not self-hosted/local).

## Risks / Limitations
- Single contributor, 2 stars, ~5 weeks old — meaningful bus-factor and continuity risk.
- Extension is still loaded as "unpacked" during prerelease, not through a store — an extra install/trust step.
- README version (0.2.1) and npm registry latest tag (0.2.0) disagree at time of review — verify the actual published version before adoption.

## Recommendation
PROTOTYPE — worth a hands-on trial against a real internal admin-panel automation task, specifically because it's Node-native and directly usable via `npx`, but track it as unproven infrastructure, not a Playwright replacement.

## Change History
### 2026-09-01
Initial discovery and review. Rotation slot 7 (experimental / hidden gems).
