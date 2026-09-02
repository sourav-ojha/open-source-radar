# Benjamin-Plus Skill

## Summary
A JetBrains-authored, benchmark-validated coding-agent skill that teaches five token-efficiency habits (consolidate lookups, read only necessary file portions, check dependencies once, rely on task-defined verification, poll builds sparingly). Injected as a system-prompt/hook payload rather than 'installed' as a tool.

## Why I Should Care
Directly reduces the per-session cost of the coding-agent workflows Sourav already runs daily, with an actual paired A/B benchmark (80 SkillsBench tasks, Claude Sonnet 5, -17.9% median cost, p=0.003 on cross-platform Java tasks) rather than a marketing claim.

## Problems It Can Remove
The specific habits and their measured effect size aren't obvious without running the same kind of A/B benchmark JetBrains did; copying a validated ~3KB payload costs nothing.

## Practical Uses
- Cut token spend on every Claude Code / Codex CLI session without changing workflow
- Reduce cost on long agentic sessions (large refactors, multi-file features) where redundant re-reads compound
- Baseline efficiency layer to stack under project-specific skills

## Product Opportunities
- Bundle into an internal 'agent starter kit' AGENTS.md for all of Sourav's repos

## Agent / Automation Opportunities
- Direct hook into Claude Code settings.json; works with any agent that accepts system-prompt or AGENTS.md injection

## Integration
Deployment: system-prompt injection, Claude Code hook, AGENTS.md snippet
Interfaces: skill/prompt payload (~3KB)
Integration effort: **Low**

## Architecture Notes
GitHub API verified: MIT, 303 stars, pushed_at 2026-08-27, created 2026-08-17, not archived.

## Maturity
Emerging. GitHub API verified: MIT, 303 stars, pushed_at 2026-08-27, created 2026-08-17, not archived.

## License
MIT. MIT, no restrictions.

## Alternatives
- Ad-hoc personal prompt tweaks (unmeasured, no evidence of effect)
- Various unvalidated 'prompt engineering' skill repos with no benchmark backing

## Risks / Limitations
- Very young repo (2 weeks old at review) — re-check in a month that JetBrains keeps maintaining it
- Benchmarked only on Claude Sonnet 5 and Codex CLI; effect size on other agents/models unverified
- Injection-only distribution means no version pinning via package manager — must track the file manually

## Recommendation
**USE NOW** (score 8.4/10) — see Why I Should Care above.

## Change History
### 2026-09-02
First discovered and reviewed. Verified via GitHub API: license MIT, current version unclear-no-tagged-release (unclear).
