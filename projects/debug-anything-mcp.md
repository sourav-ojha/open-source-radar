# Debug Anything

## Summary
An MCP server and CLI that gives a coding agent (or a human) black-box observation of any running process, container, or pod — syscalls, CPU flamegraphs, thread stacks, sockets/fds, packets, heap retainers, container state — by wrapping battle-tested local tools (`strace`, `py-spy`, `perf`, `lsof`, `tcpdump`, `gdb`/`lldb`, Node's inspector) and turning their raw output into structured findings that must cite evidence, with no instrumentation or restart required.

## Why I Should Care
An agent can read every line of code and still have no way to answer "why is this process eating 4GB" or "what's holding port 8080" — it doesn't know the flags for `strace`, and a raw 10-second trace is 1.5MB of text that blows the context window. This gives the agent eyes on the *running* system, not just the source, which is the layer most agent debugging tooling skips entirely.

## Problems It Can Remove
- Manually running and interpreting `strace`/`lsof`/`py-spy`/`gdb` output when debugging a hung, leaking, or slow Node/Python process.
- The "who's holding this port" / "why did this container get OOMKilled" investigations that otherwise mean switching tools and reading raw output by hand.
- Blowing an agent's context window by pasting raw profiler/trace output instead of a cited, ranked summary.

## Practical Uses
- `npx debuganything why <pid> --symptom leak` on a Node service suspected of leaking memory — gets RSS/heap diffs and the top retainer with its retaining path.
- Ask an agent "something is holding port 8080, who?" and it calls the tool directly via MCP instead of you running `lsof` by hand.
- Diagnose a Docker container OOMKill or restart loop (`container_info`) without SSHing in to run `docker inspect` manually.
- CI hang forensics via the companion GitHub Action when a pipeline job hangs instead of failing cleanly.
- Capture a thread-dump deadlock (two Python threads taking locks in opposite order) and get the exact blocking lines named, not just "it's stuck."

## Product Opportunities
Not a product-building block directly, but the "evidence contract" pattern (a finding is rejected by the schema unless it cites evidence) is a reusable idea for any internal agent-tooling that needs to be audit-safe rather than just plausible-sounding.

## Agent / Automation Opportunities
This *is* agent tooling: one MCP server with 14 tools, auto-setup for Claude Code/Codex/Cursor/Gemini CLI, a Claude Code plugin, a proactive hook that nudges the agent toward it when it sees `EADDRINUSE`/`ECONNREFUSED`/OOM/segfault in command output, and Python bindings for LangChain/LlamaIndex/CrewAI agents.

## Integration
`npx debuganything setup` auto-detects the agent and writes config; `debuganything init` commits a shared `.mcp.json` for a whole team. Requires Node ≥22 for the CLI itself; individual observation tools (`strace`, `perf`, `py-spy`, `tcpdump`) are optional and the `readiness` command reports what's usable on the current machine before you need them. No server, no account, no telemetry — read-only by default, with `capture_traffic` gated behind explicit consent.

## Architecture Notes
Every tool result is a `ToolResult` with `findings[]` that must cite `evidence_ids[]` pointing at saved raw artifacts — a finding without an evidence id is rejected by the schema, which is the detail that separates this from an agent just summarizing tool output in prose. Adapters declare the platforms and binaries they need; `readiness` computes what will actually work on this machine before a tool is invoked, rather than failing mid-run. Explicitly positions itself as complementary to step-through MCP debuggers (already-catalogued `mcp-debugger`, Microsoft's `DebugMCP`): those need a debug port and a known breakpoint location; this is the black-box layer for when you don't know where to look yet, and its own `suggested_next` field hands off to a step debugger once it has localized a problem to a function.

## Maturity
Emerging: created 2026-10-17, 13 stars, 3 forks, actively pushed as recently as the day of this review. Despite the low star count, the engineering signals (evidence contract, honest per-OS capability matrix, hard resource caps — 60s/tool, 50MB on disk, 16KB returned to the agent — secret redaction, a Discord community, a documented roadmap) are unusually mature for a 2-week-old repo.

## License
MIT, verified via GitHub API. No commercial-use restrictions.

## Alternatives
`debugmcp/mcp-debugger` (already catalogued, USE NOW) and Microsoft's `DebugMCP` cover step-through debugging over MCP — a different, complementary problem (you need to already know where to put the breakpoint). No other tool found doing black-box, zero-instrumentation process/container observation specifically packaged for agent consumption with an evidence-citation requirement.

## Risks / Limitations
Very new (2 weeks old) with a small contributor base so far — the Discord/roadmap infrastructure suggests ambition beyond a weekend project, but there's no track record yet. Several observation tools need root or elevated ptrace permissions on Linux (`strace`, `perf`, `py-spy`); macOS coverage is narrower (no `strace`/`perf` equivalents) and Windows isn't supported yet. Raw artifacts on disk are explicitly *not* redacted, so session directories need to be treated as sensitive.

## Recommendation
PROTOTYPE — worth installing on a real Node/Python debugging session the next time something hangs, leaks, or won't release a port; too new for USE NOW given the two-week track record, but the architecture is sound enough to bet a few minutes on.

## Change History
### 2026-10-30
Initial discovery and review.
