# DvalinCode

## Summary
Independent security verification for code written by humans or AI agents. Re-scans and re-runs a project's own tests/checks itself and reads the resulting exit codes rather than asking the model that made a fix whether the fix worked, then issues a portable "Verified Fix Record": a small JSON file anyone can re-check offline, on a laptop with no network and no Dvalin state.

## Why I Should Care
Heavy use of AI coding agents means fixes — including security fixes — are often accepted on the agent's own say-so. Dvalin's core design explicitly refuses to consult the model that wrote a repair when deciding whether it worked, which is the right shape for that risk. Directly relevant to the MSSP/PTaaS side of the business as well, where "prove it" evidence matters more than a confident claim.

## Problems It Can Remove
- Trusting an AI agent's self-report that a security fix actually worked.
- Manually re-running scans/tests after every agent-authored security fix to confirm it.
- Producing evidence of a fix for a client or auditor beyond "the agent said so."

## Practical Uses
- Gate PRs containing AI-agent-authored fixes (Claude Code, Codex, Copilot) on an independently re-run test/scan instead of trusting the agent's self-report.
- Run a zero-config baseline scan for injection, hardcoded secrets, XSS, `eval`, and unsafe shell use before optionally wiring in semgrep.
- Attach a portable, offline-checkable Verified Fix Record to a PR or incident as MSSP/PTaaS-relevant evidence.
- Use diff-only mode (`diff: true` in the GitHub Action) to gate only what a PR newly introduces, without requiring an already-messy repo to be fully clean first.

## Product Opportunities
The "verified, not self-reported" proof pattern is directly relevant to the MSSP/PTaaS side of the business — a similar record format could back a client-facing "we verified the AI-assisted fix" deliverable.

## Agent / Automation Opportunities
Has a built-in remediation executor that can call Claude Code/Codex/Copilot to attempt a fix, but — deliberately — the verification step never consults the agent that wrote it. Interoperates with other systems (e.g. Codex Security) via portable SARIF rather than trying to be the only tool in the loop.

## Integration
`npx dvalincode security scan .` runs with no install, no API key, and no network calls for the built-in ruleset. `dvalin init` + `dvalin baseline` sets up an incremental "no new high-risk findings" gate. Drop-in GitHub Action posts findings and fix-record verification directly on PRs. Low effort.

## Architecture Notes
Findings and fix records travel as a versioned, hashed envelope; a record edited after issuance fails re-verification, and the CI action re-derives the verdict from the file alone rather than trusting the pipeline that produced it. Every comment states scan coverage (complete/partial/unknown) alongside the result, so "no findings" from a partial run isn't confused with "no findings" from a complete one.

## Maturity
Emerging. 113 stars, pushed same-day, created May 2026, 12 contributors, 298 commits, 442/442 tests badge, OpenSSF Scorecard badge, versioned releases (v0.18.0), i18n (EN/zh-CN). Polished for its age but still early/pre-mature in spirit — single-dominant-author commit pattern typical of this cohort.

## License
MIT. No restrictions on commercial use, redistribution, or embedding.

## Alternatives
Trusting the coding agent's own claim that a fix works (status quo); Semgrep/CodeQL alone (surface findings but don't verify a remediation against them); Codex Security (Dvalin explicitly interoperates via portable SARIF rather than competing with it).

## Risks / Limitations
- Built-in ruleset is intentionally narrow (injection/secrets/XSS/eval/unsafe shell); real coverage depends on wiring in semgrep or another engine.
- "Verified" only covers what the scan actually ran — coverage is reported explicitly, but a shallow default policy could be mistaken for a full audit if that distinction is missed.
- 113 stars and a single-dominant-author pattern — worth watching for bus-factor risk despite the engineering polish.

## Recommendation
PROTOTYPE — worth trying on one repo's PR pipeline specifically for AI-agent-authored fixes, given the direct fit with both heavy agent use and the MSSP/PTaaS evidence angle.

## Change History
### 2026-09-04
Discovered and reviewed. GitHub API verified: MIT, 113 stars, pushed_at 2026-09-04 (same day), created 2026-05-20, 12 contributors, 298 commits. npm confirms dvalincode@0.18.0, MIT.
