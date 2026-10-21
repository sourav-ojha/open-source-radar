# ai-coding-project-boilerplate

## Summary
A TypeScript project starter purpose-built for structured Claude Code development. `npx create-ai-project` scaffolds a TS/Node repo with Biome, Vitest, and a project-level `CLAUDE.md` pre-wired, plus a slash-command workflow: `/implement`, `/design`, `/plan`, `/build`, `/review`, `/diagnose` (with `/front-` prefixed variants for frontend work). Two modes — Normal (design + security review gates, lint/test after each task) and Lite (checks run once before review) — trade process rigor for speed.

## Why I Should Care
This is the direct, lightweight counterpart to the AWS AI-DLC Workflows project catalogued the same day: same underlying idea (structured, gated agent-driven development) but scoped to one language (TypeScript) and one harness (Claude Code), with an explicit lower-overhead mode. Since Claude Code is the tool this very radar agent runs on, and TypeScript/Node is core to the stack, this is about as direct a fit as a discovery gets.

## Problems It Can Remove
Removes the recurring setup cost of writing a `CLAUDE.md` and custom slash commands from scratch on every new TypeScript project.

## Practical Uses
- Bootstrap a new TS/Node service or micro-SaaS repo with Claude Code guardrails already wired in.
- Use Lite Mode for a fast-moving solo side project; switch to Normal Mode when a change needs a documented design + security pass first.
- Hand a half-finished design off between sessions, or to a second contributor, via the approval-gated `/design` → `/plan` split.
- `/project-inject` to record project purpose, constraints, and conventions once, so every subsequent session has that context.

## Product Opportunities
None directly — this is a project-scaffolding/workflow tool, not a product component.

## Agent / Automation Opportunities
Directly structures how Claude Code is used on a project: commands, specialized agents, and skills are added via `create-ai-project`, and `/create-skill` / `/refine-skill` support building task-specific guidance over time.

## Integration
`npx create-ai-project my-project`, `cd my-project && pnpm install`, start `claude`, run `/project-inject`. Requires Node.js 24.15+, pnpm, and Claude Code. Updates via `npx create-ai-project update` (with `--dry-run` preview), which overwrites managed Claude Code files but preserves source code and the saved workflow mode. Integration effort: **Low**.

## Architecture Notes
The Normal/Lite mode split is the most reusable idea here even without adopting the full template: an explicit, named tradeoff between "always run design + security review" and "run checks once before final review" gives a vocabulary for deciding how much process a given task actually needs.

## Maturity
Experimental-to-emerging. Created July 2025, actively maintained by a single author (667 of ~673 commits, plus dependabot), v2.2.2 released a week before review. GitHub API verified: MIT, 234 stars, 28 forks, 3 subscribers, archived:false.

## License
MIT — verified via GitHub API license field, and via npm registry for the `create-ai-project` package.

## Alternatives
- **AI-DLC Workflows** (catalogued today) — far heavier process (33 stages, 6 harnesses) aimed at team-scale work; this is the lighter, single-harness alternative.
- **MoAI-ADK** (catalogued, STUDY) — focused on Claude×GLM cost routing rather than workflow structure.

## Risks / Limitations
- Single-maintainer project — continuity risk if the author stops updating it.
- Small-scale adoption (234 stars, 3 subscribers) with no independent validation of the workflow beyond the author's own usage.
- Tightly coupled to Claude Code's plugin/command conventions — not portable to other coding agents.

## Recommendation
**PROTOTYPE** — worth trying on the next new TypeScript project as the default scaffold, specifically to evaluate whether the Normal/Lite mode split holds up in real day-to-day use.

## Change History
### 2026-10-21
Initial discovery and catalog entry. Slot 1 (AI agents, MCP, coding productivity) run.
