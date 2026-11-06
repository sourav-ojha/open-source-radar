# codeburn

## Summary
A free, local-first tracker of AI coding-agent token usage and cost across 37+ tools (Claude Code, Cursor, Codex, Gemini CLI, and others), broken down by model, project and task. Runs via `npx codeburn` or as a native desktop app (macOS, Windows, Linux — Microsoft Store and Homebrew distributed). A credited Claude/Codex open-source grant recipient with a genuine Product Hunt launch.

## Why I Should Care
Coding-agent spend is scattered across however many CLIs and IDE plugins get used, and provider billing dashboards only show it in aggregate, a month late. codeburn reads local usage/session data from each tool and gives a per-project, per-model, per-task breakdown immediately, with zero backend to stand up.

## Problems It Can Remove
- Waiting for a monthly provider invoice to find out which project or model burned the budget.
- Manually cross-referencing usage across multiple agent CLIs/IDE extensions with different (or no) built-in cost reporting.

## Practical Uses
- Spot a runaway-cost session (wrong model selected, oversized context) while it's happening rather than after the bill arrives.
- Compare actual Claude Code vs Cursor vs Codex spend across side projects to decide where to standardize.
- Quick personal equivalent of the already-catalogued Agent Console, without deploying a team dashboard.

## Product Opportunities
None directly for his own products — this is a personal/team tooling play, not a building block.

## Agent / Automation Opportunities
The local data files it parses are plain and could in principle be read by a skill to let an agent self-report its own spend inside a session, though codeburn itself is a human-facing dashboard, not an agent-callable tool.

## Integration
`npx codeburn` for a zero-install run, or `npm install -g codeburn` / Homebrew / Microsoft Store for a persistent install. Low integration effort, no configuration or backend required.

## Architecture Notes
Nothing unusual — the value is the breadth of the integration surface (37+ tools' local usage-log formats normalized into one cost model), not a novel architecture.

## Maturity
Emerging-to-mature for a 7-month-old project: created 2026-04-13, 11,704 stars, 910 forks, 33 subscribers, 30 open issues, active releases across macOS/Windows/Linux, Discord community, and an official Claude/Codex open-source grant credit.

## License
MIT — no restrictions.

## Alternatives
Agent Console (already catalogued — team-oriented dashboard, needs deployment), ccusage (narrower, Claude-Code-specific CLI), manually cross-referencing provider billing dashboards.

## Risks / Limitations
- Stars-to-subscribers ratio (~355:1) is elevated against our running fake-engagement heuristic, but it pairs with real fork activity (910), real open-issue activity (30), a verified Anthropic/OpenAI open-source grant credit, and a genuine Product Hunt launch — reads as organic traction from a real cross-platform app, not inflated engagement. Flagged for visibility, not treated as disqualifying.
- Local-only, single-machine view; doesn't give the multi-seat rollup that Agent Console is built for.

## Recommendation
USE NOW — zero-risk to try (`npx codeburn`, no account, no backend), and immediately useful for anyone running more than one coding-agent tool.

## Change History
### 2026-11-06
Discovered and reviewed. GitHub API verified: MIT, 11,704 stars/910 forks/33 subscribers, created 2026-04-13, pushed 2026-11-06, latest GitHub release mac-v0.9.27 matching npm 0.9.27.
