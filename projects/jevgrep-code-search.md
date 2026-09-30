# jevgrep

## Summary
`jg` is a CLI (plus a bundled agent skill) that answers "where does X happen in this
repo?" in one shot: it uses a cheap judge model ("Jev") to rank relevant folders, files,
and declarations, then returns verbatim source excerpts and reading leads to stdout —
giving a coding agent a starting point instead of letting it burn tool calls doing its
own exploratory grep/read cycle.

## Why I Should Care
Every coding-agent session on an unfamiliar or large codebase spends real tokens just
locating the relevant code before it can start editing. jevgrep's own disclosed
benchmark (a 10-task SWE-bench comparison) reports completing the same 8 of 10 tasks as
an unaided baseline at roughly 30% lower cost — a direct, measurable reduction in the
token bill for agent-driven work on MERN/Node/React repos.

## Problems It Can Remove
The "agent reads six files to find the one that matters" tax on every non-trivial coding
task, and the manual pointing-the-agent-at-the-right-file step a developer does out of
habit today.

## Practical Uses
- `jg "How are telemetry events recorded and sent?"` before letting an agent implement a
  cross-cutting change in an unfamiliar part of a codebase.
- Bundled skill (`jg skill`) teaches Claude Code / Codex / OpenCode to call it
  automatically as a first step, rather than needing to be told each time.
- Useful on any repo, not just the author's own — a general productivity multiplier for
  agent-assisted development on his MERN stack and side projects.

## Product Opportunities
None directly — this is a developer-tooling utility, not a product-building block. Its
value is entirely in reducing agent cost/latency during development.

## Agent / Automation Opportunities
Ships as an installable Claude Code / Codex / OpenCode skill via
`npx skills add dzhng/jevgrep --skill jevgrep` (using Vercel's `skills` CLI, already
catalogued as [[vercel-skills-cli]]), so it plugs directly into an existing agent-skill
workflow rather than requiring bespoke integration.

## Integration
`npm install -g @dzhng/jevgrep`, then `jg skill` to teach the agent to use it. Requires a
key for one of: Vercel AI Gateway, TypeSafe, OpenRouter, OpenCode Zen, or a custom
TypeSafe-compatible endpoint — provider choice is flexible, but the tool does not work
without a network-accessible judge-model endpoint. Node.js 22+, macOS/Linux. Low
integration effort given the existing skills-CLI plumbing.

## Architecture Notes
Same "cheap typed judge model instead of a full LLM call" pattern seen in
[[abide-rule-enforcement]] — here applied to file/relevance ranking rather than rule
compliance. Both use the same "Jev" model family from TypeSafe, and both are built by
different teams within days of each other, suggesting a real emerging primitive rather
than a single vendor's marketing angle. Built on top of `pi` (earendil-works/pi), a
110k-star agent-toolkit base that several of today's other finds also build on — pi
itself is mainstream enough now not to warrant a separate discovery entry, but worth
knowing it's the common substrate under this generation of small coding-agent tools.

## Maturity
Very early — repository created 2026-09-26 (4 days before this review), already at
npm 0.7.0 / GitHub release v0.6.0 with 25 open issues and 5 active contributors including
the author (dzhng, known for the widely-used open-source "deep research" project). Real
multi-contributor activity and a disclosed benchmark methodology, but effectively no track
record yet — treat star/fork counts (1,815 stars / 118 forks in 4 days) as reflecting the
author's existing following, not independent validation.

## License
MIT. Depends on an external judge-model provider for its core function; several provider
options exist so it isn't locked to one vendor, but it is not usable fully offline/self-
hosted without standing up a TypeSafe-compatible endpoint yourself.

## Alternatives
Plain grep/ripgrep plus agent judgment (free, but exactly the cost this tool claims to
cut); full-LLM-call-based semantic code search tools already in the catalog (e.g. code
search/RAG-for-code entries) — jevgrep's distinction is using a cheap decision model
specifically to keep the search step itself inexpensive rather than adding a heavier
embedding/RAG layer.

## Risks / Limitations
4 days old at review time — no meaningful signal yet on long-term reliability or whether
the benchmark claim (30% cost reduction, 8/10 task parity) holds up outside the author's
own 10-task sample. Hard dependency on a paid or gateway-routed judge-model endpoint.

## Recommendation
PROTOTYPE — low-risk to try given it's an additive CLI/skill rather than a system
dependency; worth a trial run on a real task to see if the reported cost reduction holds,
but treat it as unproven until it has more runway.

## Change History
### 2026-09-30
Initial discovery and review.
