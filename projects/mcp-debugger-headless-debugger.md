# mcp-debugger

## Summary
A headless MCP server that gives AI coding agents real step-through debugging — breakpoints, stepping, variable inspection, expression evaluation — over the Debug Adapter Protocol across eight languages (Python, JS/TS, Ruby, Rust, Go, Java, .NET, C/C++). Unlike IDE-bound alternatives, it runs anywhere Node.js runs: CI runners, Docker, Kubernetes pods, SSH boxes, cloud agent sandboxes.

## Why I Should Care
Debugging with a coding agent today usually means the agent reads logs or adds print statements and guesses. This gives it the same tool a human reaches for — an actual debugger — and does it in the places an AWS/EKS-heavy backend stack actually runs (containers, pods, CI), not just inside a local VS Code window.

## Problems It Can Remove
Replaces the print-statement-driven debugging loop agents fall into by default, and removes the need to reproduce a bug locally just so a human (or another tool) can attach a real debugger.

## Practical Uses
- Attach to a misbehaving pod in EKS via `kubectl debug` + port-forward and hand the live session to an agent.
- Auto-debug a failing CI test with the provided composite GitHub Action (launches mcp-debugger + an agent on failure, posts root-cause analysis).
- Set a logpoint on a hot path in a live service to stream interpolated values without pausing execution.
- Debug a Node/Python process running inside a plain Docker container with no IDE involved at all.

## Product Opportunities
None directly — this is an internal engineering-productivity tool, not a product surface.

## Agent / Automation Opportunities
Purpose-built as an MCP tool surface: `start_debugging`, `add_breakpoint`, `step_over`, `evaluate_expression`, etc., with secret redaction on by default for variable/output values. Ships a companion Agent Skill documenting bisection-based root-cause debugging discipline rather than just raw tool access.

## Integration
`npx @debugmcp/mcp-debugger` or Docker image; STDIO or Streamable HTTP transport for any MCP client. Each target language needs its own toolchain installed for full functionality (debugpy, rdbg, Delve, JDK 21+, .NET SDK, Rust, a C/C++ compiler) — `mcp-debugger doctor` reports what's missing. Low integration effort overall.

## Architecture Notes
Adapter pattern over the Debug Adapter Protocol per language. Content/symbol-addressed breakpoints (`statement:`, `function:`) survive file edits and re-resolve across restarts. A read-only IDE mirror (`expose_session`) lets a human's IDE observe the agent's live session via a loopback, token-gated DAP endpoint without taking control.

## Maturity
Emerging — pre-1.0 (v0.24.2), created June 2025, actively released (CI, codecov, OpenSSF Scorecard + Best Practices badges). Maintained primarily by Sycamore LLC plus contributors.

## License
MIT. No commercial-use restrictions identified.

## Alternatives
**microsoft/DebugMCP** (510 stars, MIT) — same idea, but requires a running VS Code instance; can't attach to CI/K8s/remote processes the way mcp-debugger can. Choose it only if the agent must share a human's live VS Code session. mcp-debugger's own README includes a detailed head-to-head comparison table that holds up against independently pulled metadata.

## Risks / Limitations
- Pre-1.0 — tool surface can still shift between releases.
- Per-language toolchains required for full coverage.
- Single visible maintaining org; not yet a large community.

## Recommendation
USE NOW — low-effort to try (`npx`), directly useful for anyone debugging Node/Python/Go services in containers or Kubernetes with a coding agent in the loop.

## Change History
### 2026-09-18
Initial discovery and review. Rotation slot 3 (developer utilities, debugging, testing).
