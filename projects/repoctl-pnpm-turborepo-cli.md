# repoctl

## Summary
A task-first CLI for initializing, maintaining, validating, and releasing
pnpm + Turborepo monorepos: workspace health checks (`repo doctor`),
package/app scaffolding (`repo new`, `repo templates`), repeatable local
verification (`repo check`, `repo verify`), and stable/prerelease publishing
driven by pnpm change intents — with machine-readable JSON/Markdown output
for CI and editors.

## Why I Should Care
Turborepo itself only handles task orchestration; it has no opinion on
workspace health, scaffolding conventions, or release publishing. repoctl
fills exactly that gap for a pnpm+Turborepo monorepo — his actual Node/React
stack — without pulling in Nx's much larger generator ecosystem.

## Problems It Can Remove
- Bespoke scripts for checking that a pnpm+Turborepo workspace is
  internally consistent.
- Copy-pasting an existing package/app as a scaffold instead of a real
  generator.
- Hand-rolled changesets-plus-custom-scripts release flow.

## Practical Uses
- Running `repo doctor` as a one-command workspace health check instead of
  a bespoke script.
- Scaffolding a new package/app with house conventions via `repo new`.
- Gating CI on `repo verify` with machine-readable JSON output instead of
  hand-rolled lint/build/test glue.
- Publishing stable or prerelease versions from pnpm change intents with a
  consistent process across every package in the workspace.

## Product Opportunities
None directly — internal tooling, not a product-building block.

## Agent / Automation Opportunities
`repo check` / `repo verify`'s machine-readable JSON output is directly
consumable by a coding agent deciding whether a monorepo change is safe to
merge, without having to parse human-oriented CLI output.

## Integration
`pnpm add -D repoctl`; works against an existing pnpm+Turborepo workspace
with no migration required. Node `^22.23.4 || ^24.22.1 || >=26.12.1`
required; Node 25+ needs `corepack` installed manually since it no longer
bundles it. **Integration effort: Low.**

## Architecture Notes
Nothing unusual architecturally — a conventional workspace CLI — but the
codecov-tracked test suite and the "workspace upgrades preserve unowned
project files by default" design detail suggest real production use rather
than a toy generator.

## Maturity
Created 2022-11-02 (~3 years old), MIT licensed, latest npm release 6.6.1
published 2026-11-12 — steady day-to-day maintenance over a long period.
49 stars, 7 forks, 1 subscriber — thin community signal for the project's
age, but the multi-year commit history and codecov integration argue for
a real, maintained tool rather than an abandoned experiment.

## License
MIT. No commercial-use restrictions.

## Alternatives
- Turborepo's own CLI (task orchestration only, no workspace health/release
  tooling).
- Nx (far heavier, opinionated generator ecosystem, higher adoption cost).
- Hand-rolled changesets + custom shell scripts.

## Risks / Limitations
- Scoped specifically to pnpm + Turborepo; no value outside that
  combination.
- Very small community despite three years of development — limited
  external validation beyond the maintainer's own usage.

## Recommendation
**PROTOTYPE** — run `repo doctor` against an existing pnpm+Turborepo
workspace to see what it actually flags before adopting it for releases.

## Change History
### 2026-11-13
Initial discovery and review. Rotation slot 3 (developer utilities,
debugging, testing).
