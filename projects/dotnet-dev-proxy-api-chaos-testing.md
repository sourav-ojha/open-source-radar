# Dev Proxy

## Summary
A command-line API simulator from Microsoft that intercepts your app's outbound HTTP traffic and injects failures, throttling, and slow responses on demand — without changing a single line of application code. Works with any language or framework since it operates as a local proxy rather than a library.

## Why I Should Care
Testing how a React/Node app actually behaves when a third-party API (payments, auth, an AWS service, an external SaaS integration) fails, rate-limits, or goes slow is usually skipped under deadline pressure, because simulating it properly means writing throwaway mock code. Dev Proxy makes that testing config-driven instead: point the app at the proxy, define failure/throttle rules, and see what actually breaks — directly relevant to hardening any production integration, and to the MSSP/PTaaS context where demonstrating resilience under degraded conditions matters.

## Problems It Can Remove
Replaces hand-written mock servers or ad hoc code branches used "just for testing the failure path," and makes the resulting tests real integration tests against the simulated failure rather than a unit test against a stub.

## Practical Uses
- Simulate a payment provider or AWS service returning 429/500 responses to verify retry/backoff logic actually works.
- Throttle or slow down an API during local development to catch UI states (loading spinners, timeouts) that only show up under real-world latency.
- Record real API traffic and replay it with injected faults in CI, without needing network access to the real service.
- Use in a PTaaS/security-testing context to probe how a client's app degrades under API failure conditions.

## Product Opportunities
None directly — this is a testing/dev tool, not an embeddable product component.

## Agent / Automation Opportunities
No MCP server; it's a CLI/config-driven proxy. Could be scripted into a CI pipeline or an agent-driven resilience-test harness, but there's no native agent integration today.

## Integration
Install via npm, winget, Homebrew, or a standalone binary; cross-platform. Runs as a local reverse proxy — no application code changes required, just pointing outbound requests through it (typically via `HTTPS_PROXY` or a certificate trust step). Integration effort: **Low** — works with any stack since it operates at the network layer.

## Architecture Notes
Config-driven fault injection at the proxy layer rather than at the code layer — the same idea as Toxiproxy or WireMock's fault-injection features, but packaged as a turnkey CLI with no server to stand up and a polished recording/replay workflow.

## Maturity
Mature. Created 2022-09-09 (3+ years), actively maintained by Microsoft (pushed 2026-11-24), latest release v4.0.0 (2026-10-29). 849 stars / 91 forks / 11 subscribers / 108 open issues — healthy, organic-looking engagement for a corporate-backed tool.

## License
MIT. No commercial-use restrictions. Built by Microsoft but standalone and free; no hosted-service dependency.

## Alternatives
Toxiproxy (Shopify, lower-level TCP fault injection, no HTTP-aware failure presets); WireMock (more focused on full mocking than fault injection); hand-rolled proxy scripts. Dev Proxy's distinguishing feature is purpose-built HTTP-aware failure/throttle simulation with zero code changes and a polished CLI/recording workflow.

## Risks / Limitations
- Built in .NET, though distributed as cross-platform binaries/npm package — no .NET knowledge needed to use it, but worth knowing for anyone wanting to contribute or extend it.
- Primarily a Microsoft-ecosystem-adjacent tool (originally built for Graph API testing); general-purpose use is supported but less documented than the Microsoft-specific scenarios.

## Recommendation
PROTOTYPE — worth testing against one real third-party API integration (payments, AWS service call, or similar) to see how the app actually handles induced failures before adopting it as a standard part of the test suite.

## Change History
### 2026-11-24
First seen and reviewed. Discovered via GitHub search for API-mocking topic tag. Verified license, version, and maintenance activity via the GitHub API.
