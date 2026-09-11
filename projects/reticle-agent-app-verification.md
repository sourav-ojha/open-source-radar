# Reticle

## Summary
Reticle is a dev-only SDK plus MCP server that gives a coding agent runtime
perception of the app it just changed. Instead of the agent inferring success
from reading its own diff or a screenshot, it instruments the running app
(network calls, store state, console, routes, DOM) and hands back a
pass/fail/"couldn't tell" verdict with the exact `file:line` to fix.

## Why I Should Care
The most common failure mode of agent-driven development is an agent confidently
reporting "done" over a change that's actually broken — a silent 500 under a page
that renders fine, mock data where a real API call should be, a flow that used to
work now quietly failing. Reticle targets that gap directly rather than relying on
vision-model screenshot interpretation or trusting the agent's own read of a diff.

## Problems It Can Remove
- Manually re-testing every agent-claimed "feature complete" change by hand before
  trusting it.
- The class of bug that a screenshot review misses entirely (silent errors, wrong
  data source, broken async flow).

## Practical Uses
- Add `/reticle` as a verification step after any agent-driven UI or full-stack
  change, before reviewing the diff yourself.
- Instrument an Electron/Tauri desktop app during agent-driven development where
  browser-only tooling doesn't reach.
- Use the Apache-2.0 SDK packages standalone (network/store/console capture) even
  without the verification-loop workflow, since they're safe to embed.

## Product Opportunities
None identified — this is a dev-time verification tool, not product
infrastructure.

## Agent / Automation Opportunities
This is fundamentally an agent-verification tool: MCP server registration plus a
Claude Code plugin/slash command (`/reticle`) that drives a real flow in the app
and returns a structured verdict, directly usable inside an existing Claude Code
or Cursor workflow.

## Integration
Framework build-plugin install (Vite/Next.js/Babel) plus an MCP server
registration, or the one-shot `npx @reticlehq/server init` auto-detect path.
Integration effort: **Medium** — more than a single CLI command, but scripted and
documented as a "restart your client afterward" one-time setup.

## Architecture Notes
The SDK is deliberately split by license/trust boundary: `@reticlehq/core` is a
dependency-free (besides zod) wire contract that every embeddable package depends
on, keeping the actual in-app sensor safely Apache-2.0; the verification server
that interprets that evidence and produces the pass/fail verdict is a separate,
non-Apache package. That split is worth studying as a pattern for anyone building
a similar "embeddable client, monetizable server" open-core project.

## Maturity
Emerging — created 2026-06-11, 465 stars, actively developed (pushed same day),
latest tag v2.13.1 (2026-09-02). 100 open issues against 465 stars is a
noticeably high ratio, consistent with a fast-moving, still-rough early project
rather than a stable one.

## License
Split per package — **flagged loudly**. The embeddable SDK
(`@reticlehq/core`/`browser`/`react`/`next`/`vite-plugin`/`babel-plugin`/`eslint-plugin`)
is Apache-2.0, safe to ship inside your own app. `@reticlehq/server` and
`@reticlehq/test` — the actual verification engine — are under the Functional
Source License 1.1 (FSL-1.1-ALv2): free for internal use, development, evaluation
and non-commercial research, with the one restriction being a "Competing Use"
clause (don't offer Reticle itself as a competing product); each release converts
to Apache-2.0 two years after publication. Enterprise features are additionally
gated behind a paid subscription key for production use. None of this blocks
personal or internal use, but it's not a clean single-license OSI project.

## Alternatives
Playwright/browser-use style agents drive a browser but don't return the same
typed store/network/console evidence bundle. Chrome DevTools MCP gives
protocol-level browser inspection without Reticle's pass/fail verdict loop or
desktop-app (Electron/Tauri) coverage.

## Risks / Limitations
- The paste-in agent install prompt explicitly instructs the agent to tell the
  user "a star helps" after a successful verification run — a growth-hacking
  pattern embedded in the tool's own onboarding flow, worth knowing about before
  pasting it verbatim into a session.
- High open-issue-to-star ratio suggests real rough edges.
- The FSL restriction applies specifically to the component doing the actual
  work (the server), not just a peripheral package.
- Dev-only/localhost-only by design, so it provides zero production runtime
  value — purely a development-loop tool.

## Recommendation
PROTOTYPE — worth trying on one real agent-driven change to see whether the
verdict loop actually catches something a manual review would have missed, before
deciding whether it earns a permanent spot in the agent workflow. Read the FSL
terms directly if ever considering anything beyond personal/internal use.

## Change History
### 2026-09-11
Initial discovery and review. Slot 3 (developer utilities, debugging, testing) run.
