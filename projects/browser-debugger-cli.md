# Browser Debugger CLI (bdg)

## Summary
An npm CLI that keeps a Chrome session open in the background and exposes DOM, network, console, cookies, and raw Chrome DevTools Protocol (CDP) as short shell commands — built explicitly so a coding agent can drive and debug a browser from the shell without an MCP server, a Playwright script, or screenshots.

## Why I Should Care
Directly targets the AI-agent-leverage axis: it's positioned against both MCP-based browser tools (extra setup, heavier token usage from screenshots/structured state) and hand-written Playwright scripts (overkill for a one-off check). Compact, pipeable, agent-friendly output.

## Problems It Can Remove
Removes the need to set up an MCP browser server or write a throwaway Playwright script just to answer "why did this click not work" — network/console/cookie/DOM state is one short CLI command away.

## Practical Uses
- Debug a broken frontend flow: `bdg network getCookies`, `bdg eval "..."`, `bdg dom a11y tree` to see what actually happened after a click.
- Reuse an already-authenticated browser session's cookies from the shell, e.g. piping `bdg network getCookies --json` into `curl` to hit an authenticated API endpoint directly.
- Inspect accessibility tree and raw CDP metrics (`bdg cdp Performance.getMetrics`) without opening DevTools.
- Script-free automation: click/fill/navigate one command at a time, reading real output to decide the next step, instead of writing a full test script upfront.

## Product Opportunities
Could shorten the debug loop on any React/Next.js frontend when pairing with a coding agent — less relevant as a product component itself, more a personal workflow accelerant.

## Agent / Automation Opportunities
This is the whole point of the tool: plain-process, compact output designed to pipe into `jq` and cost an agent few tokens, explicitly built for Claude Code, Codex, and Gemini CLI as first-class users.

## Integration
`npm install -g browser-debugger-cli`, drives a local (or remote) Chrome instance via CDP. No MCP client or server setup required. Low integration effort.

## Architecture Notes
Thin, stable CLI wrapper over raw CDP — DOM/network/console/cookie commands plus an `bdg cdp <method>` passthrough for anything not covered by a dedicated command. The explicit design choice to avoid MCP and screenshots is the main architectural point worth studying: it trades some protocol richness for much lower token cost and zero extra moving parts.

## Maturity
Emerging: created 2025-10-31 (~1 year old), 170 stars, 1 watcher, current release v0.15.0 (2026-10-08, pushed today). npm registry confirms v0.15.0/MIT matches the GitHub tag exactly. Small sample size — worth a hands-on trial before depending on it.

## License
MIT — unrestricted.

## Alternatives
- MCP browser servers (Playwright MCP, browser-use, etc.) — require an MCP client and typically cost more tokens per call.
- **Chrome DevTools MCP** (ChromeDevTools/chrome-devtools-mcp, already catalogued) — MCP-based, heavier integration; bdg is a plain CLI with no MCP dependency.
- Hand-written Playwright scripts — the status quo for anything beyond a one-off check.

## Risks / Limitations
- Chrome/CDP-only, no Firefox/Safari.
- Small community (170 stars, 1 watcher) — limited independent validation so far.
- Fast release cadence suggests the CLI surface may still shift.

## Recommendation
PROTOTYPE — try it on the next frontend debugging session alongside Claude Code before deciding whether it replaces ad hoc MCP browser tooling.

## Change History
### 2026-10-09
First catalogued. Slot 3 (developer utilities, debugging, testing) discovery run. GitHub API + npm registry verified: MIT, 170 stars, pushed today, not archived.
