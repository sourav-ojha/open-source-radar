# ActionRepro

## Summary
A Node.js CLI that turns a failed GitHub Actions run into a local reproducibility bundle: it fetches the run/jobs/logs via the GitHub API (or reads a saved log file offline), detects the ecosystem (npm/pnpm/yarn/pip/uv/cargo/go/maven/gradle/dotnet/ruby), extracts the likely failure cause and closest repro command, redacts secrets heuristically, and writes a ready-to-run folder — deterministically, with no LLM, no backend, and no telemetry.

## Why I Should Care
"Scroll raw CI logs on a phone to figure out what broke" is a universal small annoyance in any GitHub Actions-based workflow, and this collapses it to one command that hands back a runnable local repro instead of a wall of text — directly useful for the Node/React/GitHub Actions slice of the stack.

## Problems It Can Remove
Replaces the manual process of reading a failed workflow's logs, guessing the ecosystem and command that failed, and manually reconstructing the environment to reproduce it locally.

## Practical Uses
- Run `actionrepro <run-url>` the moment a CI run fails to get a local repro bundle instead of reading logs in the GitHub UI.
- Use local-file mode (`actionrepro ./failure.log`) for air-gapped or already-downloaded logs, no network needed.
- Use the `verify` command to confirm a fix actually resolves the extracted failure before pushing.
- Wire it into coding-agent workflows via its MCP support, so an agent can pull a structured repro bundle instead of parsing raw CI output itself.

## Product Opportunities
Not product infrastructure — a personal/team engineering-velocity tool, valuable for shortening the CI-failure-to-fix loop on any GitHub Actions-based project.

## Agent / Automation Opportunities
Ships explicit MCP support for coding agents, and deliberately avoids an LLM in its own extraction logic (ecosystem detection and failure-cause extraction are rule-based) — a sensible split where the deterministic tool produces structured evidence and an agent (if involved at all) only reasons over that evidence rather than raw logs.

## Integration
Single Node.js CLI (Node ≥ 18), no Docker required, installable from source today. Low integration effort: point it at a failed run URL or a saved log, optionally add the companion GitHub Action for other repos. Secret redaction is heuristic, not guaranteed — treat the bundle as still worth a glance before sharing it.

## Architecture Notes
The `## [error]` output is deliberately written with a space to stop CI runners from parsing the tool's own output as phantom workflow annotations — a small but telling sign of someone who has actually hit that bug. Differential, multi-ecosystem failure detection in one dependency-free binary is a good scope for a utility this narrow: wide ecosystem coverage, no attempt to also be a test runner or dashboard.

## Maturity
Very early: created 2026-10-05, pushed 2026-10-10, 3 stars, 1 fork, 5 tagged releases already (latest v0.1.1) despite the young age — an unusually active release cadence for an 18-day-old, 3-star repo, suggesting real iterative use rather than a one-shot demo.

## License
MIT — unrestricted commercial use.

## Alternatives
- `act` (nektos/act) — runs GitHub Actions workflows locally wholesale; heavier and requires Docker, versus ActionRepro's lighter "just reproduce this one failure" scope.
- Manually re-reading workflow logs in the GitHub UI — the status quo this directly replaces.

## Risks / Limitations
- 3 stars, single contributor, very new — no outside validation yet.
- Secret redaction is explicitly heuristic; don't rely on it as the only safeguard before sharing a bundle externally.
- Not on npm yet (install from source only at time of review).

## Recommendation
PROTOTYPE — small enough to try on the next real CI failure; today's Small but High-Leverage Utility pick.

## Change History
### 2026-10-23
First catalogued. Slot 3 (developer utilities, debugging, testing) discovery run. GitHub API verified: MIT, 3 stars, 1 fork, created 2026-10-05, pushed 2026-10-10, not archived, 5 releases (latest v0.1.1).
