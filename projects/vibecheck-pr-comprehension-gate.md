# vibecheck

## Summary
Self-hosted GitHub merge gate that tests whether a PR's author understands their own change before the PR can merge, rather than reviewing the code itself. On PR open, it reads the diff, generates a scoped question per ~15 changed lines (always starting with "how does this change work at a high level?"), the author answers in plain English one question at a time, and an LLM (BYOK Anthropic Claude) grades each answer against its own blind interpretation of the diff. A commit status (`vibecheck/verdict`) gates the merge via branch protection. FastAPI + Postgres, Docker Compose deploy, no GitHub App/OAuth — just a bearer-token-authenticated GitHub Action.

## Why I Should Care
As more code ships from coding agents, the bottleneck shifts from writing code to someone actually understanding it before it merges. This is a narrow, testable answer to the specific failure mode of rubber-stamped "LGTM, CI green" merges on AI-generated PRs.

## Problems It Can Remove
The diff-to-exam-sheet generation, one-question-at-a-time flow, and blind-interpretation grading loop is a small but fiddly prompt-engineering and workflow problem that this project has already built, tested (public pass/fail demo PRs in its own repo), and wired into GitHub branch protection.

## Practical Uses
- Merge gate on a team repo where AI-assisted PRs are common, to confirm the human author can explain the change before it ships
- Cheap filter for drive-by AI-generated PRs on an open-source project that nobody has read closely
- Self-check: run it on your own PRs to verify you actually understood what your coding agent produced before requesting human review

## Product Opportunities
- The "exam sheet generated from a diff, graded leniently by an LLM against its own blind read" mechanism is a reusable pattern for any internal tool that needs to verify human comprehension of AI-generated artifacts, not just code

## Agent / Automation Opportunities
- Directly targets the AI-coding-agent era problem of PRs nobody (including the nominal author) actually understands

## Integration
Deployment: Docker Compose (self-hosted only, no hosted option); a Railway deploy guide is documented.
Interfaces: GitHub Action (bearer-token webhook), FastAPI service, GitHub commit-status check.
Integration effort: **Medium** — requires hosting a Docker container plus Postgres, an Anthropic API key (BYOK), one workflow file per repo, and a branch-protection rule requiring the `vibecheck/verdict` check.

## Architecture Notes
No GitHub App or OAuth: a GitHub Action in the target repo calls the self-hosted FastAPI service over a bearer token. The grading model forms a "blind interpretation" of the diff once, independent of the author's answers, then grades each answer against that interpretation rather than against the live diff directly — intended to stop the grader from being led by a plausible-sounding but wrong answer.

## Maturity
Experimental — repo created 2026-09-12, about one month old. GitHub API verified: MIT, 5 stars, 1 fork, pushed_at 2026-10-13. Latest tagged release v0.1.0 (2026-10-12). Solo-maintainer project with a tongue-in-cheek tone in its own README about the irony of using AI to police AI-generated code.

## License
MIT, confirmed via GitHub API license field. No restrictions on commercial or internal use.

## Alternatives
- roborev (catalogued) — reviews the code itself and feeds findings back into the agent session; vibecheck instead tests the human, a distinct and complementary angle
- Generic AI PR-review bots (CodeRabbit-style) — optimize for finding bugs in the diff, not for verifying author comprehension

## Risks / Limitations
- Solo-maintainer indie project, 1 month old, 5 GitHub stars — essentially unproven outside its own repo
- BYOK Anthropic Claude only today; no multi-provider support yet per the README
- Self-hosted only (Docker + Postgres), adding real ops overhead for a single CI gate; grading quality of free-text answers at scale is unverified

## Recommendation
**PROTOTYPE** (score 7.1/10) — worth trying on a side project or low-stakes repo to see how the grading holds up before relying on it anywhere that matters.

## Change History
### 2026-10-13
First discovered and reviewed via Show HN. Verified via GitHub API: MIT, 5 stars, created 2026-09-12, latest release v0.1.0 (2026-10-12).
