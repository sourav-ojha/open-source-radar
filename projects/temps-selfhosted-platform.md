# Temps

## Summary
Temps is a self-hosted, AI-native alternative to a stack of paid SaaS products — Vercel
(deploys), Sentry (error tracking), PostHog (analytics/session replay), Pingdom (uptime),
Resend (email), E2B (sandboxes), and an OpenAI-compatible AI gateway — shipped as a single
Rust binary with 440+ CLI operations and matching skills for coding agents.

## Why I Should Care
It targets the exact "replace paid SaaS with something self-hosted" axis directly, and
does it across seven categories at once instead of one. If even half of the claimed
surface works as documented, it removes several recurring SaaS bills and gives an agent
a single CLI to deploy, check traces, and diagnose incidents without touching six
different dashboards.

## Problems It Can Remove
- Paying separately for hosting/deploys, error tracking, product analytics + session
  replay, uptime monitoring, transactional email, and code-sandbox execution.
- Building custom glue to let an agent inspect deployment/error/analytics state — Temps
  exposes all of it as CLI commands and ships the agent skills for Claude Code, Codex,
  and OpenCode already.

## Practical Uses
- Self-host it for a side project or client project instead of paying for Vercel +
  Sentry + PostHog separately.
- Let a coding agent run `temps deploy`, `temps analytics`, or query traces directly via
  its own skill files rather than screen-scraping dashboards.
- Use the built-in AI gateway to front OpenAI/Anthropic/xAI/Gemini calls with per-model
  cost/latency/error attribution, without standing up a separate gateway.
- Use workflow sandboxes to run Claude Code/Codex/OpenCode against a repo from inside
  the platform itself.

## Product Opportunities
- Could anchor a "batteries-included self-hosted dev platform" offering for clients who
  want Vercel-like DX without the per-seat SaaS bill.
- The AI-chat-grounded-in-your-own-telemetry angle (answers sourced from real traces/
  metrics/revenue, not a generic model guess) is a reusable pattern for any ops product.

## Agent / Automation Opportunities
- 440+ CLI operations map cleanly to agent tool calls; ships `.claude/skills`-compatible
  skill files out of the box.
- MCP servers are injected automatically into workflow sandboxes that run coding agents
  against a project's repo.

## Integration
Installed via `curl -fsSL https://temps.sh/deploy.sh | bash` as a self-hosted single Rust
binary; CLI also available as an npm package (`@temps-sdk/cli`, npm-published 0.1.36 as
of 2026-08-30). Docker-based self-hosting implied by the deploy script; full Kubernetes/
cloud deployment story is documented on temps.sh/docs but not independently verified here.
Integration effort: **Medium** — single-binary install is simple, but the scope (deploys,
analytics, email, sandboxes) means real adoption means migrating several existing
integrations at once, not a drop-in library.

## Architecture Notes
Single self-hosted Rust binary fronting all subsystems, with an OpenAI-compatible AI
gateway and an "AI chat grounded in your own telemetry" feature that is read-only by
default (write actions require explicit confirmation) — a reasonable safety default worth
copying in any internal agent-facing ops tool.

## Maturity
Emerging. GitHub's own "latest release" tag (v0.0.8, tagged 2026-03-31) is stale and
misleading — the project actually ships nightly builds (tags like
`v0.1.0-nightly.20260908.*`) daily, with real commit activity through the day of this
review (multiple merged PRs on 2026-09-08/09) and an actively-published npm CLI at
0.1.36. Ten human/bot contributors, one clearly dominant (1769 of the visible commits).
**Note for future runs: track the npm package version or nightly tags, not GitHub
"releases," for this repo — the releases/latest endpoint understates real activity.**

## License
Apache-2.0 — no commercial-use restrictions identified.

## Alternatives
Vercel, Sentry, PostHog, Pingdom/UptimeRobot, Resend, E2B — the seven-tool bundle it
explicitly targets, plus narrower self-hosted single-purpose alternatives (Plausible/
Umami for analytics, Highlight.io for session replay + errors, LiteLLM for the AI
gateway piece alone).

## Why This One
Most self-hosted alternatives replace one SaaS product. Temps replaces seven from one
binary with a shared CLI and shared agent-skill surface, which is a materially different
integration story than running seven separate self-hosted tools side by side — at the
cost of trusting one project for all seven instead of spreading the risk.

## Risks / Limitations
- Broad surface area increases the blast radius of any one subsystem being immature —
  each of the seven replaced categories deserves independent verification before trusting
  it in place of the SaaS incumbent it replaces.
- CLI is still pre-1.0 (0.1.x); breaking changes should be expected.
- GitHub's release tags do not reflect actual release cadence (see Maturity) — verify
  against nightlies or the npm package before assuming a stale project.

## Recommendation
PROTOTYPE — worth standing up for a low-stakes side project to test the deploy +
error-tracking + analytics path specifically, before trusting it for anything
production-critical.

## Change History
### 2026-09-09
Initial discovery and review. Slot 1 (AI agents, MCP, coding productivity) run.
