# Code Quality (smixs/code-quality)

## Summary
A quality advisor built specifically for coding agents (Claude Code, Codex, Grok, pi, omp, OpenCode): git hooks and agent-lifecycle hooks that flag test tampering, hold a CRAP-complexity bar on changed functions, check diff coverage, and scan for secrets and skipped checks — across 12 languages from one config file, with no server component. It never blocks the agent; it surfaces the evidence and leaves the decision to whoever is reviewing.

## Why I Should Care
The specific failure mode this targets — "the agent made the tests green, but did it fix the code or the tests?" — is a real and growing risk as more of the day-to-day coding gets delegated to an agent. A non-blocking advisory gate that's agent-native (hooks into Claude Code's Stop/UserPromptSubmit/PreToolUse lifecycle, not just a CI step) fits directly into an agent-heavy workflow without adding a review bottleneck.

## Problems It Can Remove
Replaces the manual "did the agent actually fix this or just quiet the test" check that otherwise has to happen in code review. Diff-coverage and secrets scanning on every agent turn also removes a category of agent output that would otherwise only get caught at PR review or, worse, in production.

## Practical Uses
- Install as a Claude Code plugin to get a red-flag report after every agent turn that touched test files.
- Use the CRAP-bar check to stop an agent's refactor from quietly making one function dramatically more complex while "fixing" something else.
- Run the secrets scan as a backstop for agent-generated commits that might paste a key into a config file.
- Point the diff-coverage check at a PR to catch agent-added code with no corresponding test, independent of overall repo coverage.

## Product Opportunities
Internal engineering-quality tooling; not product infrastructure in itself, but the pattern (a cheap, deterministic advisory layer that watches agent output) is a reusable idea for any product that lets end users run agents against their own code.

## Agent / Automation Opportunities
This is itself an agent-tooling product: it ships as installable plugins/hooks for six different agent harnesses, plus an optional "Jev" LLM classifier (TypeSafe/OpenRouter) for cases the deterministic rules can't resolve cleanly. The Stop hook design — never holding the turn, reporting once on the agent's next message — is a clean pattern for non-blocking agent oversight.

## Integration
Bun required for the gate and shell hooks; install is a one-line plugin-marketplace command per agent (Claude Code, Codex, Grok 1.0.40, pi, omp) or an npm/package.json wire-up for OpenCode. Low integration effort for anyone already using one of the supported agent harnesses; no server, no account.

## Architecture Notes
407 passing tests back a 12-language rule set in one config file — unusually well-tested for a single-maintainer project at 19 stars. The optional LLM classifier is explicitly a fallback ("Jev") rather than the primary mechanism, keeping the core gate deterministic and auditable, which matters for something that's advising on whether to trust agent-written tests.

## Maturity
Emerging: created 2026-09-21, pushed 2026-10-04, 19 stars, 2 forks, 4 tagged releases (latest v1.3.3, 2026-09-28), single contributor (86 commits). Young but with real release cadence and test coverage, not a weekend demo.

## License
MIT — unrestricted commercial use and embedding.

## Alternatives
- **commit-check** (already catalogued, 2026-10-16) — adjacent niche (commit-policy-as-code, AI-attribution trailers) but doesn't inspect test-tampering or hold a complexity bar.
- **roborev** (already catalogued) — continuous AI code review daemon; broader and LLM-driven rather than this tool's deterministic-first approach.
- Plain CI coverage gates (Codecov, etc.) — catch coverage drops but not test tampering specifically.

## Risks / Limitations
- Single maintainer, 19 stars — low community validation so far.
- Bun dependency adds a runtime requirement beyond what most Node projects already have installed.
- Grok 1.0.40 hook dispatch was reported broken in the README's own live check at time of writing — verify per-agent support before relying on it broadly.

## Recommendation
PROTOTYPE — low-friction to try against an existing Claude Code or Codex workflow; the test-tampering and CRAP-bar checks are specific enough to be worth a real trial rather than just architecture study.

## Change History
### 2026-10-23
First catalogued. Slot 3 (developer utilities, debugging, testing) discovery run. GitHub API verified: MIT, 19 stars, 2 forks, created 2026-09-21, pushed 2026-10-04, not archived, 4 releases (latest v1.3.3).
