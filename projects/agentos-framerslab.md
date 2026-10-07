# AgentOS (framerslab)

## Summary
AgentOS is a TypeScript agent framework providing persistent "cognitive memory," runtime
tool forging (agents can create their own tools at runtime), multi-agent orchestration,
an optional HEXACO personality model, and one dispatch interface across 11 LLM providers.
Distributed as an npm package (`@framers/agentos`), Apache-2.0 licensed. Not to be
confused with the already-catalogued agentOS (Rivet) — same generic name, unrelated repo
and architecture.

## Why I Should Care
This is a library to embed, not a CLI tool to run alongside an agent — it's aimed
directly at building agent products, which fits the MERN/TypeScript stack better than
most agent frameworks surfaced so far (which tend to be Python-first).

## Problems It Can Remove
- Building custom memory persistence, multi-provider LLM dispatch, and multi-agent
  coordination from scratch when building an internal or commercial agent product.
- Vendor lock-in to a single LLM provider's SDK — one dispatch interface across 11
  providers means swapping models without rewriting the agent's control logic.

## Practical Uses
- As the backbone of an internal tool or a commercial product feature that needs an
  agent with memory that persists across sessions, not just a single-shot LLM call.
- Studying the "runtime tool forging" mechanism (agents authoring their own tools on the
  fly) as a pattern, even if not adopting the whole framework.
- Multi-agent orchestration for a workflow that needs more than one specialized agent
  coordinating on a task.

## Product Opportunities
A TypeScript, embeddable agent framework with persistent memory is directly reusable as
the agent layer inside a commercial product — e.g. an agent-powered feature inside an
existing SaaS, where a Python framework would mean a second runtime/language to operate.

## Agent / Automation Opportunities
Framework-level: it's the thing you'd build an MCP server, CLI, or agent-tool *on top
of*, rather than something that itself exposes an MCP surface.

## Integration
`npm install @framers/agentos`. Integration effort: **Low-to-Medium** — straightforward
as a library import, but adopting its memory and orchestration abstractions is a real
architectural decision, not a config toggle.

## Architecture Notes
NVIDIA Inception program membership is a credibility signal (vetted application
process, not a self-granted badge). The repo advertises an 85.6% score on LongMemEval-S,
a published long-term-memory benchmark — this is a real external benchmark name, but the
specific score is self-reported and was not independently reproduced here. The HEXACO
personality model (an academic personality-trait framework) applied to agent behavior is
an unusual, worth-studying design choice rather than a typical "system prompt with
persona" hack.

## Maturity
Emerging: created 2025-10-28 (~1 year old), 676 stars, 98 forks, current npm/GitHub
release v0.10.37 (2026-10-07, versions match exactly between npm and GitHub), CI +
Codecov configured. Pre-1.0 — treat API surface as still settling.

## License
Apache-2.0. No commercial-use restrictions; embeddable in a commercial product.

## Alternatives
- agentOS (Rivet, already catalogued) — unrelated despite the name collision; Rivet's
  agentOS is about in-process sandboxed execution/cold-start speed, not agent memory.
- Microsoft Agent Framework, Google ADK (both previously evaluated/mainstream-adjacent) —
  heavier, Python/.NET-first; AgentOS is lighter and TypeScript-native.
- Mastra, LangGraph.js (not deep-reviewed this run) — closest direct TS-ecosystem
  competitors; worth a head-to-head comparison before committing if evaluated further.

## Risks / Limitations
- Pre-1.0, solo-org-maintained (framerslab) — API stability and long-term maintenance
  commitment are unproven.
- Benchmark claims (LongMemEval-S 85.6%) are self-reported; verify independently before
  citing them in any decision.
- Name collision with the unrelated, already-catalogued agentOS (Rivet) risks confusion
  in notes/search — always disambiguate by repo URL.

## Recommendation
**PROTOTYPE** — worth a small spike building one agent feature on it to evaluate the
memory/tool-forging abstractions directly, before considering it for anything
commercial given the pre-1.0 maturity.

## Change History
### 2026-10-07
Initial discovery and review. Rotation slot 1 (AI agents, MCP, coding productivity).
