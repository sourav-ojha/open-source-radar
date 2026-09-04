# AgentInspect

## Summary
Local-first evidence and trajectory-testing toolkit for TypeScript AI agents. Turns an agent run into a readable execution tree (tool calls, LLM calls, retries, first causal failure), lets CI fail on the wrong trajectory via deterministic trace-contract checks, and can bundle a redacted, hash-verified evidence artifact to attach to a PR or incident — without an account, collector, or default upload.

## Why I Should Care
Custom agent code rarely fails as a single function call — it plans, retrieves, calls tools, invokes a model, retries, and produces side effects, and flat logs only show fragments. This is directly relevant if/when building a custom agent product on the AI SDK, LangGraph, or OpenAI Agents JS rather than only using off-the-shelf coding agents.

## Problems It Can Remove
- Eyeballing raw console logs to figure out where and why a custom agent run went wrong.
- Ad hoc, non-deterministic "does this agent still work" checks in CI.
- Needing a hosted tracing account (LangSmith/LangFuse-style) just to debug locally.

## Practical Uses
- Debug a failing custom TS agent run locally, from the same trace used for CI and sharing.
- Gate CI on deterministic trajectory checks (required tool calls, no failed observations) instead of eyeballing logs.
- Produce a redacted, hash-verified evidence bundle for a PR, incident, or support handoff.
- Adopt framework adapters (LangChain/LangGraph, Vercel AI SDK, OpenAI Agents JS) if building an agent product on one of those stacks.

## Product Opportunities
Evidence bundles are a plausible support/incident-response artifact for a customer-facing agent product — "here is exactly what the agent did and why" without shipping raw logs.

## Agent / Automation Opportunities
Exposes a `TraceContract` API for programmatic, bounded trajectory analysis an orchestrator could call directly in its own CI gate, plus reporters for Vitest/Jest.

## Integration
`npm install agent-inspect`, then instrument manually, via official framework adapters, or via log-to-tree/OpenInference/OTLP JSON adapters for existing structured logs. Medium effort — meaningfully useful requires actually instrumenting an agent, unlike a drop-in CLI.

## Architecture Notes
Everything runs from local JSONL traces; `view`/`report`/`explain`/`check`/`verify-safe`/`bundle` all read the same captured trace so debug, CI-gate, and share workflows share one source of truth instead of three separate integrations.

## Maturity
Emerging. 499 stars, pushed same-day, created May 2026, 16 contributors, 958 commits. Extensive docs (support-levels: Stable/Supported/Beta/Preview/Experimental surfaces called out explicitly) and framework-specific adapter packages.

## License
MIT. No restrictions on commercial use, redistribution, or embedding.

## Alternatives
Hosted tracing platforms like LangSmith/LangFuse (require an account/upload; catalogued Laminar is a similar hosted-adjacent alternative); `console.log` plus eyeballing agent output (status quo).

## Risks / Limitations
- Only useful once actively building a custom TS agent — not relevant to using off-the-shelf coding agents directly.
- Created May 2026; adapters for fast-moving upstream SDKs (LangGraph, AI SDK) need to keep pace.
- npm package is already at major version 6 for a young project — worth understanding the versioning scheme before pinning.

## Recommendation
PROTOTYPE — worth adopting the moment a custom TS agent product is actually being built; not useful as a general coding-agent tool today.

## Change History
### 2026-09-04
Discovered and reviewed. GitHub API verified: MIT, 499 stars, pushed_at 2026-09-04 (same day), created 2026-05-02, 16 contributors, 958 commits. npm confirms agent-inspect@6.17.6, MIT.
