# OpenJudge

## Summary
OpenJudge is Alibaba's AgentScope-team open-source evaluation framework for AI agents and
applications: a library of 50+ production-ready graders spanning general quality, agent
behavior (tool selection, trajectory, memory, plan feasibility), multimodal output, and a
dedicated Agent-Skill security-grader taxonomy, with a Streamlit UI and public benchmarks
(PawBench, Reference Hallucination Arena) backing its grader accuracy claims.

## Why I Should Care
LLM-as-judge is easy to prototype and easy to get subtly wrong (biased graders, no ground
truth check). OpenJudge packages validated graders for the specific dimensions that matter
for agents — tool-selection accuracy, trajectory quality, memory/context preservation — and
publishes benchmarks showing the graders actually correlate with outcomes, rather than just
asserting it.

## Problems It Can Remove
Replaces hand-rolled LLM-as-judge prompts and ad hoc eval scripts for agent QA. Removes the
need to separately build a security/threat-analysis pass for Agent Skills — OpenJudge ships
Skill Graders (threat analysis against an AITech taxonomy, declaration alignment,
completeness, relevance, design quality) as one of its grader families.

## Practical Uses
- Grade agent trajectories, tool-selection accuracy, and memory/context preservation for an
  internal agent before shipping it.
- Run a Skill Grader pass over Claude/Codex Agent Skills before installing them, as a second
  opinion alongside a dedicated MCP/skill security scanner.
- Score RAG or chatbot output quality (relevance, similarity, JSON/structure compliance)
  without hand-writing eval scripts.
- Benchmark competing models/harnesses on a custom task set, the same way PawBench does.

## Product Opportunities
An internal QA gate for an AI feature inside a commercial product, reusing a maintained
grader library instead of hand-rolled LLM-as-judge prompts.

## Agent / Automation Opportunities
Skill Graders are a ready-made second opinion to pair with SkillSpector or Cisco's
mcp-scanner (both catalogued) on Agent Skill / MCP server vetting — three independent lenses
on the same trust problem.

## Integration
`pip install py-openjudge`. Python SDK plus CLI and an optional Streamlit UI (local or
hosted at openjudge.me). Low integration effort — it's a library, not infrastructure to run.

## Architecture Notes
Grader taxonomy is organized by domain (General / Agent / Multimodal) rather than by
technique, which keeps the API surface predictable as new graders are added. The project
treats its own grader accuracy as a thing to prove (PawBench: 150 tasks × 9 models × 3
harnesses with public prompts and leaderboard; Reference Hallucination Arena) rather than
asserting it, which is the right instinct for anything calling itself a "judge."

## Maturity
Emerging. ~8 months old (created 2025-07-08), Alibaba AgentScope-team backed, weekly release
cadence (v0.2.5 as of 2026-11-10), real docs site and hosted try-online app.

## License
Apache-2.0. No restrictions on commercial or internal use.

## Alternatives
promptfoo / DeepEval / RAGAS (general LLM eval, less agent-trajectory-specific). SkillSpector
and Cisco's mcp-scanner (security-specific scanners vs. this broader quality-eval framework
with a security-adjacent grader). Hand-written LLM-as-judge prompts (status quo).

## Risks / Limitations
Young org-backed project; API surface may still shift (v0.2.x). Crowded LLM-eval space — the
differentiation is the agent/skill-specific graders, not raw eval mechanics. Elevated
stars:subscribers ratio (~272:1, 1087 stars / 4 subscribers) flagged per the running
fake-engagement heuristic, but paired with real fork activity (103 forks) and a credible,
identifiable org, so not treated as disqualifying.

## Recommendation
PROTOTYPE — worth trying against a real internal agent's trajectories/tool-selection output
to see whether the graders' judgments match manual review before relying on them for a gate.

## Change History
### 2026-11-11
Initial discovery and review. GitHub API verified: Apache-2.0, 1087 stars/103 forks/4
subscribers, created 2025-07-08, pushed 2026-11-10, not archived. Current release v0.2.5.
