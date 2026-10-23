# monorepo-doctor (Monorepo Skills)

## Summary
A read-only AI agent skill that audits npm/yarn/pnpm/bun JS/TS workspaces (with deeper rules for Nx and Turborepo) across five independent checks — package boundaries, circular dependencies, task-cache correctness, CI test coverage, and test code leaking into production — and writes an evidence-backed Markdown and JSON report. A companion skill, `monorepo-boundaries-cleanup`, takes the findings and pays them down in small PRs against a baseline.

## Why I Should Care
This lands directly on the stack: JS/TS monorepos are exactly the shape of project most likely to accumulate the problems it checks for (wrong-direction imports, deep imports into another package's internals, cache configuration that replays stale output, test fixtures crossing workspace boundaries). It's a read-only audit first, which is the right order of operations before letting anything touch code.

## Problems It Can Remove
Replaces the ad-hoc, rarely-done exercise of manually tracing import boundaries and cache config correctness across workspace packages — the kind of audit that's valuable but tedious enough that it mostly doesn't happen until something breaks in CI.

## Practical Uses
- Run the audit skill on an existing Nx/Turborepo monorepo to get a baseline report of boundary violations before a planned refactor.
- Use the circular-dependency check (package-level cycles and file-level cycles, severity-ranked) to triage which cycles are actually risky versus idiomatic lazy ORM references.
- Feed the Markdown/JSON report into a PR template or CI comment for visibility on each release.
- Hand the baseline to the companion `monorepo-boundaries-cleanup` skill to open small, reviewable PRs paying down violations incrementally.

## Product Opportunities
Not a product in itself — this is an internal dev-productivity skill for monorepo hygiene, valuable for keeping any of his own multi-package products maintainable as they grow.

## Agent / Automation Opportunities
This is an agent skill by design (ships as a Claude Code-style `SKILL.md`, with an `llms.txt`/`llms-full.txt` for agent consumption), explicitly split into a read-only diagnostic skill and a separate edit-and-PR skill — a clean pattern for keeping an agent's destructive actions behind a human checkpoint (it "edits code and opens PRs, then stops and waits for your CI report").

## Integration
Installed as an agent skill (Node ≥ 22.5 required for the audit). Low integration effort: no server, no new infra, just an agent skill pointed at an existing monorepo. The boundaries-cleanup companion skill is explicitly scoped to open PRs rather than push directly, keeping human review in the loop.

## Architecture Notes
The boundaries check distinguishes packages "no other workspace package imports" (never flagged, since an unused package isn't a boundary problem) from packages that are imported through another package's internals — a sensible refinement over a naive "flag every deep import" rule. File-cycle severity grading (medium if a binding is read during module evaluation, low if every use is deferred, skipped for idiomatic lazy ORM references) shows real effort went into reducing false positives, which is the main reason this class of linter usually gets disabled.

## Maturity
Emerging: created 2026-10-10, pushed 2026-10-22, 8 stars, 1 fork, 2 contributors, dedicated docs site (monorepo-skills.dev). Very new — no independent validation yet beyond the documented design — but the check logic described is specific and detailed rather than generic.

## License
MIT — unrestricted commercial use and embedding.

## Alternatives
- **Nx `enforce-module-boundaries`** / **`turbo boundaries`** / **dependency-cruiser** — the linter-level boundary enforcement this skill explicitly builds on top of and cites as alternatives for the cleanup step.
- **knip** — unused-dependency detection, referenced as an input signal rather than a competitor.
- Manual `madge`/dependency-graph audits — the status quo this replaces with a structured, repeatable report.

## Risks / Limitations
- 8 stars, 2 contributors, 12 days old — essentially unvalidated outside its own docs.
- Node ≥ 22.5 requirement may be ahead of some existing CI images.
- Deeper rules are Nx/Turborepo-specific; plain npm/yarn/pnpm/bun workspaces get the shallower checks only.

## Recommendation
PROTOTYPE — worth running once against an existing Node/React monorepo as a free audit; the read-only-then-PR split is also a pattern worth reusing for other agent-driven refactor tooling.

## Change History
### 2026-10-23
First catalogued. Slot 3 (developer utilities, debugging, testing) discovery run. GitHub API verified: MIT, 8 stars, 1 fork, created 2026-10-10, pushed 2026-10-22, not archived, 0 releases.
