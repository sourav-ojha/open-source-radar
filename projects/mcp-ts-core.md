# mcp-ts-core

## Summary
An agent-native TypeScript framework for building MCP servers: declarative tool/resource definitions, built-in auth, multi-backend storage, OpenTelemetry instrumentation, and first-class support for Bun/Node/Cloudflare Workers.

## Why I Should Care
It's a Node/TypeScript-native framework for exactly the kind of MCP-server building work the primary relevance axis calls out — auth, storage, and OpenTelemetry are the boilerplate that otherwise gets rebuilt per project.

## Problems It Can Remove
Auth handling, pluggable storage backends, and OpenTelemetry wiring for an MCP server is a week or more of setup that this framework replaces with declarative config.

## Practical Uses
- Scaffold a new MCP server for an internal tool without hand-rolling auth, storage, and telemetry plumbing
- Build an MCP server as a distributable product feature (e.g. exposing a Sourav-built SaaS as an MCP tool for customers' agents)
- Standardize how future MCP servers in his projects handle auth/observability instead of reinventing it each time

## Product Opportunities
- Directly enables 'expose as MCP server' as a shippable feature for any of Sourav's Node/TS products — matches primary relevance axis 1 exactly

## Agent / Automation Opportunities
- This is infrastructure for building MCP servers, not an MCP server itself — the core opportunity is using it to build his own

## Integration
Deployment: npm package (@cyanheads/mcp-ts-core), library, Cloudflare Workers
Interfaces: TypeScript library/framework
Integration effort: **Low**

## Architecture Notes
GitHub API verified: Apache-2.0, 148 stars, created 2025-03-20 (over a year old, actively maintained), pushed_at 2026-08-21. npm registry confirms latest 0.12.3 published 2026-08-21, matching GitHub tag.

## Maturity
Emerging. GitHub API verified: Apache-2.0, 148 stars, created 2025-03-20 (over a year old, actively maintained), pushed_at 2026-08-21. npm registry confirms latest 0.12.3 published 2026-08-21, matching GitHub tag.

## License
Apache-2.0. Apache-2.0, no restrictions.

## Alternatives
- Official @modelcontextprotocol/sdk (lower-level, no built-in auth/storage/telemetry conveniences)
- Miragon/mcp-toolkit (similar goal, newer/thinner, built on mcp-use)

## Risks / Limitations
- 148 stars — smaller community than the official SDK; verify it tracks MCP spec changes promptly before depending on it for a shipped product
- Declarative abstraction adds a layer versus using the official SDK directly — evaluate whether the convenience is worth the indirection for a simple server

## Recommendation
**PROTOTYPE** (score 7.8/10) — see Why I Should Care above.

## Change History
### 2026-09-02
First discovered and reviewed. Verified via GitHub API: license Apache-2.0, current version 0.12.3 (2026-08-21).
