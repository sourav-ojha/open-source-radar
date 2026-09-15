# AgentVM (DeepClause)

## Summary
A lightweight Node.js library that boots a WASM-compiled Alpine Linux VM inside a worker thread, giving AI agents an isolated environment to run shell commands without Docker, a real VM, or a subprocess sandbox. Built on `container2wasm`. Supports host-directory mounts, a persistent per-workspace root filesystem overlay, network on/off toggling, port forwarding, and outbound firewall rules — all runtime-configurable. The maintainers are explicit that it's experimental and not production-ready.

## Why I Should Care
This is the kind of narrow, honestly-scoped hidden gem the "experimental/hidden gems" rotation slot exists to surface. Every agent-tool builder eventually needs an answer to "how do I let an LLM run shell commands without risking the host" — the usual answers are Docker-in-Docker, a cloud sandbox API, or a real VM, all of which carry real infrastructure weight. A pure-npm-install, WASM-based answer that runs in-process is a genuinely different point in the design space, useful to know about even if not adopted immediately.

## Problems It Can Remove
- Standing up Docker (or a Firecracker/gVisor VM) purely to give one agent process a disposable shell sandbox.
- Building a network allow/deny layer around an agent's shell access from scratch.

## Practical Uses
- Embed as the sandboxed-execution layer inside a custom coding-agent tool where Docker isn't available or desired (e.g., inside an Electron app, as `deepclause/pi-box` does).
- A cheap, ephemeral shell sandbox for a serverless/Lambda-style agent tool-call handler where spinning up a container per invocation is too slow or unavailable.
- A local testbed for experimenting with agent tool-calling against a disposable filesystem before wiring up something more production-grade.

## Product Opportunities
- Could be the isolation primitive inside a low-cost, high-density coding-agent-as-a-service product where per-user Docker containers would be too resource-heavy.

## Agent / Automation Opportunities
- This is itself an agent-sandboxing building block — a natural fit as the execution backend behind an MCP "run shell command" tool where full container isolation is overkill.

## Integration
`npm install deepclause-agentvm`; instantiate `new AgentVM()`, `.start()`, `.exec()`. Effort: **Low** to try, but the maintainers' own security disclaimer (relies on `node:wasi`, "known to have some quirks and possibly security flaws") means it needs real evaluation before trusting it as an actual security boundary.

## Architecture Notes
Uses `container2wasm` to compile a full Alpine Linux environment to WASM, run in a Node worker thread via `node:wasi`. Notably, the README states the entire networking stack and host-mount support was built using coding agents/models — an interesting (if unverified) data point on what's achievable that way.

## Maturity
Experimental — the project's own README calls it "highly experimental," explicitly "not recommended for production use," with a disclosed possibility of WASI-related security flaws. 79 stars, active development (pushed same day as this review).

## License
MIT — no commercial-use restrictions.

## Alternatives
Docker-based agent sandboxes, E2B (hosted), Firecracker microVMs, gVisor. All of those are heavier-weight (require a container runtime or a hosted API) compared to AgentVM's pure-library, in-process approach — the tradeoff is AgentVM's honestly-disclosed immaturity as an actual security boundary.

## Risks / Limitations
- Explicitly not a hardened security boundary yet — do not use this to sandbox genuinely untrusted code today.
- API may change without notice per the maintainers' own disclaimer.
- Narrow scope (Alpine Linux VM only, no Python/Node-specific base images natively — check current npm package variants before assuming feature coverage).

## Recommendation
WATCH — the architecture is worth knowing about and worth revisiting once the WASI security concerns are addressed or independently reviewed; not yet suitable to depend on for actual isolation.

## Change History
### 2026-09-15
Initial discovery and review.
