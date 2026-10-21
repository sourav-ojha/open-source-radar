# AI-DLC Workflows

## Summary
AWS Labs' open-source "AI-Driven Life Cycle" (AI-DLC): a harness-neutral workflow engine that turns a coding agent into a structured, phase-gated software-delivery process — 5 phases, 33 stages, 14 agents (11 domain experts, 2 reviewers, 1 adaptive composer), human approval gates, and a 121-event audit trail. One deterministic core engine with thin per-harness integrations for Claude Code, Codex CLI, Cursor, Kiro, opencode, and GitHub Copilot. Invoked as `/aidlc` (or `$aidlc` in Codex CLI).

## Why I Should Care
Most "spec-driven development" tools in this space are single-author weekend projects with no comparable backing. This one is an official AWS Labs repo with real multi-contributor commit activity, a daily preview-release cadence, and genuine support for six different coding-agent harnesses rather than being locked to one. It directly targets the problem of an agent going off-process on a real feature — exactly the failure mode that makes ad-hoc agent-driven coding risky to trust on anything beyond a small change.

## Problems It Can Remove
Replaces hand-written CLAUDE.md conventions and ad-hoc slash commands for enforcing a review/approval process on agent-driven work. Removes the need to design a phase-gated workflow (design → plan → implement → review) from scratch per project.

## Practical Uses
- Force a design + security review gate before an agent is allowed to touch code on a change that matters.
- Get a persistent audit trail of what an agent decided and why across a multi-session feature build.
- Run the same structured workflow whether the agent of the day is Claude Code at a desk or Codex CLI over SSH.
- Use as a reference architecture for designing a lighter-weight internal process, even without adopting it wholesale.

## Product Opportunities
Could inform an internal "how AI-agent-assisted changes get reviewed" standard for the MSSP admin-portal SaaS or PTaaS codebases, where an audit trail of agent decisions has direct compliance value.

## Agent / Automation Opportunities
The harness-neutral design is the core agent opportunity: one workflow definition works across Claude Code, Codex, Cursor, Kiro, and Copilot without rewriting the process per tool. Ships 96 `aidlc-*.ts` tools in its core engine.

## Integration
Install via a curl (macOS/Linux/WSL) or PowerShell (Windows) script; no Node.js or Bun required for the installed CLI path. Run `aidlc config --harness <name>` then `aidlc doctor`, then invoke `/aidlc <description>` from inside the chosen harness. A manual-copy path exists for those who want to inspect the `runtime/<harness>/` directory from a release tarball instead. Integration effort: **Low** — install script plus one config command, no code changes required to an existing project.

## Architecture Notes
Single deterministic engine (`core/`) shared across all harnesses; `harness/<name>/` holds per-runtime manifests; `plugins/` for optional extensions. The approval-gate + audit-trail design (121 tracked event types) is the most transferable idea here even if the full workflow isn't adopted — worth studying as a pattern for building guardrails around any multi-step agent process.

## Maturity
Emerging. Created November 2025, actively developed with near-daily preview tags (e.g. `v2.11.2-preview.20261020.1`). Verified via GitHub API: 5,433 stars, 979 forks, 51 subscribers, 10+ distinct real contributors on the contributors endpoint, not archived.

## License
MIT-0 (MIT No Attribution) — verified via GitHub API license field. The most permissive common open-source license; no attribution requirement, no commercial-use friction at all.

## Alternatives
- **MoAI-ADK** (catalogued, STUDY) — Claude Code-specific verification harness with Claude×GLM cost routing; narrower tool scope, no multi-harness support.
- **Spec Kitty, buildermethods/agent-os, GSD Pi** (evaluated previously, not catalogued — saturated spec-driven-dev wave) — AI-DLC differentiates on official AWS backing and genuine multi-harness design.
- **ai-coding-project-boilerplate** (catalogued today) — the lightweight, single-harness, TypeScript-specific alternative for teams that want far less process overhead.

## Risks / Limitations
- Heavy process for a solo developer — built for team-scale workflows; no confirmed "lite mode" equivalent.
- Early product category despite the backing; the workflow structure itself still appears to be in active tuning given the release cadence.
- No formal Releases/CHANGELOG surfaced via the GitHub API beyond the latest tag at review time, making it harder to audit version-to-version changes than a project with full release notes.

## Recommendation
**PROTOTYPE** — worth trying on a real feature to see whether the phase-gate overhead pays for itself, especially for higher-stakes changes in the MSSP/PTaaS codebases where an audit trail has direct value. Not an immediate daily-driver replacement for lighter ad-hoc Claude Code usage.

## Change History
### 2026-10-21
Initial discovery and catalog entry. Slot 1 (AI agents, MCP, coding productivity) run.
