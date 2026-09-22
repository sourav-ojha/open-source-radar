# agentOS (Rivet)

## Summary
agentOS is a JavaScript/TypeScript library that gives AI agents an "operating system" running inside your own backend process — V8 isolates for guest JS and WebAssembly for compiled tools — instead of spinning up a microVM, container, or third-party sandbox service per agent session. It ships as an npm package (`@rivet-dev/agentos`) built by Rivet, the actor-runtime company, and bundles ready-to-run adapters for Pi, Claude Code, Codex, and OpenCode.

## Why I Should Care
Every coding-agent or automation product that needs to let an LLM run untrusted code currently reaches for E2B, Daytona, Modal, or a self-managed Firecracker fleet — all separate network services with cold-start and per-session cost. agentOS claims 92x faster cold starts, 47x less memory, and 254x lower cost than sandbox providers in its published benchmarks, because the VM boots in-process rather than over the network. For a MERN/Node stack this is a library import, not new infrastructure to operate.

## Problems It Can Remove
- A dedicated sandbox microservice (E2B/Daytona-style) for any product feature that lets users or agents execute code.
- Per-session network round-trips and cold-start latency when spinning up ephemeral compute for an agent.
- Credential-leak risk from exposing host secrets to agent-run code — host functions keep credentials server-side and expose only inputs/outputs to the guest.

## Practical Uses
- Embedding a "run this agent-generated script safely" feature directly into a Node backend (e.g., a micro-SaaS that lets users automate a workflow via natural language).
- Building an internal coding-agent tool without paying for or operating a third-party sandbox.
- Letting an agent read/write files and exec POSIX utilities (coreutils, sed, grep, tar) inside a scoped VM as part of a pipeline.
- Prototyping agent-driven admin-panel actions with permission gating (filesystem/network/process/env) instead of hand-rolling an allowlist.

## Product Opportunities
Could underpin a "safe automation" feature in a micro-SaaS product — e.g., a billing/ops admin panel that lets an AI agent propose and execute a remediation script inside an in-process sandbox, with permissions and audit built in, without standing up separate sandbox infrastructure.

## Agent / Automation Opportunities
This *is* agent infrastructure: it is the execution substrate coding agents (Claude Code, Codex, OpenCode, Pi) run inside, and it exposes a client SDK for driving sessions programmatically — a natural building block for any custom agent orchestration layer.

## Integration
- **Installed locally / embedded as library**: `npm install @rivet-dev/agentos`, imported directly into a Node/TS backend.
- **Self-hosted**: yes, runs entirely in-process; no external service required.
- **Cloud-deployable**: optional managed deployment via Rivet Cloud, but self-hosting is the default path.
- **Interfaces**: TypeScript client/server SDK, host-function bridge, optional embedded mode via `@rivet-dev/agentos-core`.
- **Integration effort: Low** for a Node/TS backend — it's an npm install plus a small amount of setup code, no new infra to provision.

## Architecture Notes
Guest JavaScript runs in V8 isolates and compiled tools run as WebAssembly, all multiplexed inside one compact runtime process — this is the same in-process-isolation pattern Cloudflare Workers popularized, applied to agent sandboxing instead of HTTP handlers. Host functions are the only bridge between guest code and real capabilities, which is a clean way to keep credentials off the agent's attack surface. It composes with full sandboxes rather than replacing them: "sandbox mounting" spins up a real VM on demand and mounts its filesystem into the lightweight VM when a workload genuinely needs one (native binaries, browsers, dev servers).

## Maturity
Emerging. Created February 2024, actively developed (pushed same day as this review), 4,649 stars, 10 contributors, Apache-2.0. Versioned v0.2.21 (pre-1.0) — README explicitly states "agentOS is in preview and the API is subject to change." Backed by Rivet, an established infra company, not a solo weekend project.

## License
Apache-2.0 — no commercial-use restrictions.

## Alternatives
E2B, Daytona, Modal Sandboxes, Firecracker-based self-managed microVM fleets, Docker-per-session. All of these are full Linux environments reached over a network API; agentOS is explicitly narrower (no native binaries, no browsers) in exchange for running inside the existing backend process.

## Risks / Limitations
- Pre-1.0, API subject to change — not yet a long-term-stable dependency.
- Narrower capability than a full sandbox: no native binaries or browser automation without falling back to real sandbox mounting.
- Team/community still small relative to the sandbox incumbents it benchmarks against; the published benchmarks are self-reported (methodology is documented, but not independently verified here).

## Recommendation
PROTOTYPE — worth wiring into a small internal tool or a coding-agent side project to validate the cold-start/cost claims firsthand before depending on it for anything customer-facing.

## Change History
### 2026-09-22
Initial discovery and review. Rotation slot 7 (experimental/hidden gems), surfaced via GitHub search for "wasm sandbox agent."
