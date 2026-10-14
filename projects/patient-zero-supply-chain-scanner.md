# patient-zero

## Summary
Scans Node/Python projects and AI-agent configs for indicators of compromise from known npm/PyPI supply-chain attacks since September 2025 (maintainer-account-phish incidents such as the chalk/debug compromise, and Shai-Hulud-style self-propagating worm packages). Three modes share one IoC database: one-shot `npx patient-zero` triage, install-time blocking that resolves a proposed install tree in a sandboxed temp directory and refuses to proceed on a match (postinstall scripts never execute), and a CI mode. No signup, no telemetry, offline-capable.

## Why I Should Care
The specific failure mode it targets — an AI coding agent installing a compromised transitive dependency from a hijacked maintainer account, unsupervised — is new since agents started running package installs on a developer's behalf without every install being individually reviewed. `npm audit` targets known CVEs, not maintainer-phish/worm IoCs, so this fills a real, narrow, and currently-relevant gap.

## Problems It Can Remove
Manually checking whether a project is affected every time a new npm/PyPI supply-chain incident breaks on Hacker News, and the blind trust an agent's autonomous `npm install` currently requires.

## Practical Uses
- `npx patient-zero@latest` for a 30-second yes/no check when a new incident breaks
- `npx patient-zero@latest install <package>` to let an agent install a package through a sandboxed resolution step that blocks on an IoC match before any postinstall script runs
- Wire the same IoC database into CI as a standing check on every commit/PR

## Product Opportunities
None directly — a narrow security utility, not a product component. Relevant context for the MSSP-adjacent work: the same install-time-blocking pattern is a reasonable feature to point clients toward for their own AI-agent-assisted pipelines.

## Agent / Automation Opportunities
The install-blocking mode is explicitly designed to sit between an agent and `npm install`, which is the most direct agent-automation angle available here.

## Integration
Deployment: `npx patient-zero` (no install required) or add to CI as a GitHub Action.
Interfaces: CLI.
Integration effort: **Low** — zero-config, no signup, works offline against its bundled/fetched IoC data.

## Architecture Notes
Resolves a proposed install tree into a sandboxed temporary directory before it touches the real project, cross-references every transitive dependency against the IoC database, and refuses to proceed on a match — the sandboxing is what lets it block a malicious postinstall script before it ever executes, rather than only detecting after the fact.

## Maturity
Experimental. Repo created 2026-05-14, 18 GitHub stars, single maintainer, MIT license. The npm package has not shipped a new version since its 2026-05-14 initial release (0.2.1) even though the GitHub repo shows commits as recent as today — worth checking directly whether the IoC database updates independently of the npm version before trusting it for a brand-new incident.

## License
MIT, confirmed via GitHub API license field. No restrictions.

## Alternatives
- Socket.dev (commercial) — broader, continuously-updated supply-chain risk platform, not free/self-hosted
- `npm audit` — different threat model entirely (published CVEs against known-vulnerable versions, not maintainer-phish/worm IoCs for very recent incidents)

## Risks / Limitations
- npm package version has not moved in 5 months; verify IoC-database freshness directly rather than trusting the README's "IoC: fresh" claim at face value
- Only 18 stars, single maintainer — low independent validation
- Covers npm/PyPI only; no coverage for other ecosystems (cargo, RubyGems, etc.)

## Recommendation
**PROTOTYPE** (score 7.3/10) — genuinely useful, zero-friction one-off check; verify IoC freshness before leaning on it as a standing CI gate.

## Change History
### 2026-10-14
First discovered via GitHub search API (`mcp security`, pushed:>2026-09-20) during the slot 1 run. Selected as today's Small but High-Leverage Utility.
