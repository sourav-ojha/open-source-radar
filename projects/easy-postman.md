# EasyPostman

## Summary
An open-source Java 17 desktop app combining Postman-style API debugging (HTTP/SSE/WebSocket, collections, pre/post scripts, assertions) with JMeter-style load testing (thread groups, distributed master/worker runs, live metrics) in one tool. The same workspace runs headless via CLI in CI or is exposed to AI clients (Codex, Claude Desktop, Cursor) through a built-in MCP server.

## Why I Should Care
Most free Postman alternatives (Bruno, Hoppscotch, Insomnia) stop at the request-collection layer. This one bundles load testing and an MCP server in the same local-first app, covering more of the API-development lifecycle without adding a paid Postman Team seat.

## Problems It Can Remove
Removes the recurring cost of a Postman Team plan for collection sharing (replaced by Git-backed workspaces) and the need for a separate JMeter setup for load testing.

## Practical Uses
- Share API collections across a team via Git workspaces instead of a paid Postman workspace.
- Run the same collection as both a functional test and a JMeter-style load test.
- Run collections headless in CI via the CLI instead of Newman.
- Let an AI client (Claude Desktop, Cursor, Codex) call saved API requests directly through the built-in MCP server.

## Product Opportunities
Removing a SaaS seat cost (Postman Team) for any team-collaboration use case is a direct, if modest, product-opportunity signal rather than something to build into a product.

## Agent / Automation Opportunities
Built-in MCP server lets an agent run a saved, pre-authenticated API request directly instead of being handed a curl command to reconstruct from scratch.

## Integration
Native installers for Windows/macOS/Linux (bundled runtime, no separate Java install needed), a cross-platform JAR for anyone running Java 17+ directly, a headless CLI for CI, and an MCP server. Low integration effort as an end-user desktop tool — no library/ecosystem adoption required.

## Architecture Notes
Local-first by default (collections/environments/test data stored locally), with Git workspaces layered on top for team collaboration rather than a hosted sync service. The same underlying workspace model is shared across the desktop GUI, headless CLI, and MCP server, which is the interesting part architecturally — one data model, three different access surfaces.

## Maturity
Emerging: created 2025-06-18, 722 stars, 61 forks, current release v6.2.4 (2026-10-08, pushed today). Smaller community than Bruno/Hoppscotch/Insomnia, so fewer eyes on edge cases. No official project website beyond GitHub/Gitee.

## License
Apache-2.0 — unrestricted commercial use.

## Alternatives
- **Bruno / Hoppscotch / Insomnia** — free Postman alternatives, none bundle JMeter-style load testing.
- **Postman** (paid Team tier) — the SaaS cost this removes.
- **k6 / JMeter standalone** — load testing without the API-client/collections layer.

## Risks / Limitations
- Java 17 desktop app, outside the core JS/TS stack (acceptable since it's used as an end-user tool, not a library dependency).
- Smaller community than the mainstream free Postman alternatives.
- No independent project website to cross-check provenance beyond GitHub/Gitee.

## Recommendation
PROTOTYPE — worth trying for the next team API-collection-sharing or load-testing need before reaching for Postman Team or a separate JMeter setup.

## Change History
### 2026-10-09
First catalogued. Slot 3 (developer utilities, debugging, testing) discovery run. GitHub API verified: Apache-2.0, 722 stars, pushed today, not archived.
