# GuardianWAF

## Summary
A zero-dependency Web Application Firewall written in pure Go, shipped as a single binary. Runs as a standalone reverse proxy, a Go library middleware, or a Docker/Kubernetes sidecar with label-based auto-discovery. Uses a tokenizer that decomposes requests into typed tokens and assigns cumulative threat scores — rather than binary allow/block regex rules — to produce graduated responses: allow, log, proof-of-work challenge, or block.

## Why I Should Care
Directly relevant to the MSSP security admin-portal SaaS and PTaaS partnership context: a self-hostable, single-binary WAF with SIEM export and an MCP server for agent-driven analysis is something that could sit in front of client-facing apps without adopting a CDN vendor's WAF subscription or building/maintaining an nginx+ModSecurity+OWASP-CRS stack.

## Problems It Can Remove
Removes the operational overhead of compiling/maintaining ModSecurity modules, or the recurring cost of a hosted WAF (Cloudflare/AWS), for a self-hosted deployment. The library middleware mode also removes the need for a separate reverse-proxy hop entirely for a Go backend.

## Practical Uses
- Front a client's web app with WAF protection for an MSSP engagement without a nginx/ModSecurity build.
- Embed WAF middleware directly into an existing Go HTTP service for inline protection.
- Export WAF events as CEF/syslog into a client's SIEM for PTaaS/MSSP compliance reporting.
- Auto-protect Docker Compose services via `gwaf.*` label-based discovery with no per-service config.

## Product Opportunities
A bundled, white-labelable WAF layer offered as part of an MSSP security admin-portal SaaS for clients wanting app-layer protection without buying a separate CDN-vendor WAF.

## Agent / Automation Opportunities
MCP server with 44 tools for agent-driven configuration, querying, and threat analysis — notable scope for a security tool's agent surface.

## Integration
Low effort. Single binary for standalone proxy mode; Go library import for middleware mode; Docker/K8s sidecar for containerized environments. No module compilation or external dependency chain required in any mode.

## Architecture Notes
The tokenizer-plus-cumulative-score detection model is a more nuanced middle ground between brittle regex-based WAF rules and a full ML detection pipeline — worth studying as a pattern even independent of this specific tool. The three-deployment-mode design (proxy/library/sidecar) from one codebase is also a reusable idea for any security tool aiming to minimize adoption friction.

## Maturity
Emerging. 35 stars, 9 forks, 0 subscribers, 20 open issues at review time; created 2026-03-17. Has CI, 79% test coverage, cross-browser E2E tests (Chromium/Firefox/WebKit), and extensive documentation (production deployment, Kubernetes/Helm, metrics, runtime reload). Single named maintainer (Ersin Koc).

## License
Core is MIT. Cluster mode and commercial support are explicitly described as planned future enterprise features under a separate, not-yet-released commercial license — not a blocker today, but worth tracking if clustering is ever needed.

## Alternatives
- ModSecurity + OWASP CRS — mainstream, heavier operational footprint (module build, separate proxy).
- Cloudflare WAF / AWS WAF — hosted, recurring cost, less control over detection logic.
- Fail2ban-style log scraping — reactive, not inline request inspection.

## Risks / Limitations
- Single maintainer — bus-factor risk for a security-critical dependency.
- The "<1ms p99" latency figure is labeled a target in one place and a measured table elsewhere; no inline benchmark methodology is given.
- The competitor comparison table is self-authored and dated March 2026 — treat as marketing, not independent verification.
- WebSocket binary frames are size-checked only, not content-inspected.
- Detection-quality figures (false-positive/false-negative rates) are referenced in a separate document, not shown inline.

## Recommendation
PROTOTYPE — worth trialing in front of a non-critical service to validate the detection quality and false-positive rate directly before trusting it for a client-facing MSSP deployment.

## Change History
### 2026-10-27
First reviewed. Surfaced via a "single binary zero dependency" GitHub search during slot 7 (experimental/hidden gems).
