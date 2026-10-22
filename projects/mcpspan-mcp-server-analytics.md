# mcpspan

## Summary
Self-hosted, open-source analytics and observability for MCP servers. Add one `instrument(...)` call via an SDK (TypeScript, Python, Go, C#/.NET, Java/Kotlin, Rust, Ruby, or PHP, all checked against a shared conformance test suite) and get per-tool/resource/prompt call counts by client, latency percentiles, response-size distributions, the token cost of the tool list an agent actually sees, failure/rejection tracking with suggested close matches, step-by-step session replay, and release-over-release comparison — in a self-hosted dashboard, or forwarded to OpenTelemetry.

## Why I Should Care
Nothing else in the catalog answers "which of my MCP tools are agents actually using, and what is the tool list costing me in context tokens." Any MCP server shipped as part of a product or internal agent tooling currently flies blind on this; mcpspan is purpose-built for exactly that gap.

## Problems It Can Remove
Hand-rolled logging inside an MCP server to answer basic questions ("is this tool ever called", "why did that call fail", "did the last release regress latency") — each of those is normally bespoke, per-server, per-language code.

## Practical Uses
- Instrument an MCP server to find tools that are defined (and costing context tokens) but never called, then prune them
- Alert on error-rate spikes or a server going quiet (Slack/Discord/webhook)
- Compare latency and usage before/after a release to catch a regression in a specific tool
- Export NDJSON session traces to debug one specific agent interaction

## Product Opportunities
If an MCP server ships as part of a product (e.g. an agent-facing API surface for the MSSP admin portal), this is the missing observability layer most MCP servers don't get by default — a concrete differentiator for anyone shipping agent tooling commercially.

## Agent / Automation Opportunities
This *is* agent infrastructure — it specifically instruments the agent-to-MCP-server interaction layer, which generic LLM-call tracing tools (Laminar, Langfuse) don't model.

## Integration
Deployment: self-hosted via Docker Compose (`docker compose up -d`); add the SDK to the MCP server and point it at the instance plus an API key. Optional settings (ports, data retention, API address) live in a `.env` file.
Interfaces: web dashboard, per-language SDK, OpenTelemetry export, CSV/NDJSON export, Slack/Discord/webhook alerts.
Integration effort: **Low** — one SDK call per server, Docker Compose for the backend.

## Architecture Notes
Parameter values stay within the instrumented server's own process (not sent to mcpspan) unless explicitly exported; error messages can be disabled since tool output may contain sensitive data. The SDK measures each tool call without changing its behavior. Server-side storage/internals are not documented in the public README — worth confirming retention mechanics directly before production use.

## Maturity
Emerging — repo created 2026-10-03, 20 stars, single contributor. Despite the youth, the feature surface (8-language SDK parity with a shared conformance suite, OTel bridging, release-comparison dashboard) is unusually broad for how new the project is.

## License
MIT. No restrictions on commercial use, SaaS deployment, or embedding.

## Alternatives
- Laminar / Langfuse-style LLM tracing (Laminar catalogued) — covers general LLM call tracing but not MCP-protocol-specific concepts like tool-definition token cost or "tools shown but never called"
- Hand-rolled logging inside the MCP server — the status quo this replaces

## Risks / Limitations
- Single-maintainer project (1 contributor on GitHub) — bus-factor risk
- Very new (created 2026-10-03), 20 stars — unproven at any real scale
- Storage/retention internals undocumented in the README; verify before production use
- Discovered via a low-traction Show HN post (1 point) — catalogued on architecture/README substance rather than external validation

## Recommendation
**PROTOTYPE** (score 7.8/10) — worth instrumenting any MCP server already in use to see what the data actually looks like before depending on it.

## Change History
### 2026-10-22
First discovered via Hacker News Show HN. Verified via GitHub API (MIT, 20 stars, 0 subscribers, 0 forks, created 2026-10-03, 1 contributor).
