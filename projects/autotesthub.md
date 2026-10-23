# AutotestHub

## Summary
A self-hosted regression-testing framework with a web console covering three channels — API execution plans, browser tests (pytest + Playwright), and AI agent conversation/tool-call evaluation — unified around one workflow: observe a failure, confirm the expected behavior, publish an immutable case revision, run it, and compare against a fixed baseline.

## Why I Should Care
AI-agent regression testing is a gap most testing tools don't address: once a product has an agent in the loop, "did this change break the agent's behavior" is a different question from "did this change break an API response," and AutotestHub's workflow — starting from an observed agent failure rather than a hand-written test case — matches how agent bugs actually get found in practice.

## Problems It Can Remove
Replaces the ad-hoc practice of eyeballing agent conversation logs after a prompt or tool change, with no record of what "correct" was supposed to look like. The immutable-baseline model also removes the usual regression-suite rot where a baseline silently drifts because someone edited the expected result instead of re-confirming it.

## Practical Uses
- Capture a failed AI agent session, confirm the correct behavior, and get an automatic baseline comparison on every subsequent prompt/model change.
- Run API execution plans and Playwright browser tests from the same console, with shared regression batches that let an API fix and its UI confirmation sit in one reviewable run.
- Use the CLI to retrieve comparison results for a CI gate rather than reviewing every run in the web console.
- Apply it to the PTaaS/MSSP admin-portal context to track behavior of any AI-assisted triage or reporting features as prompts change.

## Product Opportunities
The "observe → confirm → publish → run → compare" workflow is a reusable pattern worth borrowing even without adopting the full tool — any product that ships agent-facing features eventually needs an equivalent regression-evidence trail.

## Agent / Automation Opportunities
Purpose-built for exactly this: AI agent evaluation is a first-class test channel, not bolted on, with session traces, confirmed rules, and judge profiles as the artifacts a baseline comparison is built from.

## Integration
Self-hosted via Docker Compose (Postgres, Redis, scheduler, three execution workers) behind a Python 3.11+ init script; a CLI exists for retrieving results outside the web console. Medium integration effort — this is a real multi-service stack to stand up, not a single binary, and HTTPS/network access need separate configuration before any shared deployment.

## Architecture Notes
The deliberate separation of "publish an immutable case revision" from "run and compare" is the architecturally interesting part: it forces a human decision point (confirm expected behavior) before a case becomes something future runs are graded against, which prevents the common regression-suite failure mode of a baseline being edited to match whatever the code currently does.

## Maturity
Emerging: created 2026-10-09, pushed 2026-10-15, 21 stars, 0 forks, 0 releases, single contributor (19 commits). Zero forks on 21 stars and no tagged release are signs this hasn't yet been tried outside its own author's use, despite a substantive README and architecture doc.

## License
MIT — unrestricted commercial use and embedding.

## Alternatives
- **Playwright** / **pytest** directly — what this wraps for the browser channel; AutotestHub adds the console, baselines, and agent-channel on top.
- Ad-hoc eval harnesses (LangSmith, custom scripts) — cover AI-agent evaluation but without the unified API/browser/agent regression workflow.
- **vibecheck** (already catalogued) — adjacent but different: an LLM-graded PR comprehension gate, not a regression-testing framework.

## Risks / Limitations
- Single contributor, zero forks, zero releases — treat as unproven until it sees outside use.
- Python/Postgres/Redis stack is a heavier footprint than the JS/TS-first bias generally prefers; worth it only if the AI-agent regression channel specifically is needed.
- No evidence yet of production use beyond the project's own documentation.

## Recommendation
STUDY — the regression workflow design is worth borrowing conceptually for any agent-facing feature; the infrastructure footprint and lack of outside validation make it too early to stand up for real use yet.

## Change History
### 2026-10-23
First catalogued. Slot 3 (developer utilities, debugging, testing) discovery run. GitHub API verified: MIT, 21 stars, 0 forks, created 2026-10-09, pushed 2026-10-15, not archived, 0 releases.
