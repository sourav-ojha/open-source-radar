# OrcaReplay

## Summary
Records any coding agent's full execution — including shell exit codes, file writes, and the exact system prompt the harness assembled — at the process/socket boundary, then replays it byte-for-byte offline with no model called, or forks the run from any step onto a different model to compare outcomes.

## Why I Should Care
Nothing else catalogued so far gives deterministic, offline replay of a specific agent failure with the ability to fork mid-run onto a different model. It turns "why did the agent do that" from re-running and guessing into an inspectable, reproducible artifact.

## Problems It Can Remove
Removes the need to re-run a session (burning tokens, and possibly getting a different failure) just to understand what an agent did overnight — including exactly which tool call deleted a file.

## Practical Uses
- Reproduce exactly what a Claude Code or Codex session did overnight without spending tokens re-running it.
- Fork a recorded run at the step where it went wrong and re-run from there on a cheaper/different model to see if the mistake was model-specific.
- Capture and diff the actual system prompt a harness assembles for interactive vs. `-p` invocations across models, for debugging harness-level issues.
- Grade a replayed/forked run automatically (e.g. `npx tsc --noEmit`) as a lightweight eval harness for agent behavior changes.

## Product Opportunities
None directly for embedding, though the underlying trace format (published as an open spec under CC BY 4.0) could inform an internal agent-eval harness.

## Agent / Automation Opportunities
Captures below the agent, at the process/socket boundary, so it works even for agents that can't be modified or that hold their own API credentials — a structural advantage over SDK-wrapper-based observability tools that require code changes to instrument.

## Integration
`npm i -g orcareplay`; no server required for local record/replay. Low integration effort — two environment variables per the README, no SDK wrapping needed.

## Architecture Notes
An opt-in `--tls-intercept` mode captures agents with no redirectable API endpoint (e.g. a subscription-authenticated CLI talking to its own backend). The trace format is documented as an open spec separate from the tool itself.

## Maturity
Experimental — three weeks old at review (created 2026-08-29), but built by a team with a real existing commercial product (OrcaRouter, a multi-model API gateway) and already 255 stars / 65 forks.

## License
Apache-2.0 for the code; the trace spec is separately licensed CC BY 4.0. No commercial-use restrictions identified.

## Alternatives
LLM observability tools already catalogued (Langfuse-style, Opik/Comet, Laminar) tell you cost and token counts but not deterministic byte-for-byte replay or step-level model forking. The status quo — manually re-running the same prompt and hoping for the same failure — is not deterministic.

## Risks / Limitations
- Very young project; no independent long-term track record yet.
- Built and maintained by the team behind a commercial product (OrcaRouter); the free tool works standalone with your own agent/API keys today, but watch for future features being reserved for the paid router.
- The TLS-intercept capture mode is higher-trust; review exactly what it captures before enabling it on anything with production credentials.

## Recommendation
PROTOTYPE — worth a hands-on trial the next time a coding-agent session does something inexplicable; too new for USE NOW given the lack of track record, but the architecture is genuinely differentiated.

## Change History
### 2026-09-18
Initial discovery and review. Rotation slot 3 (developer utilities, debugging, testing).
