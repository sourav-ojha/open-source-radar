# skills (Vercel Labs)

## Summary
`npx skills` is a CLI package manager for the open agent-skills ecosystem. It installs,
lists, updates, and removes `SKILL.md` packages across 70+ supported coding agents
(Claude Code, Cursor, Codex, OpenCode, and more) from GitHub, GitLab, arbitrary git
URLs, direct download URLs, or local paths. Backed by Vercel Labs.

## Why I Should Care
Skill packages are becoming the standard way to extend coding agents, and this is the
closest thing to an official, cross-agent package manager for them — already supporting
70+ agents with zero-config `npx` install. It is directly in the same category as the
mechanism this very radar's own `AGENT.md` operating spec informally reimplements by
hand.

## Problems It Can Remove
- Hand-copying skill folders into per-tool config directories for every agent in use
- Building a bespoke internal distribution mechanism for shared agent playbooks/runbooks

## Practical Uses
- Install/manage shared `SKILL.md` packages across every agent in use (Claude Code,
  Cursor, etc.) from one CLI instead of maintaining copies per tool
- Pull a private, org-specific skills repo into every teammate's agent setup with one
  command
- Distribute an internal playbook (deployment runbook, an AGENT.md-style operating
  spec) as an installable skill instead of a wiki page nobody opens mid-session
- `skills use <source> | claude` to trial a skill without permanently installing it

## Product Opportunities
If an agent-based feature ships inside a product, this is a ready-made distribution
mechanism for teaching it new capabilities without building a bespoke plugin system.

## Agent / Automation Opportunities
It **is** agent infrastructure directly — the packaging/distribution layer for skills.
Worth studying its source-resolution logic (GitHub shorthand, private-repo auth
fallback chain, direct SKILL.md URLs, archive extraction with size limits) as a
reference even independent of adoption.

## Integration
`npx skills add <owner>/<repo>` — zero local install required, works interactively or
non-interactively (CI-friendly with `-y`/`--all`). **Low** integration effort.

## Architecture Notes
Notably careful about credential handling: for private repos it tries normal git
credentials, then `gh repo clone`, then SSH — and explicitly documents that it never
executes `gh auth token` or reads the stored GitHub CLI credential into the Node.js
process. Downloads are capped (10 MiB by default, 25 MiB extracted, 1000 files per
archive) with env-var overrides — a reasonable default-safe posture for a tool that
downloads and executes third-party content into an agent's environment.

## Maturity
Emerging but fast-moving: created 2026-01-14 (about 7 months old), already at 29,960
stars, pushed 2026-08-18, latest release v1.5.23 the same day. 1,153 open issues against
that star count is a notably high ratio — a sign of real usage and real churn.

## License
MIT — no restrictions on commercial use, redistribution, or embedding.

## Alternatives
Community skills marketplaces layered on the same `SKILL.md` convention — 
`dukelyuu/skills-marketplace`, `Karanjot786/agent-skills-cli`,
`jeremylongshore/tons-of-skills-marketplace` — all smaller and less official than the
Vercel-backed tool.

## Risks / Limitations
- High issue-to-star ratio suggests rough edges; verify behavior on the exact agents in
  use (Claude Code) before relying on it for anything automated or CI-driven.
- Vercel-backed but not a first-party Vercel product — long-term governance commitment
  is unclear.

## Recommendation
**PROTOTYPE** — try it for personal/team skill distribution before depending on it in
an automated pipeline; the open-issue volume warrants a real trial rather than blind
adoption.

## Change History
### 2026-08-29
First catalogued. GitHub API verified: MIT, 29,960 stars, pushed_at 2026-08-18, not
archived, created 2026-01-14.
