# fx (Vercel Labs)

## Summary
A tiny (~7.8MB), embeddable coding-agent harness written in Zig, styled as a Unix-like CLI rather than a heavyweight terminal IDE. Cold-starts in ~10µs, supports MCP/skills/subagents, ships native and WASM builds, and speaks the Agent Client Protocol (ACP) for editor integration.

## Why I Should Care
Most coding-agent CLIs are built as end-user TUI products; fx is explicitly designed to be embedded into other systems (WASM SDK, ACP, tiny binary), which is the more useful shape if the goal is building agent capability into a product rather than using a terminal app.

## Problems It Can Remove
A minimal, permission-controlled, multi-provider agent harness with MCP/ACP support is a multi-week build even before session management and sandboxing are considered.

## Practical Uses
- Scriptable, pipeable coding-agent invocations from shell/CI without a heavy TUI
- Embed a coding-agent core into a custom internal tool via the WASM SDK
- Prototype an agent-orchestration layer using fx as the lightweight execution engine

## Product Opportunities
- Embeddable core for a bespoke internal dev-automation tool without adopting a full agent framework
- Could underlie a micro-SaaS 'agent-in-a-box' offering given its small footprint

## Agent / Automation Opportunities
- MCP server/client support and subagent model make it directly composable into a larger agent-orchestration setup

## Integration
Deployment: CLI binary, WASM module, library (embed in own agent infra)
Interfaces: CLI (fx, fx ask), MCP client, ACP (editor integration), WASM SDK
Integration effort: **Low**

## Architecture Notes
GitHub API verified: Apache-2.0, 2679 stars, created 2026-08-11, pushed_at 2026-09-02, not archived. Same org as previously catalogued vercel-labs/skills.

## Maturity
Experimental. GitHub API verified: Apache-2.0, 2679 stars, created 2026-08-11, pushed_at 2026-09-02, not archived. Same org as previously catalogued vercel-labs/skills.

## License
Apache-2.0. Apache-2.0, no restrictions on commercial or SaaS use, redistribution, or embedding.

## Alternatives
- OpenCode, Aider, Claude Code, Codex CLI (all heavier TUIs, not designed for embedding)
- vinhnx/VTCode (Rust, safety/governance-focused rather than minimal-footprint)

## Risks / Limitations
- Explicitly marked 'Experimental. Use at your own risk' by the maintainers — v0.0.7, pre-1.0
- WASM SDK and durable background-process handling called out as unstable
- 3 weeks old; Vercel Labs has a mixed track record of maintaining side projects long-term

## Recommendation
**PROTOTYPE** (score 8.0/10) — see Why I Should Care above.

## Change History
### 2026-09-02
First discovered and reviewed. Verified via GitHub API: license Apache-2.0, current version v0.0.7 (2026-08-29).
