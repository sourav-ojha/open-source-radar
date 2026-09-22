# peerd

## Summary
peerd is a browser extension (Chrome + Firefox) that runs a full AI agent loop natively inside the browser using only browser primitives — Workers, origins, sandboxing, OPFS, WASM/WASI, WebRTC, WebAuthn. It drives your existing signed-in tabs directly rather than spinning up a separate cloud browser or requiring the agent to have host OS access. No backend, no account, BYOK for the LLM, and current builds send no telemetry.

## Why I Should Care
The current browser-automation-for-agents landscape mostly falls into two camps: cloud browser services (Browserbase-style, someone else's machine) or full local computer-use (the agent gets your whole OS). peerd's pitch — "agent platforms are trying to pull the browser into the harness, peerd pulls the harness into the browser" — is a genuinely different architecture: it uses the browser's own three-decade-old security boundaries (site isolation, sandboxed workers) instead of building new ones, and it can reuse your already-logged-in sessions without re-authenticating or exposing credentials to the agent.

## Problems It Can Remove
- Paying for/operating a cloud browser service just to let an agent act on a site you're already logged into.
- The credential and blast-radius exposure of giving an agent full computer-use access to automate a browser task.
- Re-authentication friction when automating a task against a site with 2FA/session-based login.

## Practical Uses
- Automating repetitive logged-in-web-app tasks (admin panel data entry, SaaS dashboard checks) without a separate scraping/automation credential.
- Learning a site's UI once ("builds reusable site clients") and reusing that client for later automation tasks.
- Running compute-heavy agent subtasks (JS notebooks, WASI tools, small Linux WebVMs) fully client-side with no server cost.
- Prototyping a personal browser-based research/ops assistant without standing up backend infrastructure.

## Product Opportunities
The "keyless actor per tab/environment" isolation model is a useful pattern to borrow for any product feature that lets an AI agent touch a user's browser session — e.g., a support tool that needs to reproduce a customer's UI issue without ever seeing their credentials.

## Agent / Automation Opportunities
This is itself a general-purpose agent runtime, not just a browser-automation tool — it can delegate to compiled WASI tools, sealed JS notebooks, "Apps" with no ambient network access, and full Linux WebVMs (Chrome-only), all inside the browser sandbox. Preview builds add signed peer-to-peer agent-to-agent communication over WebRTC.

## Integration
- **Installed locally**: browser extension (Manifest V3), Chrome and Firefox.
- **Self-hosted**: N/A in the traditional sense — it runs entirely client-side; no server to host.
- **Interfaces**: browser extension UI; no CLI, HTTP API, or MCP server surface (by design — it lives inside the browser, not alongside it).
- **Integration effort: Low** to try (install the extension, bring your own API key), but it is not embeddable into a backend product the way a library would be — its value is personal/interactive use, not product infrastructure.

## Architecture Notes
Non-orchestrator agent loops run in separate dedicated worker heaps on Chrome and Firefox; if the browser cannot prove that isolation boundary, the actor request is refused outright rather than degrading gracefully. Apps (one of the compute environment types) have no ambient network access at all — external HTTP/HTTPS links require explicit user confirmation. The engineering discipline is unusually visible for a 3-month-old project: badges for in-browser Chrome/Gecko test suites, E2E side-panel tests, a documented red-team results file, vendored-code integrity checks, and pinned GitHub Actions.

## Maturity
Emerging. Created June 2026 (~3 months old), 410 stars, 7 contributors, Apache-2.0, tagged releases (v0.7.3). Explicitly documents its own threat model and known limitations (`docs/security/THREAT-MODEL.md`) rather than claiming completeness.

## License
Apache-2.0 — no commercial-use restrictions.

## Alternatives
Browserbase/Browser-use (cloud browser services), Anthropic/OpenAI computer-use (full host OS access), existing "stealth browser for agents" tools (catalogued/rejected wave: CloakBrowser, obscura, ego-lite). peerd's differentiation is running fully client-side with no backend and reusing the browser's own isolation model instead of a new one.

## Risks / Limitations
- Browser-only scope — cannot automate native apps, dev servers, or anything outside what a browser extension can reach (WebVMs partially bridge this on Chrome only).
- 3 months old; "Red Team" results badge exists but independent security review has not happened at scale.
- Firefox support is explicitly behind Chrome (offscreen-document-dependent features removed on Firefox; WebVMs are Chrome-only).
- Peer-to-peer/agent-to-agent features are preview-only and pruned from store packages.

## Recommendation
PROTOTYPE — install it against a low-stakes logged-in site to evaluate the actor-isolation UX firsthand; the architecture is worth studying regardless of whether it becomes a daily-driver tool.

## Change History
### 2026-09-22
Initial discovery and review. Rotation slot 7 (experimental/hidden gems); surfaced via GitHub search for "browser automation agent."
