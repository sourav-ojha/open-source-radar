# AgentKernel

## Summary
A CLI/HTTP service that runs AI coding-agent commands inside real Firecracker microVMs
— the same virtualization technology behind AWS Lambda — on Linux/KVM hosts, with
automatic fallback to Docker, Podman, or Apple Containers where KVM isn't available.
Includes a network-layer secret-injection proxy so credentials never enter the sandboxed
VM at all.

## Why I Should Care
Most "sandboxed" agent execution setups today are just a Docker container (shared
kernel). AgentKernel provides actual hardware-level isolation via Firecracker on the
exact tech AWS uses for Lambda, which is directly legible given an AWS-heavy background,
plus a genuinely different secret-handling model worth studying regardless of adoption.

## Problems It Can Remove
- Letting an agent run arbitrary shell/build/test commands with shared-kernel-only
  isolation (plain Docker) when real isolation is warranted.
- Injecting real API keys/secrets into an agent's environment or mounted files, where a
  compromised or misbehaving agent process could read or exfiltrate them.
- Per-language sandbox setup boilerplate — image selection, Procfile parsing, etc.

## Practical Uses
- `agentkernel run npm test` / `cargo build` / `pytest` — auto-detects the right runtime
  image, creates a temporary sandbox, runs, cleans up.
- `agentkernel sandbox create --branch` for a persistent, per-git-branch sandbox during
  parallel agent work.
- Warm-pool daemon mode (`agentkernel serve`) for ~195ms sandbox starts on Linux vs.
  ~800ms cold Firecracker boot, when an agent needs many short-lived executions.
- Network-layer secret injection ("Gondolin pattern"): a host-side proxy intercepts
  outbound HTTPS and injects real credentials scoped to specific domains, while the
  sandbox itself only ever sees a placeholder env var.
- Signed execution receipts (`--receipt`, `receipt verify`, `receipt replay`) for an
  auditable record of what an agent actually ran.

## Product Opportunities
The credential-injection-at-the-network-layer pattern is directly reusable for any
internal agent-sandboxing, CI job-runner, or PTaaS-adjacent execution service where
"the executor should never actually hold the secret" is a real requirement.

## Agent / Automation Opportunities
Positioned explicitly as the execution layer under a coding agent — any agent CLI that
shells out to run commands can be pointed at `agentkernel run` instead of the host shell.
MCP support exists for Firecracker lifecycle/file/exec delegation via the server's local
control endpoint.

## Integration
Homebrew, an install script, or Cargo, followed by `agentkernel setup` to fetch/build
Firecracker components. HTTP API mode supports API-key auth and a private Unix-domain
socket for the local control endpoint. Integration effort: **Medium** — real isolation
requires a Linux/KVM host; macOS/non-KVM environments get a materially different
(container-based) isolation guarantee via automatic fallback.

## Architecture Notes
Uses Firecracker microVMs (via KVM) as the primary isolation mechanism on Linux, with a
warm pool of 3-5 pre-booted VMs maintained by a daemon for fast repeated execution.
Falls back to Podman/Docker on non-KVM Linux and Apple Containers on macOS 26+, with each
fallback's isolation level clearly documented rather than glossed over. Also supports
Hyperlight (Microsoft's hypervisor-isolated WASM microVMs) as a faster (~68ms), more
restrictive alternative on KVM Linux.

## Maturity
Created 2026-01-19, 59 stars, 2 contributors, current release v0.20.1 (2026-08-23)
against an actively pushed main branch. Small team relative to the depth of the feature
set (security profiles, SSH-into-sandbox, HTTP API, signed receipts).

## License
MIT. No commercial restrictions.

## Alternatives
- AgentVM / DeepClause (already catalogued, WATCH) — WASM-based sandbox in a Node worker
  thread; no root/KVM needed but weaker isolation guarantees than a real microVM.
- Plain Docker/Podman — shared-kernel isolation only, what most agent setups default to.
- E2B and similar hosted sandbox-as-a-service products — not self-hosted, ongoing cost.

## Risks / Limitations
- Small contributor base (2) relative to the security-sensitive surface area (secret
  injection proxy, VM lifecycle management) — less battle-tested than the underlying
  Firecracker/KVM technology it wraps.
- The actual isolation guarantee depends heavily on platform: full Firecracker isolation
  is Linux/KVM-only, macOS gets a different (container-based) model.
- Latest tagged release is roughly three weeks stale against main at time of review —
  check for drift before pinning a version.

## Recommendation
**PROTOTYPE** — Worth testing on a Linux/KVM box (or a cloud VM) specifically to
evaluate the Firecracker isolation and the Gondolin secret-injection pattern for
sandboxing an agent's shell access, before considering it for anything beyond
experimentation.

## Change History
### 2026-09-16
Initial discovery and review. Rotation slot 1 (AI agents, MCP, coding productivity).
