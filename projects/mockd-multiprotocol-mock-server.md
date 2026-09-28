# mockd

## Summary
A single Go binary that mocks HTTP, gRPC, GraphQL, WebSocket, MQTT, SSE, and SOAP from one CLI. Imports OpenAPI/Postman/HAR/WSDL specs, supports stateful CRUD mocks, chaos-injection profiles and circuit breakers, and ships a local MCP server so a coding agent can create or mutate mocks itself.

## Why I Should Care
Every mainstream mock tool forces a trade-off: one protocol, a JVM/Electron runtime, or a paid cloud tier for chaos/circuit-breaker features. mockd bundles nine protocols, fault injection, and an MCP server into one dependency-free binary under Apache-2.0.

## Problems It Can Remove
Standing up WireMock (JVM) or MockServer (JVM) just to mock one HTTP endpoint; hand-writing separate mock setups for WebSocket/MQTT/gRPC endpoints when they're each a different tool; building custom timeout/circuit-breaker fault injection for resilience tests.

## Practical Uses
- Mocking a third-party API (payments, messaging) during development before real credentials or a sandbox account exist.
- Integration tests against WebSocket/MQTT/gRPC/SOAP endpoints without standing up the real infrastructure.
- Chaos-testing client resilience (timeouts, circuit-breaker trips, bandwidth throttling) before it happens in production.
- Letting a coding agent spin up or adjust a mock backend for a frontend task via its built-in MCP server.

## Product Opportunities
Usable as the mock layer inside an internal contract-testing pipeline for any product with third-party API dependencies.

## Agent / Automation Opportunities
Ships a local MCP server specifically so an agent can generate/modify mocks — this is an explicit, advertised first-class agent integration, not an afterthought.

## Integration
`go install`, Homebrew, Docker image, or prebuilt binaries for Linux/macOS/Windows. Zero runtime dependencies.

## Architecture Notes
Single Go binary implementing nine protocol handlers plus a stateful mock engine, an admin REST API, a web dashboard, and an embedded MCP server. Positions itself directly against WireMock/Mockoon/Prism/MockServer/Beeceptor/json-server with a detailed (self-authored) comparison matrix in the README.

## Maturity
Emerging. Created 2026-02-04, 147 stars. Solo-maintainer project — one contributor accounts for the overwhelming majority of commits. Latest tagged release (v0.7.1, 2026-06-21) trails the last `main` push (2026-09-17) by roughly three months.

## License
Apache-2.0, unrestricted. A managed SaaS tier is listed as roadmap/not-yet-live.

## Alternatives
WireMock (JVM, per-protocol extensions), Mockoon (Electron), Prism (HTTP-only), MockServer (JVM), Beeceptor (SaaS, rate-limited free tier), json-server (REST only).

## Risks / Limitations
Bus-factor risk from single-maintainer development. Newer functionality on `main` may be untagged/undocumented since releases lag pushes. The README's competitor-comparison table is marketing from the project itself — verify specific claims about WireMock/MockServer independently before relying on them.

## Recommendation
PROTOTYPE — strong single-binary design and a genuinely useful MCP angle; worth trying on a real integration before depending on it, given the solo-maintainer and release-lag risk.

## Change History
### 2026-09-28
Initial discovery and review. Slot 6 run; found via GitHub Search API `topic:chaos-engineering` query.
