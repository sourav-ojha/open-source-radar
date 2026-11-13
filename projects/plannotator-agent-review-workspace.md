# Plannotator

## Summary
A local-first surface for reviewing and annotating what a coding agent
produces — plans, specs, markdown/HTML artifacts, and diffs/PRs — with
one-click feedback sent straight back into the agent session. It wires into
Claude Code, Codex, Copilot CLI, Gemini CLI, OpenCode, Droid, Kiro, Pi, and
Amp via each harness's own hooks, so plan-review mode opens automatically
whenever any of those agents proposes a plan.

## Why I Should Care
This is a review layer for the exact workflow this radar itself runs on —
a long Claude Code session that produces plans and diffs that need a human
decision before proceeding. Instead of reading a plan in scrollback and
typing a paragraph of corrections, Plannotator gives a dedicated annotate
surface and sends the structured feedback back to the same agent session.

## Problems It Can Remove
- Reading an agent's plan/spec in raw terminal scrollback and manually
  typing out corrections.
- Needing a different review habit per agent harness — one surface covers
  eight+ of them.
- Losing track of what was approved/rejected in past sessions (local,
  non-telemetry archive of past plan decisions).

## Practical Uses
- Reviewing a coding agent's plan before it starts implementing, annotating
  specific lines, and sending corrections back in one action.
- Reviewing a GitHub PR, GitLab MR, local uncommitted diff, or a GitButler
  workspace from the same tool regardless of which agent produced it.
- Browsing a read-only archive of past plan decisions to see what was
  approved and why.

## Product Opportunities
The project's own structure — a free local app plus a separately hosted
"Workspaces" product for team sharing — is a directly applicable open-core
pattern for a micro-SaaS built around a free local-first tool.

## Agent / Automation Opportunities
Ships native integrations for Claude Code, Codex, Copilot CLI, Gemini CLI,
OpenCode, Droid, Kiro, Pi, and Amp — usable as-is inside his existing Claude
Code workflow with no custom glue code.

## Integration
CLI/local app install; plan-review mode activates automatically through
each harness's hooks, no server required for the open-source local
workflow. **Integration effort: Low.**

## Architecture Notes
Local-storage-first design: plans, diffs, annotations, drafts, and history
stay on disk by default; the only outbound network calls are an (currently
non-optional) GitHub release-check ping, URL-annotation fetches (via Jina
Reader), and whatever provider "Ask AI" is configured against. Team sharing
is explicitly split out into a separate hosted product rather than bolted
onto the local app.

## Maturity
Created 2025-12-28 (~11 months old), Apache-2.0, 776 forks, 194 open
issues — real engagement by both counts. The star-to-subscriber ratio
(10,227 stars / 22 subscribers, ~465:1) is elevated, similar to several
other recent finds in this niche, but the fork/issue volume argues against
pure fabrication.

## License
Apache-2.0 for the open-source local app. No commercial-use restrictions.
"Workspaces" (team sharing) is a separate hosted product outside this
license.

## Alternatives
- Planning with Files (already catalogued, USE NOW) — PWF persists an
  agent's own task state across context loss/compaction; Plannotator is a
  human-facing surface for reviewing and annotating what the agent
  produces. Complementary, not competing.
- Reading agent plans/diffs directly in the terminal or in GitHub's web UI.

## Risks / Limitations
- Team-sharing direction ("Workspaces") is still described as in-progress;
  the older peer-to-peer link-sharing feature is being deprecated with no
  removal date announced.
- Checks GitHub for the latest release on every surface load with no
  opt-out setting yet (low-sensitivity, but worth knowing).
- Elevated star:subscriber ratio noted above.

## Recommendation
**PROTOTYPE** — wire it into one real Claude Code planning session and see
whether the annotate-and-send-back loop is actually faster than typing
feedback directly.

## Change History
### 2026-11-13
Initial discovery and review. Rotation slot 3 (developer utilities,
debugging, testing).
