# Bernstein

## Summary
A deterministic orchestrator for CLI coding agents (Claude Code, Codex, Gemini CLI, and 40+ more). It decomposes a goal into tasks, spawns each coding task in an isolated git worktree, verifies concrete signals (tests, lint, types), and merges verified work — with a replay journal, an always-on lineage spine, and an opt-in HMAC-chained, Ed25519-signed audit trail a reviewer can check offline without rerunning anything.

## Why I Should Care
Fanning out multiple Claude Code/Codex sessions across a backlog today means ad hoc tmux/shell scripting with no reproducibility and no record of what actually happened. Bernstein's core claim — no LLM in the coordination loop, so a run replays byte-identically — turns "the agent did something overnight" into something you can verify rather than trust.

## Problems It Can Remove
- Hand-rolled scripts for running multiple coding-agent sessions in parallel against isolated git worktrees.
- The lack of an audit trail for autonomous agent runs — useful both for personal trust and for any client-facing story about AI-assisted delivery.
- Manually tracking which parallel task succeeded, failed, or needs a different model retry.

## Practical Uses
- Fan out a feature/bugfix backlog across parallel Claude Code/Codex/Gemini CLI sessions, each in its own worktree.
- Produce a signed, offline-verifiable receipt of an autonomous run as evidence of what an agent changed.
- Benchmark agent reliability with `bernstein bench run --reliability k` (pass^k floor) instead of trusting a single pass@1 run.
- Watch progress live via the TUI (`bernstein live`) or browser dashboard (`bernstein gui serve`).

## Product Opportunities
- Signed run receipts plus the newer governance layer (identity lifecycle, credential brokering, ISO 42001 / PROV-O evidence exports) are now a substantially stronger compliance artifact for security-conscious clients who want proof of what an AI agent changed — relevant to the MSSP/PTaaS side of the business.
- Could underpin an internal "agent ops" dashboard for auditing AI-assisted delivery work, now with a one-command `bernstein govern` audit report.

## Agent / Automation Opportunities
This is itself agent-orchestration infrastructure: CLI + TUI + browser dashboard, 40+ built-in CLI agent adapters, plus a generic `--prompt` wrapper for anything not natively supported.

## Integration
`pipx install bernstein` / `uv tool install bernstein` / brew / npm / Docker / air-gapped wheelhouse. `bernstein init` scaffolds `.sdd/` in a project, then `bernstein -g "<goal>"` runs it. Low integration effort — CLI-first, no server to stand up for the basic case.

## Architecture Notes
Plain-Python scheduling (not LLM-driven) is the deliberate design choice that makes runs reproducible. Each coding task gets an isolated git worktree; artifact-mode tasks (reports, datasets, non-code deliverables) get a working directory instead and complete on a signed lineage receipt rather than a git commit. The four-stage flow — decompose (one LLM call) → spawn (isolated worktrees) → verify (concrete signals) → merge — is a reusable pattern independent of whether Bernstein itself is adopted.

## Maturity
Emerging, bordering mature by engineering signals: 158 PyPI releases at v3.18.2, real multi-human contributor list beyond the primary author (Chirag6722, shanemmattner, vaibhav8a, Phoenix1504e, Louis20060723) plus renovate/dependabot/github-actions bot automation. Self-described "Status: beta" in the README — interfaces may change across minor versions despite the high version number counting releases, not maturity.

## License
Apache-2.0. No restrictions on commercial use, redistribution, or embedding.

## Alternatives
Manual tmux/shell fan-out of multiple agent sessions (status quo); coleam00/Archon (higher-level harness builder, different focus); ruvnet/ruflo (agent meta-harness, less deterministic/audit-focused).

## Risks / Limitations
- Solo-dominated commit history (chernistry: 3,776 commits vs. next contributor at 55) — bus-factor risk despite bot infrastructure and a growing contributor list.
- Explicitly unstable interface across minor versions per the README.
- Python/uv-based tool, adding a non-Node dependency to an otherwise JS/TS-centric toolchain — acceptable since it's consumed as a CLI, not a library.

## Recommendation
PROTOTYPE — pilot on one real backlog of parallel-safe tasks before relying on it for anything unattended. The determinism and audit-trail claims are unusually well substantiated (offline-verifiable signed receipts, CI that re-verifies a committed demo receipt on every push) for a project this new to the space.

## Change History
### 2026-08-30
Discovered and reviewed. GitHub API verified: Apache-2.0, 1,036 stars, pushed_at 2026-08-30 (same day), created 2026-03-22. PyPI confirms v3.18.2, 158 releases.

### 2026-09-04
Meaningful update: v3.18.2 → v3.19.1 (94 changes) is a positioning shift, not a patch bump. The project now frames itself explicitly as "a governance framework for AI agents": fail-closed approval timeouts, credential brokering that never stores secret values, an HMAC-chained principal identity lifecycle with scope-narrowing on child identities, signed drift observations, per-principal/per-grant cost rollup with budget-cap chain events, and compliance evidence exports (ISO/IEC 42001 Annex A, W3C PROV-O, signed TRACE 0.2 records). Status kept at PROTOTYPE (still self-described beta), but this materially strengthens the MSSP/PTaaS product-opportunity angle noted at first review.
