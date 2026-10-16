# Cakewalk MCP Test Kitchen

## Summary
Configurable MCP (Model Context Protocol) server testbed for exercising MCP clients and
gateways against fault-injection scenarios — HTTP error sequences, JSON-RPC errors, tool
failures, elicitation/approval flows, and SDK-compatibility checks — with a live
request-observation log in a React admin UI.

## Why I Should Care
There is a generic-API mock/fault-injection niche (mockd, Pikopod) already in the
catalog, but nothing targets the MCP protocol specifically. If I'm building or
integrating MCP servers/gateways, this is the only tool found so far that lets me
deliberately break MCP transport/protocol behavior to see how a client reacts, instead
of discovering it in production.

## Problems It Can Remove
Hand-writing a throwaway broken MCP server to test retry/backoff or error-handling logic
in an MCP client or gateway I'm building.

## Practical Uses
- Point an in-development MCP client at a scenario that returns 503 then 200 to verify
  retry/backoff behavior
- Verify a client handles a JSON-RPC error or a tool result flagged `isError: true`
  without crashing
- Test elicitation/approval UX flows before shipping an MCP integration
- Check compatibility against the MCP C# SDK 2.0 / 2026-07-28 protocol revision

## Product Opportunities
None specific — internal testing tool, not a product-embeddable component.

## Agent / Automation Opportunities
Directly useful for anyone building MCP servers, gateways, or proxies as part of an
agent-tooling stack — including validating a self-built MCP gateway before trusting it
with a real agent session.

## Integration
Quickest path is Docker Compose (`docker compose up --build`), serving API/MCP
endpoint/UI on `localhost:5094` with a default dev PAT. Local development needs .NET 10
SDK, Node 22.17.1, and Docker for Postgres — heavier than a typical standalone testing
CLI. A hosted live demo exists (Google/GitHub sign-in). Medium integration effort.

## Architecture Notes
Serves a Streamable HTTP MCP endpoint; per-user scenario selection applies to the user's
next MCP session. Always-on resources/prompts include deliberately schema-invalid
responses for malformed-response testing. Several documented scenarios (timeouts, auth
failures, catalog changes) exist in the codebase but aren't registered by default yet.

## Maturity
Experimental. Created 2026-10-07 — 9 days old at review — with 4 stars and a single
visible contributor (andrea-delpopolo). Genuinely novel niche, but production use would
be premature.

## License
MIT — no restrictions.

## Alternatives
mockd (catalogued, multi-protocol mock server) and Pikopod (catalogued, API-failure
sandbox) cover generic REST/gRPC/WS fault injection. Neither targets MCP's own
transport/protocol semantics (JSON-RPC errors, elicitation, SDK compatibility), which is
this project's actual differentiator.

## Risks / Limitations
- Extremely early (9 days old, 4 stars, one contributor) — treat as experimental
- Several scenarios exist in code but are not wired up by default
- Stack (.NET 10 + Node + Docker/Postgres) is heavier than the problem strictly requires

## Recommendation
PROTOTYPE — worth a trial run against any MCP client/gateway currently being built, with
the expectation that the project itself may need patching or may stall given its age.

## Change History
### 2026-10-16
First discovery and review.
