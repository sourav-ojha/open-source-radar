# Graft (getmodern-ai)

## Summary
Graft is an MCP server that synthesizes a missing API integration on demand. When an agent needs to call a vendor's API and no tool exists for it, Graft's own coding model reads the vendor's documentation, writes and statically checks a small module, proves it with read calls, dry-runs writes behind a network-pinned proxy that injects the credential (never exposed to the model or sandbox), then promotes the result into the calling agent's MCP tool list.

**Note:** this is unrelated to the already-catalogued project also named "Graft" (`trailhq/Graft`, slug `graft-codebase-context`), which is a codebase-context tool for coding agents. Two distinct projects, from distinct organizations, that happen to share a name.

## Why I Should Care
The hard part of "let an agent call any API" isn't writing the HTTP request — it's doing so safely at agent runtime: sandboxing the generated code's network egress, keeping the credential out of the model and sandbox entirely, and gating any write behind a dry run a human approves. Graft ships that safety machinery already built, with an eval harness that scores a real run against exactly those properties (reads happen before publish, no write reaches the vendor before dry-run approval, the credential is never recorded in traces).

## Problems It Can Remove
- Removes the need to hand-write bespoke integration code every time an agent needs to reach a long-tail vendor API with no existing MCP tool.
- Removes the recurring engineering cost of maintaining a growing static list of MCP-server integrations for an agent fleet.

## Practical Uses
- Give an ops/coding agent a safe path to acquire a new third-party API integration at runtime instead of filing a ticket for someone to build it.
- Study its sandbox/proxy design as a reference architecture for any feature where an agent writes and runs its own code against a real external system.

## Product Opportunities
The architecture pattern — sandboxed, on-demand tool synthesis behind a credential-injecting proxy — is directly reusable for an internal agent-orchestration product that needs to onboard new integrations without dedicated engineering time per vendor.

## Agent / Automation Opportunities
Core meta-tools exposed over MCP: `acquire`, `acquire_status`, `find_tool`, `promote`, `demote`, `run_tool`, `request_connection`, `request_credential`. OAuth-based MCP connection for Claude.ai/ChatGPT; static bearer tokens for Hermes/OpenClaw, including a bundled Hermes skill that tells the agent when to reach for these tools.

## Integration
Self-hosted via Docker Compose (server, proxy, MCP endpoint, console, sandbox images), requiring Postgres and a Docker daemon. Published images: `ghcr.io/getmodern-ai/graft` and `ghcr.io/getmodern-ai/graft-sandbox`. Integration effort: **Medium** — it's a small self-hosted service stack, not a single binary, and the project itself flags that the server mounts the Docker socket (root-equivalent on the host) as a real operational consideration.

## Architecture Notes
Egress from the sandbox is locked to the proxy only; a published conformance suite (`packages/sandbox/src/conformance.ts`) asserts that requests to arbitrary hosts fail and that a detached process is no freer. The credential is injected by the proxy at the network boundary and never passed into the generated module, the sandbox, or the model itself. A generated module must pass a static check (typed against the input schema, banned surface, no literal foreign host, SDK calls bound to the proxy) before it's allowed to run at all, and a failed attempt is retried under a token ceiling rather than left broken.

## Maturity
Experimental — v0.1.0 (released 2026-09-22), created September 2026, 4 stars, single maintainer. The project's own release notes are unusually candid about its limits: "No third-party security audit has been done. One maintainer, a young codebase, written with heavy coding-agent assistance."

## License
Apache-2.0 for the core; MIT for the skill and harness plugins. The hosted multi-tenant console/backing is explicitly a private package not included in the repository.

## Alternatives
Static MCP server catalogs/marketplaces (today's dominant pattern, but require someone to have already built the integration), Composio and similar hosted integration platforms (Graft's differentiator is synthesizing the tool itself at call time rather than exposing a pre-built catalog).

## Risks / Limitations
- v0.1.0, single maintainer, and the project's own README states no third-party security audit has been done.
- Self-hosting mounts the Docker socket — the project itself calls this root-equivalent on the host; accept that blast radius knowingly.
- Generated code paths were built "with heavy coding-agent assistance" per its own documentation — worth extra scrutiny before trusting it with real credentials.
- Very new and essentially unproven outside its own author's usage (4 stars).

## Recommendation
STUDY — the sandboxed, credential-injecting, dry-run-gated architecture for agent-synthesized tools is a genuinely useful pattern worth understanding even without adopting the project directly; the security posture (single maintainer, no audit, Docker-socket mount) argues against production use at this stage.

## Change History
### 2026-10-10
Discovered via GitHub search (slot 4: data, search, documents, RAG). First catalog entry.
