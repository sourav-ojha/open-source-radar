# dif.sh

## Summary
Feature flags and A/B test definitions stored as Markdown files inside the repo, compiled by a Rust-based CLI into a typed, zero-dependency TypeScript client for Next.js/React/SvelteKit. Flag evaluation is fully self-contained — no server, no signup, no dashboard. An optional hosted "dif.sh Cloud" adds event ingest and statistical analysis for A/B experiments on top of the same files, but is explicitly not required for plain feature flags.

## Why I Should Care
Every mainstream feature-flag platform (Flagsmith, Unleash, GrowthBook) requires standing up or paying for a backend service just to gate a feature behind a boolean. For a solo/small-team MERN/Next.js project, that's disproportionate infrastructure for what is often just "ship this behind a flag, let me toggle it in a PR review." dif stores the flag as a Markdown file with YAML frontmatter, compiles it at build time into a typed client, and the flag lives and reviews like any other code change — git history is the audit log.

## Problems It Can Remove
Removes the need to run or pay for a feature-flag backend at all for simple on/off flags and basic variant rollouts, and removes the "flags nobody remembers to clean up in a dashboard" problem since each flag is a file that shows up in normal code review.

## Practical Uses
- Gate a new feature behind a flag defined as `dif/flags/new-checkout.md`, reviewed in the same PR as the code it guards.
- Ship a variant test (e.g. CTA copy) with weighted rollout, with no third-party analytics dependency if just A/B-ing copy manually.
- Let a coding agent draft and triage experiments directly via the shipped Claude Code skills (`dif-author-experiment`, `dif-triage-experiments`).
- Opt into `dif.sh Cloud` only if real statistical experiment analysis (lift, confidence, exposures) is needed later — the files remain the source of truth either way.

## Integration
```sh
npm install -g @dif.sh/cli   # or: curl -fsSL https://dif.sh/install.sh | sh
dif init && dif new my-flag --surface home
```
Generated runtime SDK (`@dif.sh/sdk`, ESM-only, zero dependencies) is imported once at app boot. Requires a `dif build` build step (recommended as a `prebuild` script) since generated files are gitignored. Integration effort: **Low** for plain flags; Cloud sign-up only needed for experiment analytics.

## Maturity
Experimental. Created 2026-05-22, latest release v0.9.0 (2026-11-23, same day as review — actively developed). 48 stars / 0 forks / 0 subscribers / 1 open issue — very thin community signal for a six-month-old project; this is effectively an unproven solo/small-team effort despite a genuinely polished CLI, CI pipeline, and npm package.

## License
MIT. No commercial-use restrictions. The optional Cloud component is a separate hosted service (cloud.dif.sh) but the README is explicit that nothing in the core CLI/SDK requires it.

## Alternatives
Flagsmith, Unleash, GrowthBook (all require a self-hosted or SaaS backend even for simple boolean flags). dif's distinguishing feature is that flag evaluation needs no backend at all — the tradeoff is giving up a real-time remote-config dashboard for anyone who actually needs runtime flag changes without a redeploy.

## Risks / Limitations
- Zero forks/subscribers — no evidence of use beyond the author yet; revisit before relying on it for anything business-critical.
- Flags are compiled at build time, not evaluated at runtime from a remote source — toggling a flag requires a rebuild/redeploy, which is a real limitation for teams that need instant kill-switches without a deploy.
- Early-stage; breaking changes between minor versions are plausible at this maturity.

## Recommendation
PROTOTYPE — low-risk to try on one real Next.js/React side project given the near-zero integration cost, but not yet something to bet a team workflow on given the thin community signal.

## Change History
### 2026-11-24
First seen and reviewed as today's Small but High-Leverage Utility. Discovered via GitHub search for feature-flag topic tag. Verified license and the optional-vs-required nature of the Cloud component by reading the README directly.
