# roborev

## Summary
A local daemon that installs a git post-commit hook and reviews every commit a coding agent makes in the background, surfacing findings in a TUI/browser UI and feeding them straight back into the live agent session (Claude Code, Codex, Gemini CLI, Copilot CLI, Cursor, and others) via per-agent hooks. `roborev fix`/`refine` can auto-apply fixes and iterate until reviews pass.

## Why I Should Care
Most "AI code review" tools review at PR time, after an agent has already spent hours building on top of whatever it wrote. roborev reviews every commit continuously and pipes findings back into the same agent session while context is still fresh — a materially different point in the loop to intervene.

## Problems It Can Remove
Removes the need to manually re-prompt an agent to self-review its own diff, and catches duplication/complexity/dead-code/security issues before they compound across a long agentic session instead of surfacing all at once at PR time.

## Practical Uses
- `roborev init` on a repo to get automatic background review of every commit, no remote workflow required.
- `roborev refine` before opening a PR to iterate until every background review passes.
- `roborev agent-hook install` to wire findings directly back into an active Claude Code/Codex/Cursor session.
- Browser UI for browsing, filtering, and commenting on review history, plus cost/latency/reliability analytics.

## Product Opportunities
Complements Bernstein (already catalogued) rather than competing with it: Bernstein governs agent identity/credentials/audit trail at the process level, roborev reviews the actual code quality of every commit. Together they'd cover both "did the agent behave" and "is the code any good."

## Agent / Automation Opportunities
This is itself agent-loop infrastructure — the core design is closing the loop back into the coding agent's active session rather than producing a report for a human to read later.

## Integration
Single binary, installs as a git hook plus an optional per-agent hook. Runs entirely locally with no hosted service or extra infrastructure — review execution is delegated to whichever coding agent/model is already configured. Low integration effort.

## Architecture Notes
Two-layer hook design: a git post-commit hook (works with any agent, or none) plus an agent-session hook that re-surfaces open findings inside the live session of a specific supported agent. `roborev compact` re-verifies findings against current code to filter false positives and merge duplicates before they reach the agent — a sensible guard against review-noise fatigue.

## Maturity
Emerging: created 2026-01-05, 1748 stars, 165 forks (a healthy ~9.4% fork:star ratio), current release v0.71.0 (2026-10-03), pushed today. Pre-1.0 and iterating fast — expect breaking changes. 60 open issues against 11 watchers.

## License
MIT — unrestricted.

## Alternatives
- **proval** (seoes/proval, already catalogued) — self-hosted LLM PR-review agent for GitLab/Forgejo/GitHub; reviews at PR time rather than continuously per-commit.
- Manually re-prompting an agent to review its own diff — the status quo this replaces.

## Risks / Limitations
- Review quality is bounded by whichever model/agent is configured to perform the review — no independent judge model of its own.
- Fast-moving pre-1.0 project; API/behavior may shift between releases.
- Small team (11 watchers) relative to 60 open issues — verify responsiveness before relying on it for anything load-bearing.

## Recommendation
PROTOTYPE — worth wiring into one actively agent-driven repo to see whether the per-commit review signal is actually useful day to day, versus noise.

## Change History
### 2026-10-09
First catalogued. Slot 3 (developer utilities, debugging, testing) discovery run. GitHub API verified: MIT, 1748 stars, pushed today, not archived.
