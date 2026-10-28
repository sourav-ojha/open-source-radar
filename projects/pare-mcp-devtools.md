# Pare

## Summary
Pare is a suite of 28 MCP server packages (240 tools total) wrapping common dev CLIs — git, GitHub, npm/cargo/go/bun/deno/dotnet/ruby/jvm, build/lint/test runners (tsc, eslint, vitest, pytest, jest), docker/k8s/terraform/trivy/ssh, and utilities like ripgrep/curl/psql/redis. Each tool returns schema-validated structured JSON (via MCP's `outputSchema` feature) instead of raw terminal text, aimed at cutting both the token cost and the parsing fragility of an agent shelling out to these tools directly.

## Why I Should Care
A meaningful share of a coding-agent session's context budget is spent on verbose CLI output — test runs, docker/npm installs, build logs. Pare attacks this at the source (structured tool output) rather than via post-hoc compression, for exactly the tool surface (git, npm, docker, test runners, Postgres/Redis via `db`) this profile uses daily.

## Problems It Can Remove
Writing and maintaining one-off MCP wrappers per CLI tool to get structured, agent-parseable output instead of scraped terminal text — and the recurring agent tool-use errors that come from parsing raw CLI text.

## Practical Uses
- Replace raw `git status` / `npm test` / `docker ps` shell calls in an agent's tool use with structured JSON, cutting reported token cost 36-95% on verbose commands per the README's own benchmark
- Standardize how a coding agent interacts with build/lint/test tooling across a polyglot repo via the `full` preset
- Scope which tool domains an agent session can reach (e.g. only `git` + `test`, not `docker`/`k8s`) via `PARE_GIT_TOOLS` / `PARE_TOOLS` env-var filtering
- Use the terraform/trivy/ssh/k8s servers to give an agent structured infra-inspection tools instead of raw CLI text

## Product Opportunities
None directly — this is developer tooling, not a product-building block.

## Agent / Automation Opportunities
Directly reduces context-window pressure from verbose CLI output across long agent sessions — a recurring pain point for multi-hour coding-agent runs. `outputSchema`-based structured responses are more reliably parseable by an agent than scraping CLI text, removing a class of agent tool-use errors.

## Integration
`npx @paretools/init --client <client> --preset <preset>` (presets: `web`, `python`, `rust`, `go`, `full`), append the generated agent-rules file to the project, restart the client session, validate with `npx @paretools/init doctor`. Setup guides cover roughly ten clients including Claude Code, Cursor, VS Code, and Gemini CLI. Node.js 20+ required. Integration effort: **Low** — one installer command, though the README itself recommends installing only 2-4 of the 28 servers rather than all of them, to avoid context bloat from over-registering tools.

## Architecture Notes
Each server returns both a `content` field (for display) and a `structuredContent` field (for the agent), leaning on MCP's `outputSchema` feature rather than a bespoke protocol. This is a different approach from generic output-compression tools (Headroom, Paritok, squeeze — already in the catalog) that compress whatever text a tool happened to return after the fact; Pare instead replaces the tool call itself so structured output is native, for the specific CLI surface it covers.

## Maturity
Emerging. GitHub API confirmed: MIT, 139 stars, 16 forks, 0 subscribers, 19 open issues, created 2026-02-10, pushed 2026-10-28. README shows CI, code coverage, OpenSSF Scorecard and Best Practices badges, TypeScript strict mode, and a published npm package with download badge. Token-savings benchmark covers only 8 commands and is self-reported; broader claims ("eliminates this entire class of errors," "zero information lost") are unsubstantiated in the README. No subscribers despite the badges is worth rechecking.

## License
MIT, confirmed via GitHub API. No restrictions.

## Alternatives
Headroom, Paritok, and squeeze (already catalogued) solve a related but different problem — generic post-hoc compression of whatever output an agent already received, rather than making the underlying tool call return structured output in the first place.

## Risks / Limitations
- Self-reported benchmark covers only 8 of 240 tools
- 0 subscribers on GitHub despite 139 stars and OpenSSF badges
- Breadth (28 packages) vs. depth: no visible release history or adoption signal per-wrapper, hard to judge how battle-tested each one is
- Operational friction noted in the README itself: Windows users may need a `cmd /c npx` wrapper, slow first starts / connection timeouts common enough to have dedicated troubleshooting docs

## Recommendation
PROTOTYPE — install the 2-4 servers most relevant to this profile's stack (git, npm, test, docker) per the README's own guidance, rather than the full preset, and measure whether the structured-output approach actually reduces agent tool-use errors in practice.

## Change History
### 2026-10-28
Initial discovery and review. Rotation slot 1 (AI agents, MCP, coding productivity).
