# Supercov

## Summary
CLI coverage tool built around a one-gap-at-a-time loop for coding agents: runs your
existing test command unmodified, reports line/statement/function/branch/MC-DC coverage
plus per-test attribution, lists the single most useful uncovered path, and shows a diff
of what a new test actually covered. An optional paid add-on scores code quality and
security per file.

## Why I Should Care
Most coverage tools stop at a percentage. Supercov turns "coverage is low" into a
concrete, boundable unit of work — one uncovered path, one test, one verification step —
which is exactly the shape of task a coding agent (or a human on autopilot) can execute
reliably without drifting into unrelated changes.

## Problems It Can Remove
Manually hunting for the most valuable uncovered code path after a PR; writing a
cross-language coverage CI check from scratch; standing up a hosted CodeCov/Coveralls
account just to see which lines a PR left untested.

## Practical Uses
- Wire into CI so a GitHub Action comments on PRs with exactly which changed lines have
  no test coverage, and optionally fails below a threshold
- Point a coding agent at `supercov gaps` and let it close one uncovered path per
  iteration instead of chasing an aggregate percentage
- Run overnight/unattended as a "software factory" loop that is restricted to touching
  tests only, never application code
- Use `supercov diff` to confirm a new test actually covers what it was written for

## Product Opportunities
None specific — this is developer/agent tooling, not a product-embeddable component.

## Agent / Automation Opportunities
This is close to a reference implementation of "give an agent one bounded verification
task." Ships as a Claude Code / Codex / Gemini CLI agent skill with slash commands
(`/supercov:coverage`), plus a GitHub Action for CI. No MCP server mentioned in the docs.

## Integration
Single CLI install via npm, PyPI, RubyGems, crates.io, Go, or Homebrew (`npx supercov --
npm test`). Runs the existing test suite in an isolated workspace without touching
source/config. Data stays local in a git-ignored `.supercov/` directory; reports are
self-contained HTML files. Native binaries for macOS/Linux/Windows, x64/arm64. Low
integration effort.

## Architecture Notes
Supports JavaScript, TypeScript, Python, Ruby, Rust, Go, Java, and Kotlin (Zig/PHP/C
listed as coming soon) with a single consistent CLI surface across all of them —
non-trivial to get right, and the actual differentiator versus per-language coverage
tools. The optional quality/security scoring is a separate network call to a paid
"TypeSafe AI" (Jev) service, cached by content hash so reruns only pay for changed files.

## Maturity
Emerging. Created 2026-08-23 — under two months old at review — but already shipping
npm v4.12.2, published via GitHub Actions OIDC trusted publishing (a credible modern
supply-chain signal). Fast version churn is worth watching before depending on it in CI.

## License
MIT for the core CLI — no restrictions. The optional quality/security scoring feature
requires a separate paid API key (~$0.01/MB, cached); core coverage reporting works
without it and without any account.

## Alternatives
Istanbul/nyc, c8, and built-in Jest/Vitest coverage give raw numbers with no agent-
directed workflow. CodeCov/Coveralls are hosted dashboards with no local-only option and
no agent loop. SonarQube is heavier and server-based. Supercov's distinct pitch is the
single-gap-at-a-time loop plus cross-language parity in one CLI.

## Risks / Limitations
- Under two months old with rapid version churn (already v4.12.2) — verify stability
  before trusting it in a CI gate
- 0 GitHub "watchers" against 155 stars and 6 forks is a minor engagement-signal oddity,
  not independently disqualifying
- The paid scoring add-on is a proprietary dependency; confirm the free coverage path is
  fully standalone before building on it

## Recommendation
PROTOTYPE — try the core CLI (free, local-only) on one real repo's CI pipeline before
touching the paid scoring feature.

## Change History
### 2026-10-16
First discovery and review.
