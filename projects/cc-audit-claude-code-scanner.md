# cc-audit

## Summary
A deterministic, "AI-free" static security scanner for third-party Claude Code Skills,
Hooks, and MCP server configs. Flags data exfiltration, prompt injection, privilege
escalation, and overpermissive tool access (e.g. `allowed-tools: *`) before an artifact
from an unaudited marketplace gets installed.

## Why I Should Care
Anthropic's own security docs state it does not audit third-party MCP servers. As the
skill/hook/MCP ecosystem grows, installing an unreviewed artifact is a real, currently
unaddressed supply-chain risk — this closes that gap with a five-second scan.

## Problems It Can Remove
- Manually reading every third-party skill/hook/MCP config before installing it (what
  most people actually do today: skip the review entirely).
- No pre-commit or CI gate exists today to stop an overpermissive or exfiltrating
  artifact from being added to a shared repo.

## Practical Uses
- `cc-audit check ./skill/` before installing any new SKILL.md, Hook, or MCP config from
  awesome-claude-code or a similar marketplace.
- `cc-audit check --all-clients` to retroactively sweep everything already installed
  across Claude Code, Cursor, and other supported clients.
- `cc-audit hook init` to install a git pre-commit hook that scans any skill/hook added
  to a shared repository automatically.
- CI integration (GitHub Actions / GitLab CI) to fail a PR that introduces a
  wildcard-tool-permission skill or an exfiltration pattern.

## Product Opportunities
Directly relevant to the MSSP/PTaaS side of the business as a concrete, demoable example
of supply-chain scanning applied to the fast-growing agent-artifact ecosystem — a
narrow but real category (agent-artifact security) worth watching for adjacent tooling.

## Agent / Automation Opportunities
Runs as an MCP server itself (`cc-audit serve`) and as an MCP proxy for runtime
monitoring (`cc-audit proxy`), so an agent can be instructed to audit a new skill before
self-installing it.

## Integration
Homebrew, Cargo, or npm; also distributed as prebuilt binaries via GitHub Releases.
Single static binary, no runtime dependency. Integration effort: **Low**.

## Architecture Notes
Rust CLI with a CWE-mapped rule set (e.g. `EX-001` for network calls using an
environment-variable secret, `OP-001` for wildcard tool permissions), producing a risk
score, severity-tagged findings, and a "why/ref/fix" explanation per finding. Explicitly
deterministic/rule-based rather than LLM-based, so results are reproducible.

## Maturity
Created 2026-01-24, 24 stars, 5 contributors, current release v3.22.25 (2026-09-16,
the day of this review). Small but actively maintained, with CI, codecov, and docs.rs
documentation in place — signs of real engineering discipline despite the small
audience.

## License
MIT. No commercial restrictions.

## Alternatives
- Manual code review of every third-party artifact (the actual status quo).
- bouncer.run — a hosted web checker for npm packages/MCP servers surfaced via Show HN
  the same week; open-source status unconfirmed, narrower scope than cc-audit's
  Skills/Hooks/MCP coverage.

## Risks / Limitations
- Small project (24 stars, 5 contributors) — rule coverage will have real gaps; treat it
  as a first-pass filter, not a substitute for reading a skill you genuinely don't trust.
- Explicitly static/deterministic ("AI-free" by design), so a cleverly obfuscated
  exfiltration path that doesn't match a known signature will not be caught.

## Recommendation
**USE NOW** — Trivial to install and run; add it as a pre-install check for any new
Claude Code skill/hook/MCP server, and as a pre-commit hook on any shared repo that
accepts them.

## Change History
### 2026-09-16
Initial discovery and review. Rotation slot 1 (AI agents, MCP, coding productivity).
Featured as Small but High-Leverage Utility of the day.
