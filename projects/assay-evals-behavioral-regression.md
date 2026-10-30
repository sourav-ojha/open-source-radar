# Assay (assay-evals)

## Summary
Behavioral regression testing for AI apps/agents in Python and TypeScript: a pytest plugin and a Jest/Vitest/node:test helper that wraps an existing LLM SDK client (`instrument(new Anthropic())`), records each test case's tool calls/cost/context against its own last passing run, and on a prompt or model change tells you exactly which cases changed, what changed about them, and whether the change is a real regression or noise (via repeated-run sampling, not a single diff).

## Why I Should Care
A green test suite doesn't tell you an agent's *behavior* changed — it only tells you the final assertion still passes. Assay's own pitch example is the real failure mode: one line added to a support agent's prompt makes it skip a refund approval step while the final reply still looks fine; `pytest --assay` catches it because it's diffing tool-call sequences, not just output text.

## Problems It Can Remove
- Manually re-checking an agent's tool-calling behavior after every prompt or model change.
- False confidence from a passing output-quality eval (promptfoo-style) when the underlying tool-call sequence actually regressed.
- Mistaking normal agent non-determinism for a real regression (or the reverse) — repeated-sample comparison and calibration against human labels address this directly.

## Practical Uses
- `assay init && pytest --assay tests/ai` to gate a prompt-engineering PR on behavioral regressions, not just output checks.
- Wrap an existing OpenAI/Anthropic/Vercel AI SDK client with `instrument()` and get call-level recording with no manual trace instrumentation.
- `assay diff` after a model swap to see case-by-case what changed in tool-calling behavior before shipping the swap.
- Use the document-extraction metrics mode for agents doing structured extraction work, where "did the schema output change" matters as much as tool calls.
- Wire the PR check so a reviewer sees "3 regressed, 1 flaky" directly in the pull request rather than eyeballing conversation logs.

## Product Opportunities
Same micro-SaaS shape as the already-catalogued whatbroke: a narrow, well-defined CI-gate-as-a-service for teams shipping tool-calling agents, with the added angle of judge calibration against real human labels as a trust signal a hosted product could sell on.

## Agent / Automation Opportunities
Built to sit in CI (GitHub Check / PR comment) and as a judge-calibration layer (Cohen's kappa against human labels) — the `assert_called`/`assert_not_called`/`assert_max_steps` primitives are a reusable pattern for grading any coding or support agent's tool usage deterministically rather than via another LLM judge.

## Integration
`pip install assay-server pytest` or `npm install --save-dev assay-evals` plus `pip install assay-server` for the CLI; `assay init` detects the test runner (pytest, Jest, Vitest, node:test) and scaffolds config. Low effort for a project already using one of those SDKs directly; the `instrument()` wrapper approach needs no custom trace-emission code, unlike whatbroke's JSONL event format.

## Architecture Notes
The three-question framing (what changed, is it real or noise, can the result be trusted) maps cleanly onto three separate mechanisms: per-case diffing against the case's own last passing run, repeated-sample statistical comparison to separate flakiness from regression, and judge calibration against human labels with reported Cohen's kappa — a more rigorous trust story than most "LLM judges the agent" tools offer.

## Maturity
Emerging: created 2026-09-24 (five weeks old), 23 stars, actively pushed (same day as this review), real published packages on both PyPI (`assay-server`, tracked downloads) and npm (`assay-evals`), single-maintainer profile so far.

## License
MIT, verified via GitHub API. No commercial-use restrictions.

## Alternatives
whatbroke (already catalogued, PROTOTYPE) covers the same core idea — diffing agent tool-call behavior between runs — via manual JSONL trace instrumentation; Assay differentiates with native pytest/Jest/Vitest integration, automatic SDK-client wrapping (no custom instrumentation needed), statistical noise-vs-regression separation, and judge-calibration against human labels. promptfoo/LangSmith remain the output-quality-scoring alternative, which both whatbroke and Assay explicitly position themselves against rather than replace.

## Risks / Limitations
Single-maintainer, no established production user base yet beyond its own demos. Overlaps enough with whatbroke that running both would be redundant — worth picking one rather than adopting both; Assay's native-SDK-wrap integration is the likely tie-breaker for a TypeScript-first stack.

## Recommendation
PROTOTYPE — try it on one existing agent/support-bot test suite via the `instrument()` wrapper before deciding whether it replaces or complements whatbroke; too early and too close to an existing catalog entry for USE NOW.

## Change History
### 2026-10-30
Initial discovery and review.
