# browse (Anomaly)

## Summary
An MCP server plus a Chromium browser extension that lets a coding agent (OpenCode, Claude Code, Codex, Cursor, or any other MCP client) drive the user's own real browser — their actual tabs, saved logins, and installed extensions — instead of a separate, logged-out automation browser profile. Agents open tabs in a background tab group, read pages, click, type, screenshot and capture network traffic; when a login, 2FA prompt or payment screen appears, control is handed back to the human to click Continue before the agent resumes. Built by Anomaly, the company behind OpenCode.

## Why I Should Care
Most browser-automation-for-agents tools in this catalog (and in the broader market) operate an isolated, logged-out browser profile, which means re-establishing logins and losing installed extensions for every agent task. browse's core architectural choice — use the browser already logged into everything, and hand control back for anything sensitive — directly removes the recurring "the agent needs me logged in somewhere" friction without ever copying credentials anywhere.

## Problems It Can Remove
- Re-authenticating a headless or isolated browser session for a task that only needs an account already logged into the developer's day-to-day browser.
- Manually driving the browser for anything scriptable (form fills, repeated data entry on an internal tool without a public API) while still staying in the loop for 2FA or payment steps.
- Losing access to installed browser extensions (password managers, dev tools) when switching to an automation-only browser profile.

## Practical Uses
- Fill out and submit a form, or scrape an authenticated internal dashboard, using the developer's already-logged-in session.
- Automate a workflow against a SaaS tool without a public API by driving its real web UI.
- Visual QA or screenshot capture of a site exactly as it renders with the developer's actual extensions and profile.
- Record a network trace of an agent's browsing session to debug an integration.

## Product Opportunities
None directly — this is an agent-tooling product, not a building block for Sourav's own products.

## Agent / Automation Opportunities
The install flow writes native MCP configuration for 10+ agent clients (OpenCode, Claude Code, Claude Desktop, Cursor, Codex, VS Code/Copilot agent mode, Windsurf, Amp, Antigravity, Augment, Cline, CodeBuddy, Cortex Code and more) in a single step, picking up whichever are already installed. The human-handback protocol for logins/2FA/payment is a reusable pattern worth studying for any other agent tool that needs to act inside an authenticated session without ever holding the credentials itself.

## Integration
```sh
curl -fsSL https://opencode.ai/browse/install | bash
```
Also installable via npm/bun/pnpm/yarn/Volta as a global package, each updating itself through that package manager. The installer verifies the downloaded binary's SHA-256 (or npm's SHA-512 integrity) before installing. Requires pairing a Chromium-family browser extension (Chrome, Edge, Brave, Arc, Helium, Vivaldi, Opera) in addition to the native binary. Integration effort: **Medium** — more setup than a pure CLI/MCP tool, but a one-time step with a guided picker for which agents to wire up.

## Architecture Notes
A small native binary (the actual MCP server) paired with a browser extension that grants it access to the real browser; tabs the agent opens are grouped out of the way rather than interleaved with the user's own tabs. The install script reads a signed release manifest from Anomaly's own release service and checks binary integrity before installing — a more deliberate release pipeline than most single-maintainer MCP-server projects in this catalog.

## Maturity
Emerging. v0.1.5 tagged 2026-11-17, created 2026-10-27 — about a month old at review, but shipped with CI, a real install/update pipeline across five package managers, and backing from Anomaly (the OpenCode company) rather than a solo maintainer.

## License
MIT, confirmed via the repo's license badge. No commercial-use restrictions.

## Alternatives
Playwright-MCP-style wrappers and browser-use/Stagehand-class tools (already evaluated in prior runs — a saturated niche in this catalog) operate an isolated, logged-out automation browser profile, requiring fresh logins for every task. Manual browser use by the developer is the default this replaces for the scriptable parts of a workflow that still need an existing login.

## Risks / Limitations
- Young (about a month old) with a star:subscriber ratio (~362:1) on the high side for this catalog, though 82 forks and 35 open issues suggest real usage rather than inflated stars.
- Requires both a native binary and a browser extension — more moving parts than a pure CLI/MCP tool, and another thing to keep updated.
- Backed by a company (Anomaly) building a commercial product (OpenCode) around this ecosystem — worth watching for feature gating toward a paid tier as the project matures.

## Recommendation
PROTOTYPE — worth setting up for one real workflow that currently requires manual browser use because of an existing login, to validate the handback behavior for sensitive steps before relying on it more broadly.

## Change History
### 2026-11-25
First seen and reviewed. Discovered via GitHub search for AI-agent browser-control keywords (slot 1: AI agents, MCP, coding productivity). Verified license, stars/forks/subscribers and release tag via the GitHub API; README read directly to confirm the real-browser (vs. isolated-profile) architecture and its distinction from the already-evaluated browser-automation wave.
