# solidtime

## Summary
A modern, self-hosted time-tracking application for freelancers and agencies — projects, tasks, clients, billable rates, multi-organization support, and import from Toggl/Clockify/CSV. Laravel (PHP) backend, Vue/TypeScript frontend, AGPL-3.0, with an optional hosted cloud from the same team.

## Why I Should Care
Running an MSSP admin-portal SaaS and a PTaaS partnership means billable time and client-rate tracking are a real, recurring need, not a hypothetical one. This replaces a Toggl/Clockify subscription with a self-hosted app that already has import paths off those exact tools, so switching cost is low.

## Problems It Can Remove
- Recurring subscription cost for Toggl/Clockify/Timely for freelance or agency-style time tracking.
- Manually tracking billable rates per client/project/organization member in a spreadsheet.
- Migrating time-tracking history when leaving a SaaS tool — direct import support exists for the two most common incumbents.

## Practical Uses
- Track billable hours across MSSP/PTaaS client engagements with per-client, per-project billable rates.
- Multi-organization support if running more than one client-facing engagement stream under one account.
- Import existing Toggl or Clockify history rather than starting from zero.

## Product Opportunities
Not a strong fit for embedding into another product (it's a standalone Laravel app, not a library), but the billable-rate model (rate can be set at project, member, or organization level, with clear precedence) is a reusable data-modeling idea for a billing feature on his own MSSP portal.

## Agent / Automation Opportunities
No MCP server or explicit agent-tool support found. It exposes a documented API (Laravel-based, referenced in its docs site) that could be scripted against for automated invoicing/reporting, but this is a manual-integration task, not an out-of-the-box agent capability.

## Integration
Self-hosting guides and an examples repo are published (`solidtime-io/self-hosting-examples`), or use their hosted cloud instead. Needs a standard Laravel stack (PHP, a database, queue worker) — heavier to operate than a single static binary. **Integration effort: Medium** — real self-hosting effort for a PHP/Laravel app outside the primary Node/TypeScript stack, though it's used as a standalone tool rather than something to integrate at the code level.

## Architecture Notes
Laravel + Inertia-style Vue frontend, PHPStan at level 7 (strict static analysis) and Codecov-tracked test coverage — signals of a codebase held to real engineering standards rather than a fast-and-loose side project. Contribution policy requires being "vouched" before larger PRs are accepted, including an explicit ban on unreviewed AI-generated ("AI slop") pull requests — a governance detail worth noting given how much of this cohort's fast-moving repos lean the opposite way.

## Maturity
Established. Created 2024-01-16 (~20 months), 8,928 stars, 508 forks (healthy ~5.7% ratio), latest release v0.20.1 (2026-09-16), pushed_at 2026-09-17. Pre-1.0 versioning despite the star count and age, worth noting before treating it as fully stable.

## License
**AGPL-3.0 — flagged loudly.** Self-hosting for internal/own-business use (tracking your own team's billable time) is unaffected. Would matter only if reselling a modified version as a hosted service to third parties.

## Alternatives
Kimai (`kimai/kimai`, 5,012 stars, AGPL-3.0) — the more established "#1 open-source time-tracking application," already long-running and arguably already known given its maturity; not treated as a new discovery here. Toggl, Clockify — proprietary SaaS incumbents this directly targets via import support. solidtime's distinguishing pitch is a more modern UI/UX and multi-organization model versus Kimai's older, more enterprise-oriented interface — worth a side-by-side trial rather than taking either project's framing at face value.

## Risks / Limitations
- Pre-1.0 versioning (v0.20.1) despite real usage — check the changelog for breaking changes before relying on it for invoicing-critical data.
- PHP/Laravel stack is outside the primary Node/TypeScript toolchain, so it's a standalone tool to run, not something to extend in-code without picking up Laravel.
- AGPL-3.0 (see above).

## Recommendation
PROTOTYPE — trial it against one real client engagement's time tracking before migrating fully off an existing tool, given the pre-1.0 version and the operational shift to a PHP/Laravel stack.

## Change History
### 2026-09-20
Discovered and reviewed. GitHub API verified: AGPL-3.0, 8,928 stars, 508 forks, created 2024-01-16, latest release v0.20.1 (2026-09-16), archived: false.
