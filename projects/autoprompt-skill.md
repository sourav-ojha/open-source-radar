# Autoprompt Skill

## Summary
A coding-agent skill that replaces step-by-step prompting with a single clear goal, running a plan -> build -> review -> test -> sign-off -> sweep loop where separate sub-agents plan, execute, and independently verify the work (preventing self-approval bias). Supports 9 coding agents including Claude Code, Codex, and OpenCode.

## Why I Should Care
Backed by a real benchmark (Terminal-Bench 2.1: OpenCode alone solved 60/89 tasks, OpenCode+Autoprompt solved 73/89, a 45% reduction in failures) rather than an unverified claim, and the plan/execute/verify separation is a pattern worth adopting even without using this exact skill.

## Problems It Can Remove
The specific separation-of-concerns prompt structure and its measured effect required a real benchmark run most people won't do themselves before trusting the approach.

## Practical Uses
- Hand an agent a single high-level goal for a multi-file feature instead of micromanaging steps
- Reduce silent task failures on larger agentic coding tasks where self-verification bias creeps in
- Use selectively on higher-stakes tasks given the cost/time tradeoff

## Product Opportunities
None identified beyond direct use.

## Agent / Automation Opportunities
- Drop-in skill for any of the 9 supported agent CLIs — no infrastructure to stand up

## Integration
Deployment: skill file (Node.js 20+, Python 3.11+, Git required)
Interfaces: skill payload for Claude Code, Codex, OpenCode, Kilo Code, VS Code, Prime Agent, Oh My Pi, DeepSeek Harness, Reasonix
Integration effort: **Low**

## Architecture Notes
GitHub API verified: MIT, 961 stars, created 2026-08-17, pushed_at 2026-08-30, not archived.

## Maturity
Emerging. GitHub API verified: MIT, 961 stars, created 2026-08-17, pushed_at 2026-08-30, not archived.

## License
MIT. MIT, no restrictions.

## Alternatives
- JetBrains/benjamin-plus-skill (optimizes cost, not failure rate — complementary rather than competing)
- Manual plan/execute/verify prompting done by hand each time

## Risks / Limitations
- Explicitly trades ~3x execution time and ~2x token usage for the failure-rate improvement — not worth it for small/low-stakes tasks
- 2 weeks old at review despite 961 stars — fast growth worth watching for continuity
- The 3x/2x cost figures are described as planning estimates, not measured, so treat with some caution

## Recommendation
**PROTOTYPE** (score 7.6/10) — see Why I Should Care above.

## Change History
### 2026-09-02
First discovered and reviewed. Verified via GitHub API: license MIT, current version unclear-no-tagged-release (unclear).
