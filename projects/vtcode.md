# VT Code

## Summary
An open-source Rust terminal coding agent built around a safety-first design: restricted shell sandbox, tool guardrails, subprocess isolation, audit logging, and a provider whitelist to prevent accidental data leakage to unapproved LLM endpoints. Modular crate architecture (vtcode-battery-pack) allows embedding individual components.

## Why I Should Care
Most terminal coding agents optimize for capability; VT Code's differentiator is explicit governance controls (provider whitelist, audit logging, approval gates) — the exact concern that comes up once agent tooling moves from personal use toward anything security-sensitive.

## Problems It Can Remove
Sandboxing, subprocess isolation, and a working propose/verify sub-agent split represent significant security-engineering effort beyond what a wrapper script provides.

## Practical Uses
- Terminal coding agent for local-first/offline work via Ollama, LM Studio, llama.cpp
- Corporate-safe agent usage where a provider whitelist and audit log are required
- Study the propose/verify sub-agent separation pattern for a custom agent build

## Product Opportunities
- The provider-governance and audit-logging design is directly relevant to Sourav's MSSP admin-portal context if agent tooling ever needs to be offered to security-conscious customers

## Agent / Automation Opportunities
- MCP + Agent Plugin support; WebMCP lets a browser editor attach to a live terminal session

## Integration
Deployment: CLI (curl install script, Homebrew, Cargo), library (individual crates)
Interfaces: TUI/CLI, Agent Skills, MCP, Agent Plugins, WebMCP (browser-editor bridge)
Integration effort: **Medium**

## Architecture Notes
GitHub API verified: Apache-2.0, 827 stars, created 2025-08-29 (over a year old, actively maintained), pushed_at 2026-09-01, not archived.

## Maturity
Emerging. GitHub API verified: Apache-2.0, 827 stars, created 2025-08-29 (over a year old, actively maintained), pushed_at 2026-09-01, not archived.

## License
Apache-2.0. Apache-2.0, no restrictions.

## Alternatives
- vercel-labs/fx (minimal/embeddable rather than governance-focused)
- Aider, OpenCode, Claude Code, Codex CLI (none emphasize provider whitelisting/audit logging to the same degree)

## Risks / Limitations
- Rust ecosystem is foreign to Sourav's MERN-first stack — some friction versus a Node/TS-native tool
- Local inference explicitly called out as experimental/hardware-dependent
- Active development status — breaking changes possible between releases

## Recommendation
**PROTOTYPE** (score 7.9/10) — see Why I Should Care above.

## Change History
### 2026-09-02
First discovered and reviewed. Verified via GitHub API: license Apache-2.0, current version 0.151.2 (2026-08-30).
