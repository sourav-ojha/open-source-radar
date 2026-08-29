# Opslane

## Summary
Records user sessions, ranks the UX failures nobody's exception tracker caught (dead buttons, abandoned forms, dropdowns closing before selection) by how many users hit them, investigates the ranked ones, and opens a PR only when it can verify the fix — self-hostable.

## Why I Should Care
Most session-replay tools stop at 'here is a recording' — this one closes the loop to a verified PR, which is the actual expensive part of the workflow.

## Problems It Can Remove
The verification gate (don't open a PR unless the fix is confirmed) is the hard 20% of this idea; building and trusting that gate is a multi-week project on its own.

## Practical Uses
- Catch silent UX failures in an admin portal (relevant to OffenFlow) that never throw exceptions
- Reduce manual triage time on session-replay tools by auto-ranking failures by user impact
- Study the verify-before-PR pattern for any 'agent that opens PRs' project

## Product Opportunities
- The rank-by-impact + verify-before-PR pattern is a genuinely reusable architecture idea for any auto-remediation feature

## Agent / Automation Opportunities
- Itself an autonomous coding-agent workflow (session data → hypothesis → verified fix → PR) — worth studying the verification gate specifically

## Integration
Deployment: self-hosted, Docker (implied by self-host quickstart)
Interfaces: SDK, self-hosted web app
Integration effort: **Medium**

## Maturity
Experimental. GitHub API verified: AGPL-3.0, 36 stars, pushed_at 2026-08-29 (today). AGPL flagged per spec section 14 — loud warning, not a disqualifier.

## License
AGPL-3.0. AGPL-3.0 — network-use copyleft. Self-hosting internally is fine; embedding inside a closed-source commercial SaaS (e.g. bundled into OffenFlow) would trigger source-disclosure obligations. Treat as internal-tool-only unless a commercial license is offered.

## Alternatives
- PostHog session replay + manual triage
- Sentry + manual triage
- FullStory

## Risks / Limitations
- 36 stars, pushed same day as this review — genuinely early, expect breaking changes
- AGPL is a hard blocker for embedding in a closed commercial product
- Self-hosting a session-recording pipeline has its own privacy/compliance surface to review before pointing at real user sessions

## Recommendation
**PROTOTYPE** (score 6.8/10) — see Why I Should Care above.

## Change History
### 2026-08-29
First discovered and reviewed. Verified via GitHub API: license AGPL-3.0, current version v26.8.12 (2026-08-29).
