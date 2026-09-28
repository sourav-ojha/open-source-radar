# AgentSight

## Summary
A local-first, eBPF-based observability tool for AI coding agents — a `top`/`strace` for agents. It attaches to the kernel and to TLS functions to observe prompts, model calls, tool decisions, file activity, and network activity for any running agent (Claude Code, Codex, Gemini CLI, OpenCode, OpenClaw, or an arbitrary command), with no SDK, proxy, or vendor integration required.

## Why I Should Care
Every application-level agent observability tool (LangSmith, Langfuse, Phoenix) requires owning the agent's source code, and gateway tools (Helicone) require routing its LLM calls through a proxy. AgentSight instead watches the real OS-level effects of a closed-source CLI agent — which is the only approach that works when the agent binary itself is opaque.

## Problems It Can Remove
Manually correlating an agent's self-reported logs with what it actually did on disk/network when a run fails, stalls, or behaves unexpectedly. Also removes the need to instrument each agent tool individually to get basic activity visibility.

## Practical Uses
- Watching what a coding agent actually touched on disk/network during a run, independent of its own log output.
- Diagnosing stalls, retry loops, or runaway token/tool-call spend by connecting prompts to real system effects.
- Auditing agent-initiated network calls and file writes for security review before trusting an agent with more autonomy.
- Flamegraph-style profiling (`agentpprof`) of where an agent session's time and resources actually went.

## Product Opportunities
A lightweight, always-on "agent activity monitor" layer for an internal coding-agent fleet, without instrumenting each agent/SDK individually.

## Agent / Automation Opportunities
Purpose-built as an observability layer *for* agent tools, not itself an agent — pairs naturally with any coding-agent workflow already in use (Claude Code included).

## Integration
`cargo install agentsight` or a prebuilt binary. Linux-only (eBPF + TLS-function hooking), runs as a host-level process. No SDK or code changes to the observed agent required.

## Architecture Notes
Attaches eBPF probes to TLS library functions to trace encrypted traffic in plaintext at the point of encryption/decryption, combined with kernel-level syscall tracing (file, process, network), then correlates both back to agent sessions. Backed by a published paper (arXiv:2508.02736, also published at an ACM venue, DOI:10.1145/3766882.3767169).

## Maturity
Emerging. Created 2025-07-07, 710 stars, 106 forks, latest release v1.0.31 (2026-09-05). Active development, but commit history mixes a real maintainer (eunomia-bpf team, also behind other eBPF tooling) with a large volume of CI-bot commits.

## License
MIT — no restrictions on commercial or embedded use.

## Alternatives
LangSmith, Langfuse, Arize Phoenix (application-level, require owning the app code); Helicone (gateway/proxy-level, requires routing provider traffic through it).

## Risks / Limitations
Linux-only, requires eBPF support and elevated privileges on the host — not usable for agents run on a Mac dev machine or inside restricted CI runners. eBPF probes attached to TLS functions are inherently kernel-version-sensitive; verify compatibility before relying on it beyond local debugging.

## Recommendation
PROTOTYPE — genuinely differentiated approach to a real problem (observing closed-source coding agents), but young enough and Linux-eBPF-dependent enough to test in a real workflow before leaning on it.

## Change History
### 2026-09-28
Initial discovery and review. Slot 6 (infrastructure, observability, deployment) run; found via GitHub Search API `topic:ebpf+observability` query.
