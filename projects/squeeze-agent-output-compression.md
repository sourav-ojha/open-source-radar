# squeeze

## Summary
A CLI that sits in a Unix pipe between a tool and a coding agent, parsing the tool's actual output format — `terraform show -json`, `terraform validate -json`, tflint, checkov, trivy, pytest, `go test -json` — and printing the smallest text that still carries every change, failure, or decision in it. Not a truncation: it understands each format's own semantics (Terraform's counts and symbols, pytest's rerunnable node IDs) and announces what it cut and which flag restores it. Guarantees that any value a tool marks as sensitive never reaches stdout, at any nesting depth.

## Why I Should Care
Directly answers two concrete problems cited (with linked GitHub issues) in its own README: one Claude Code diagnostics call that returned roughly 580,700 tokens and filled an agent's context window, and a `pytest -v` run that printed around 150 lines when 8 carried the actual decision. For anyone running Terraform, pytest, or Go tests through a coding agent regularly, this is a direct token-cost and context-budget fix with no agent-side integration beyond a pipe.

## Problems It Can Remove
Hand-rolled `| tail -n 100` or byte-cap hacks in an agent's instructions that can — and regularly do — cut past the one line that actually mattered (the failing test, the destructive resource replacement).

## Practical Uses
- `terraform plan -json | squeeze` before letting an agent read a plan, to see drift without re-reading full unchanged state
- `pytest 2>&1 | squeeze` so a failing agent session reruns exactly the failing node ID squeeze names, not a scrollback guess
- `checkov -d . -o json | squeeze` / `trivy fs --format json . | squeeze` to compress security-scan output the same way
- `--fail-on error,fail` in an agent's AGENTS.md/CLAUDE.md so a stop condition is a clean exit code, not prose-parsing
- `--raw-dir .squeeze` to keep the untouched original on disk for the rare case the squeezed output isn't enough

## Product Opportunities
None directly — a developer-facing CLI utility, not a product component.

## Agent / Automation Opportunities
Designed explicitly to be dropped into an agent's own project instructions (AGENTS.md/CLAUDE.md) as a one-line replacement for running these tools directly — no MCP server, no hook configuration, no editor plugin required.

## Integration
Deployment: `go install github.com/cauebraz/squeeze/cmd/squeeze@latest` (or `@<commit>`, recommended while there is no tagged release).
Interfaces: CLI only (`--output json` available for scripts).
Integration effort: **Low** — single binary, works with any agent or script that can run a pipe.

## Architecture Notes
Each format is a separate package behind a shared interface with its own contract tests; Terraform-format conformance is checked by running squeeze against Terraform's own command test corpus at a pinned commit (`just conformance`). The project enforces a 95% coverage gate and per-format fuzzing (`just fuzz <format> <seconds>`) as part of its own CI.

## Maturity
Experimental. Repo created 2026-10-13 (one day old at review time), 4 stars, no tagged GitHub release yet — the README itself says to pin a commit since output shapes, flags, and JSON keys may still change. Despite the youth, the engineering rigor (conformance tests, fuzzing, coverage gate, concrete linked-issue motivation) is unusually high for a project this new.

## License
MIT, confirmed via GitHub API license field. No restrictions.

## Alternatives
- Headroom (already catalogued) — general-purpose context/log/RAG-chunk compression as a proxy or library; broader scope, less format-specific guarantees
- Paritok (`Paritok-official/paritok-4b-v1`) — a model-based compression gateway (fine-tuned 4B model as a drop-in LLM proxy); claims larger compression ratios on long sessions but requires hosting or calling a model, a heavier integration than a pipe
- `tf-summarize`, `tfplan2md` — human-oriented Terraform plan summarizers (color, Markdown tables) that still cost an agent the same tokens for the layout

## Risks / Limitations
- Explicitly pre-1.0 with no release yet; do not take a floating dependency on it
- Essentially unvalidated outside the maintainer's own tests (4 stars, 1 day old)
- Covers only 9 formats; anything else falls through to a generic text-compaction mode with weaker guarantees

## Recommendation
**PROTOTYPE** (score 7.5/10) — hidden-gem candidate given the engineering quality, but genuinely too new to run unattended; try it by hand on one real Terraform or pytest run first.

## Change History
### 2026-10-14
First discovered via HN Algolia `search_by_date` (Show HN, posted 2026-10-13) during the slot 1 run.
