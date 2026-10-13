# nono

## Summary
Zero-setup, zero-latency sandbox for running coding agents (Claude Code, Codex, Cursor-style CLIs, OpenCode, Hermes, Copilot, OpenClaw) with least-privilege filesystem, network, and credential policies. No daemon, container, or VM — runs as a process-level sandbox on macOS, Linux, and Windows (WSL2). Delegated tools (git, gh, curl, kubectl, MCP clients) get their own nested command sandboxes with an L7 credential proxy, so a secret given to an agent session is not automatically available to every tool it shells out to. Ships a public registry of per-agent policy profiles and FFI bindings for Rust, Python, TypeScript, and Go.

## Why I Should Care
I run Claude Code and similar agents against real repos with real credentials daily. nono scopes exactly what an agent session (and each tool it calls) can touch, instead of trusting the agent's own judgment or a blanket allow-list.

## Problems It Can Remove
A correct least-privilege sandbox with filesystem/network/credential scoping, nested per-tool policies, and an L7 credential proxy is a multi-week security-engineering project with easy-to-miss escape paths; this one is already OpenSSF-reviewed and cross-platform.

## Practical Uses
- Run Claude Code or Codex against a real repo with filesystem/network access scoped to exactly what the task needs
- Give an agent a GitHub token through a credential proxy that only allows specific API methods/paths, instead of an ambient env var any tool can read
- Nest a separate, tighter sandbox around delegated tools (git, gh, ssh, package managers) so the agent's broad session grants don't inherit down to them
- Pull a reviewed, versioned policy profile from the community registry instead of hand-writing a sandbox policy per agent
- CI step that signs/verifies agent-authored commits (agent-sign GitHub Action) for supply-chain accountability

## Product Opportunities
- Could underpin a client-facing "show me what the agent could touch" governance feature for the MSSP admin-portal SaaS
- Usable as the sandboxing layer inside an internal multi-agent orchestration product rather than building process isolation from scratch

## Agent / Automation Opportunities
- Purpose-built for wrapping coding-agent CLIs directly (first-class profiles for Claude Code, Codex, Hermes, OpenCode, Copilot, OpenClaw)
- FFI bindings mean the policy engine can be embedded inside a custom agent orchestrator rather than shelled out to
- Registry (registry.nono.sh) gives reusable, shareable policy profiles per agent/tool combination

## Integration
Deployment: CLI binary via curl install, Homebrew, or Nix; library via Rust/Python/TypeScript/Go FFI bindings; GitHub Action (agent-sign).
Interfaces: CLI, library bindings (4 languages), GitHub Action.
Integration effort: **Low** — single install command, policy profiles pulled from a registry rather than authored from scratch.

## Architecture Notes
Process-level sandboxing (no container/VM) with a composable JSON policy format. The distinguishing design choice is nested command sandboxes: when an agent calls a delegated tool (`git`, `gh`, `curl`, `kubectl`, MCP clients), nono's broker launches that tool under its own command policy with its own filesystem/network/credential scope — the tool does not inherit the parent session's broad `--allow` grants or raw credential paths unless the tool's own policy says so. Credentials can be injected via a proxy that filters by HTTP method and path (L7 filtering) rather than handing over a raw token.

## Maturity
Emerging, pre-1.0 (README: "APIs are stabilizing... may still occur"). GitHub API verified: Apache-2.0, 4,467 stars, 302 forks, pushed_at 2026-10-13, created 2026-01-31. Built by maintainers associated with Sigstore (the supply-chain attestation standard used by PyPI/npm/Homebrew/Maven Central); carries an OpenSSF Best Practices badge and a working CI pipeline. Named production-use testimonials from Datadog and Okta security engineers appear in the README but are maintainer-supplied and not independently verifiable.

## License
Apache-2.0, confirmed via GitHub API license field. No restrictions on commercial use, redistribution, or embedding.

## Alternatives
- AgentVM / DeepClause (catalogued, WATCH, 6.1) — WASM-based, heavier and less mature
- AgentKernel (catalogued, PROTOTYPE, 7.6) — microVM-based, stronger isolation guarantee but real VM overhead vs nono's zero-setup process sandbox
- Pikopod (catalogued, PROTOTYPE, 7.8) — solves a different problem (simulating third-party API failures), not a general agent sandbox

## Risks / Limitations
- Pre-1.0: README states APIs are still stabilizing and may change before a 1.0 release
- Datadog/Okta production-use quotes are maintainer-supplied marketing copy and cannot be independently verified from the repo alone
- The community registry of shared agent policy profiles is a new supply-chain trust surface if profiles are pulled from unreviewed third parties rather than authored locally

## Recommendation
**USE NOW** (score 8.6/10) — low-risk, low-setup install; the main caveat is pre-1.0 API churn, not the security model itself.

## Change History
### 2026-10-13
First discovered and reviewed. Verified via GitHub API: Apache-2.0, 4,467 stars, pushed_at 2026-10-13, created 2026-01-31, latest release v0.79.0 (2026-09-30). Surfaced in a GitHub search for AI-agent sandboxing tools alongside dozens of low-traction same-shape repos; selected for review on the strength of its Sigstore-team lineage, OpenSSF badge, and nested per-tool sandboxing design.
