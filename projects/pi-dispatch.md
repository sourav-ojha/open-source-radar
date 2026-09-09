# pi-dispatch

## Summary
pi-dispatch is the missing operational layer for the [pi](https://github.com/earendil-works/pi)
coding agent: a self-hosted background service that runs pi on a schedule or in response
to GitHub/GitLab/Forgejo/Azure DevOps events, inside a locked-down, ephemeral container,
behind a durable queue and hard spend caps, with a live admin panel.

## Why I Should Care
"Let a coding agent work unattended without surprise bills or full machine access" is a
real, unsolved operational gap — most coding-agent CLIs (including pi, by its own
README) ship with no job queue, no concurrency control, no spend limit, and no
permission system. pi-dispatch is a narrow, well-scoped answer to exactly that gap
rather than another coding agent to evaluate.

## Problems It Can Remove
- Building a custom job queue + container sandbox + spend-cap layer around a coding
  agent to let it run unattended safely.
- Manually triggering an agent against issues/PRs instead of wiring it to forge events.
- The "no permission system" gap in the underlying agent — enforced here at the
  container boundary (`--cap-drop=ALL`, non-root, ephemeral, read-only mounted
  instructions) instead of trusting the agent's own restraint.

## Practical Uses
- Run pi as a background service that reacts to GitHub issue/comment/PR events on a
  repo, with per-repo daily/weekly/monthly spend caps enforced before any tokens are
  spent.
- Use the CLI/cron/forge-event triggers interchangeably — same queue, same sandboxed
  container, same admin panel regardless of trigger source.
- Bake a project's toolchain (including Playwright/Chromium) into the job image so a
  flow can build a frontend, screenshot it, and iterate on the rendered result
  unattended.
- Use the insights page to see what a subscription-based agent actually saves versus
  metered API pricing, and what a flow would cost on a different model.

## Product Opportunities
- Directly reusable as the operational backbone of a "coding-agent-as-a-service"
  product: durable queue, spend governance, and sandboxing are the three hardest parts
  of offering that safely to other people.
- The insights/cost-attribution page is a reusable pattern for any product billing for
  agent usage.

## Agent / Automation Opportunities
- This *is* the automation layer: turns any GitHub/GitLab/Forgejo/Azure DevOps event or
  cron schedule into a bounded, sandboxed pi run with recorded cost and outcome.
- Explicitly designed to sit underneath the agent a team already runs rather than
  compete with it.

## Integration
Self-hosted via Docker; job images are user-authored Dockerfiles baking in whatever
toolchain a flow needs. Admin panel is a live TUI/dashboard exposed by the service.
Integration effort: **Medium** — requires running a background service plus Docker image
maintenance, but the trigger/queue/budget model is well documented (`docs/costs.md`,
`docs/job-image.md`).

## Architecture Notes
Treats the container as the actual security boundary rather than trusting the agent's
own guardrails, and checks spend caps *before* a container starts rather than after —
both are the correct default for running any LLM-driven process unattended and worth
copying even if this specific tool isn't adopted.

## Maturity
Emerging. Created 2026-07-15 (~2 months old), 173 stars, MIT, active releases
(`admin-v1.10.2`, 2026-09-07). Effectively solo-authored (427 commits from one author vs.
1 from a second contributor) — real bus-factor risk despite the polish.

## License
MIT — no commercial-use restrictions.

## Alternatives
Building the same queue/sandbox/budget layer by hand around any coding-agent CLI;
general-purpose CI runners (GitHub Actions, self-hosted runners) repurposed for agent
jobs, which lack the spend-cap and per-flow cost-attribution features this ships with
natively.

## Why This One
Most "run a coding agent unattended" projects are themselves another agent to evaluate.
pi-dispatch is explicitly the opposite: it assumes you already trust pi and solves only
the queue/budget/sandbox problem around it, which is a narrower and more immediately
useful scope than most of the "autonomous coding agent" wave.

## Risks / Limitations
- Tied specifically to the `pi` coding agent; not a generic wrapper for any agent CLI.
- Effectively single-author project.
- Container-boundary security model is only as strong as the Docker sandbox itself —
  worth independent verification before trusting it with anything sensitive.

## Recommendation
PROTOTYPE — worth wiring up against a low-stakes repo with a hard spend cap to validate
the queue/budget/admin-panel claims before trusting it with anything unattended and
consequential.

## Change History
### 2026-09-09
Initial discovery and review. Slot 1 (AI agents, MCP, coding productivity) run.
