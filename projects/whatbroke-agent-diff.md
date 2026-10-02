# whatbroke

## Summary
A CLI that diffs an AI agent's *behavior* between two recorded runs — which tool calls fired, what arguments they carried, cost, latency, and outputs — rather than diffing text output. Built specifically to catch the failure mode where a model swap, prompt edit, or proxy change makes an agent silently skip a side-effecting tool call while the natural-language reply still reads fine.

## Why I Should Care
Text-based evals (promptfoo, LangSmith, manual eyeballing) score output quality but can't see that `cancel_subscription` silently stopped being called while the agent kept saying "your subscription is cancelled." whatbroke's own case studies document exactly this: a 3x-smaller model swap, a same-size vendor swap, and a LiteLLM streaming route that silently dropped every tool call while still returning HTTP 200. Anyone building or iterating on tool-calling agents (coding agents, support bots, anything with real side effects) has this exact blind spot.

## Problems It Can Remove
- Manually re-testing an agent's tool-calling behavior after every model/prompt/framework change.
- The false confidence that passing text-based evals gives when the underlying tool-call behavior has actually regressed.
- Noise from normal agent non-determinism being mistaken for a real regression (it runs multiple samples per scenario and demotes findings that also flap between baseline samples to "flaky info").

## Practical Uses
- CI gate: record a baseline trace and a current trace of an agent test suite, run `whatbroke diff baseline.jsonl current.jsonl --fail-on changed`, fail the build on breaking changes.
- Model-swap checklist: before switching an agent to a new/cheaper model, diff traces to see exactly what tool-calling behavior changes.
- Debugging a proxy or gateway (e.g. LiteLLM) that may be silently altering tool-call behavior under certain settings (their own case study caught exactly this).
- Comparing prompt-engineering iterations to confirm a prompt edit didn't change tool usage as a side effect.

## Product Opportunities
This is squarely a micro-SaaS signal: "regression testing / CI gate for AI agents" as a narrow, well-defined add-on service for teams shipping tool-calling agents — a hosted version with trace storage, historical comparison, and a dashboard would be a plausible small paid product, in the same spirit as BuildPulse/Trunk but for agent behavior instead of flaky tests.

## Agent / Automation Opportunities
Pure automation/CI tool: ingest-trace → diff → exit-code-gate, designed to live in a CI pipeline (`$GITHUB_STEP_SUMMARY` markdown output is a first-class output format). The JSONL trace format is intentionally simple enough to emit from any agent framework in a short amount of custom instrumentation code.

## Integration
`npm install -g whatbroke-cli` or `npx whatbroke-cli`. No server, no account. The only integration cost is writing the few lines of code to emit JSONL trace events (`run_start`, `llm_call`, `tool_call`, `output`, `run_end`) from whatever agent framework is in use — there's no out-of-the-box instrumentation for popular frameworks yet.

## Architecture Notes
Aligns runs by id, then aligns tool calls within each run, and classifies findings into three severities (breaking / changed / info) rather than a single pass/fail — this graduated severity model plus the flaky-sample demotion logic (comparing every before-sample against every after-sample) is a more careful approach to noisy agent behavior than a naive single-run diff would be.

## Maturity
Experimental — created July 2026, 21 stars, 1 open issue, apparently single-maintainer. Documentation (README, case studies, "when to use what" doc) is unusually thorough for the repo's age, which is a positive signal, but there's no track record yet.

## License
MIT, verified via GitHub API. No restrictions on commercial or internal use.

## Alternatives
promptfoo, LangSmith eval suites — these score output/response quality; whatbroke explicitly documents how it differs ("text diffs can't see this") and is complementary rather than a replacement. No other tool found doing structured tool-call-level behavioral diffing between two agent runs.

## Risks / Limitations
Very early — single maintainer, no established user base, could stall or be abandoned. Requires custom trace instrumentation per agent framework (no adapters for LangChain/LlamaIndex/etc. yet). Worth validating the "when to use what" positioning holds up before fully relying on it instead of existing eval tooling.

## Recommendation
PROTOTYPE — worth testing with real trace instrumentation the next time a model or prompt change is being considered for an agent-based project; too early for USE NOW given the single-maintainer/early-stage profile.

## Change History
### 2026-10-02
Initial discovery and review.
