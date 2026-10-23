# mutator

## Summary
A single-binary mutation testing tool from Robert C. Martin (Uncle Bob) covering Clojure, Java, Go, TypeScript, Rust, and Python in one run. It detects each source file's language, applies that language's mutation rules, runs the test suite against each mutant, and writes a differential snapshot that its companion tool `uml-viewer` renders as a kill/survive score.

## Why I Should Care
Mutation testing answers a question code coverage can't: not just "did a line run" but "would the suite notice if the logic on that line broke." TypeScript is a first-class target (same mutation rules as Java, applied to TS/JS syntax, with explicit handling of `??`/`?.` and Express handlers), so this drops straight into a Node/React test suite to reveal tests that pass but don't actually assert anything meaningful.

## Problems It Can Remove
Replaces the manual, rarely-done exercise of auditing whether "100% coverage" tests are load-bearing. Differential re-runs (`--changed`, `--since-last-run`) mean this is cheap enough to run per-PR instead of as a one-off audit, which is the usual reason mutation testing gets tried once and abandoned.

## Practical Uses
- Run `mutator --changed` in CI on a PR's touched files to catch tests that would pass even if the new logic were wrong.
- Point it at one file while writing tests for legacy code with suspiciously high coverage but a history of regressions.
- Use the exit codes (`0` clean, `2` baseline failed, `3` mutant survived) as a CI gate without needing the full uml-viewer stack.
- Combine with existing coverage reports (`--use-existing-coverage`) to skip re-instrumenting a slow test suite.

## Product Opportunities
Not product infrastructure — this is an internal engineering-quality tool, valuable for raising confidence in test suites before shipping, not something to embed in a customer-facing product.

## Agent / Automation Opportunities
A coding agent asked to "add tests" can be graded by mutation score instead of line coverage, which is a much harder target to game. The deterministic exit codes make it a clean CI/agent gate without needing an LLM in the loop.

## Integration
CLI tool, Python-based first run installs a `.venv` automatically. Medium integration effort: the mutation-rule engine is standalone and usable today, but full namespace/function-name attribution requires checking out the sibling repo `crapper` next to it, and the CRAP-score view requires `uml-viewer` — this is part of Uncle Bob's own toolchain, not a fully decoupled product. Usable stand-alone for raw kill/survive counts without the siblings.

## Architecture Notes
Mutation rules for TypeScript, Rust, and Python explicitly reuse the same decisions as the Java ruleset, "spelled in that language" — a pragmatic way to keep a multi-language mutation engine consistent without re-deriving rules per language. Coverage-aware skipping (uncovered lines aren't mutated) and parallel per-file mutant execution (one worker per core by default) keep repeated runs fast enough to use routinely rather than as a rare audit.

## Maturity
Experimental/emerging: created 2026-09-30, 53 stars, 6 forks, single contributor, no tagged releases yet, actively pushed as recently as 2026-10-21 (11 commits in three weeks). Authenticity is not in question — the author is the verified Robert C. Martin (Clean Code, 17.3k GitHub followers, cleancoder.com) — but the project itself is very new and tightly coupled to his own `crapper`/`uml-viewer` ecosystem.

## License
0BSD (Zero-Clause BSD) — maximally permissive, no attribution required, unrestricted commercial use.

## Alternatives
- **Stryker Mutator** (StrykerJS) — the mature, JS/TS-native mutation testing incumbent with IDE integration and dashboards; far more production-ready today.
- **PIT** (Java) / **cargo-mutants** (Rust) — mature single-language mutation testers.
- **EffortlessMetrics/ripr** — previously rejected test-oracle-gap detector in an adjacent niche (2026-10-16), held out for suspicious issue-tracker activity.

## Risks / Limitations
- Single maintainer, no releases, no CI badge — treat as experimental until it stabilizes.
- Full value (namespace/function attribution, CRAP scoring) needs the sibling `crapper` and `uml-viewer` repos checked out alongside it.
- StrykerJS is the safer default for a team that just wants mutation testing on a JS/TS codebase today; this is more interesting for its multi-language architecture and its author's track record than as an immediate drop-in replacement.

## Recommendation
STUDY — the multi-language mutation-rule architecture and the "same decisions, different syntax" design are worth learning from, and it's worth a spot-check run against an existing TS suite to see what it flags, but StrykerJS remains the lower-friction choice for actually adopting mutation testing in a JS/TS project today.

## Change History
### 2026-10-23
First catalogued. Slot 3 (developer utilities, debugging, testing) discovery run. GitHub API verified: 0BSD, 53 stars, 6 forks, created 2026-09-30, pushed 2026-10-21, not archived, 0 releases. Author account verified as Robert C. Martin (Clean Code author).
