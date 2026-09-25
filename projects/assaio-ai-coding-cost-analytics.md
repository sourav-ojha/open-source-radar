# assaio

## Summary
Offline-first Go CLI (`assaio-agent`) with embedded SQLite that reads local logs from
Claude Code, Codex CLI, Gemini CLI, GitHub Copilot CLI, Cline, and Antigravity CLI to
report token usage, API-equivalent cost, and accepted-edit/code-producing activity per
project and vendor — without extracting or storing prompt text, response text, or
repository file contents.

## Why I Should Care
This radar's own README flags that a daily agent run "draws from the same rolling
usage window as every other Claude Code session" on the account — assaio is a
concrete, local, privacy-preserving way to actually see that spend and correlate it
with what got shipped, instead of guessing from the account usage page alone.

## Problems It Can Remove
Removes the need to check five separate vendor dashboards to understand AI-coding
spend and activity, and removes the guesswork in "is this actually producing shipped
code" by correlating local sessions with local commits (labeled matched/ambiguous/
unmatched, never asserted as causal).

## Practical Uses
- `assaio-agent dashboard --since 30d --output assay.html` for a self-contained
  offline cost/activity report across every installed coding assistant.
- `assaio-agent evidence --repo . --since 30d` to see which sessions plausibly
  produced which commits in a specific repo.
- `assaio-agent doctor --strict` to catch vendor log-format drift before trusting a
  report.
- Run it against this very repo's history to see what the daily radar run itself
  costs in Claude usage over time.

## Product Opportunities
The self-hosted "team server" mode (`serve`/`sync`) — explicitly an early MVP today —
is worth revisiting later as a lightweight, privacy-first alternative to enterprise
AI-coding-analytics SaaS, if it matures.

## Agent / Automation Opportunities
Indirect: it's a tool *about* agent usage rather than an agent tool itself. Its
plugin/extension protocol (any executable, any language, adding a parser/metric/check
via a versioned JSON handshake) is a clean integration point if a custom local coding
tool's logs ever need to be added.

## Integration
Single Go binary via Homebrew or `go install`. `assaio-agent demo` previews it without
touching real logs; `assaio-agent init` does a guided import and writes the first
report, sending nothing over the network on the default path. Low integration effort.

## Architecture Notes
Go binary with embedded SQLite, no external database. Per-vendor parsers read existing
local log formats (not proprietary telemetry). Costs are computed from a vendored
LiteLLM pricing snapshot (MIT-licensed, separately attributed) rather than live vendor
pricing APIs; a `reconcile` command compares a downloaded export against the local
estimate. Every reported metric carries explicit source coverage, sample size, and
parser-version metadata rather than silently treating missing data as zero.

## Maturity
Experimental/pre-1.0, and the project says so directly: "suitable for evaluation and
design-partner pilots," not yet validated across multiple external teams or release
cycles. v0.28.0, 86% statement-coverage snapshot at last audit, CI with race tests and
native parser fuzzers, a published corrections log for figures later found wrong.
Single primary maintainer (131 of ~136 commits). Last push 2026-09-25 (day of review).

## License
Apache-2.0. No restrictions on commercial or embedded use. The embedded LiteLLM
pricing snapshot is separately MIT-licensed (attributed in NOTICE).

## Alternatives
Individual vendor usage dashboards (Anthropic Console, OpenAI usage page, GitHub
Copilot metrics) give accurate quota data but are siloed per vendor with no local
commit correlation. No self-hosted, cross-vendor, privacy-first equivalent surfaced in
this run's search.

## Risks / Limitations
- Pre-1.0, single maintainer — treat conclusions as directional, not authoritative,
  until it matures.
- Cost figures are estimates from a maintained pricing snapshot, not vendor invoices;
  use `reconcile` before trusting an exact dollar figure.
- Team server mode explicitly lacks RBAC, token rotation, and a tested backup/restore
  path today — local single-user mode is the production-ready part.
- Vendor log formats can and do change; `doctor` surfaces drift but can't prevent it.

## Recommendation
PROTOTYPE — low-risk to try immediately given the read-only, offline-by-default
design; genuinely useful right now for understanding this radar's own Claude usage
footprint and any other coding-agent spend, with the caveat that it's evaluation-grade
software, not yet a hardened team tool.

## Change History
### 2026-09-25
Discovered via GitHub topic search (`topic:developer-tools`) during slot 3 discovery.
Catalogued as PROTOTYPE, 7.4/10.
