# AnythingMCP

## Summary
AnythingMCP is a self-hosted MCP gateway that converts REST, SOAP/WSDL, GraphQL, and
SQL/NoSQL databases (plus other MCP servers) into MCP tools automatically — import a spec
or point it at a connection string — with OAuth2, RBAC, and audit logging built in, plus
175+ pre-built adapters for common SaaS APIs.

## Why I Should Care
Wrapping an existing API or database as an MCP tool is normally a bespoke, hand-written
server per integration. This does it generically, and — unlike most hand-rolled MCP
wrappers — ships RBAC and an audit log by default, which matters the moment an agent gets
access to anything customer-facing.

## Problems It Can Remove
Removes the "write a small MCP server for this one API" chore that recurs every time a new
agent-accessible integration is needed, and the "who can call what, and is it logged"
gap that most quick MCP wrappers skip.

## Practical Uses
- Expose the MERN admin-portal's existing REST API to Claude/agents as MCP tools without
  hand-writing a wrapper server.
- Wrap a Postgres/Mongo read replica as queryable MCP tools for internal agent workflows.
- Give agents controlled, audited access to third-party SaaS APIs already in use, with
  RBAC instead of a shared API key.

## Product Opportunities
Fast path to prototyping an "AI agent acts on your data" feature for the MSSP
admin-portal SaaS without a bespoke MCP server per integration — worth a hands-on
prototype before considering it for anything customer-facing.

## Agent / Automation Opportunities
This is the core product: turning arbitrary existing infrastructure into MCP tools with
auth and audit built in, usable from Claude, ChatGPT, Gemini, Copilot, and Cursor.

## Integration
Docker Compose (`docker compose up -d` after copying `.env.example`), or the 60-second
guided setup. Requires Docker 24+. Integration effort: **Medium** — quick to stand up, more
verification needed to trust the "no-code" spec-import claim against a real-world API.

## Architecture Notes
Builds a "knowledge graph" of how imported APIs/data sources relate to each other and
teaches agents how to use them together, on top of the MCP protocol layer. Also exposes a
"turn your API into a ChatGPT app" path in addition to raw MCP tool exposure.

## Maturity
Emerging. Created 2026-02-28 (~6 months old), active (v0.4.4 released 2026-08-27), 185
stars, 14 open issues, 7 distinct contributors visible (including a Claude-attributed
commit account).

## License
**AGPL-3.0 — flagged loudly.** Fine for internal agent tooling wrapping owned APIs. If this
gateway itself were exposed as a hosted, customer-facing feature of a commercial product,
AGPL's network-copyleft clause requires releasing modifications' source, or the vendor's
managed cloud offering should be used instead.

## Alternatives
Hand-rolled MCP servers per API (status quo), aws/mcp-proxy-for-aws (narrower, AWS/IAM-only,
also catalogued as Worth Watching today), Composio/Pipedream-style commercial connector
platforms, OpenConnector (flagged Worth Watching on 2026-08-30, similar "connector gateway
for agents" positioning but not MCP-native or database-focused in the same way).

## Risks / Limitations
- AGPL-3.0 network-copyleft (see License).
- "No-code" conversion claim needs hands-on verification against a real OpenAPI spec with
  non-trivial auth before trusting it for anything customer-facing.
- Young (185 stars, 14 open issues) — adapter coverage for less-common APIs may be thinner
  than the "175+ pre-built adapters" headline suggests.

## Recommendation
**PROTOTYPE** — worth wrapping one existing internal API with it and testing agent-driven
calls end-to-end (including the RBAC/audit path) before considering wider adoption.

## Change History
### 2026-08-31
Initial discovery and review. GitHub API confirmed AGPL-3.0, 185 stars, created
2026-02-28, pushed_at 2026-08-31, not archived. README verified via
raw.githubusercontent.com.
