# brief

## Summary
Single-binary Go CLI that scans a project (local path, git URL, or registry package like `npm:`/`pypi:`/`crate:`) and reports its language, package manager, test runner, linter, formatter, and build tooling as structured JSON or human-readable output. Also does diff-scoped detection between two refs, dependency-sink/threat-model summaries, and packing a source tree into one document for an LLM.

## Why I Should Care
Every new repo session with a coding agent starts with the same few questions — what language, how to install, how to test, how to lint. `brief` answers all of them in one deterministic, non-judgmental report instead of burning a turn on exploration or getting it wrong.

## Problems It Can Remove
Replaces manually reading `package.json`/`pyproject.toml`/etc. (what most agents and new contributors do today) to bootstrap understanding of an unfamiliar repo's toolchain.

## Practical Uses
- Add `brief .` to a `CLAUDE.md`/`agents.md` instruction so a coding agent gets install/test/lint commands as facts instead of guessing.
- `brief diff` on a feature branch to narrow which linters/test frameworks are relevant to a changed subset of files.
- Quick onboarding check on an unfamiliar repo (own or a client's) before diving in.
- `brief outline` to pack a source tree into one document when handing context to an LLM without an agent harness.

## Product Opportunities
None directly — this is a developer-tooling utility, not a product-building block.

## Agent / Automation Opportunities
Explicitly designed as an agent bootstrap step — directly reduces the token/time cost of the first few turns of any coding-agent session on a new repo. `brief threat-model` and `brief sinks` give a cheap first-pass security-surface summary of a stack, potentially useful ahead of a PTaaS engagement's reconnaissance phase.

## Integration
Install via `brew install git-pkgs/git-pkgs/brief`, `go install`, or a downloaded release binary. No configuration required for basic use. Low integration effort — it's advisory output, not something wired into a build.

## Architecture Notes
Supports registry-package resolution (`npm:express`, `pypi:requests`, `crate:serde`, `gem:rails`) by resolving to source repos, not just local scans. Path-role filtering excludes fixtures/vendored/generated files from detection evidence while still using them as dependency evidence where appropriate (e.g. lockfiles).

## Maturity
Emerging: v0.14.2, created 2026-03-15, pushed 2026-10-25 (today). 87 stars, 6 forks, 1 subscriber, 6 contributors (lead author ~249/316 commits).

## License
MIT — unrestricted commercial use.

## Alternatives
Manually reading manifest/config files (the default today). Overlaps only partially with repomix-style source-packing tools via the `outline` subcommand; toolchain/test/lint detection is not covered by those.

## Risks / Limitations
- Small project (87 stars, 1 subscriber) — some risk of stalling, though low-stakes since its output is advisory rather than load-bearing.
- `threat-model`/`sinks` subcommands are a first-pass heuristic summary, not a substitute for a real security scanner.

## Recommendation
USE NOW — near-zero-risk, low-effort addition to any coding-agent workflow across multiple repos.

## Change History
### 2026-10-25
Discovered via GitHub search for recently-updated single-binary CLI tools. Today's Small but High-Leverage Utility pick.
