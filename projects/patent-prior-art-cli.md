# patent

## Summary
A Rust CLI/TUI that takes a plain-English dev-tool idea and fans it out across 19 sources
(npm, crates.io, PyPI, Homebrew, Packagist, Hex, GitHub, Hacker News, and more), ranks the
results by local semantic similarity (via `fastembed`, no API key required), and summarizes
the landscape as **Open**, **Crowded**, or **Saturated**. An optional local Ollama model (or
any OpenAI-compatible endpoint) turns the similarity data into a short verdict; `--fast`
skips the LLM entirely and still gives a usable similarity-ranked list.

## Why I Should Care
This radar's entire mission is deciding "has this already been built, and is it worth
building anyway" — `patent` automates the first half of that question for Sourav's own
side-project and micro-SaaS ideas, before any code gets written. It is the kind of narrow,
well-executed utility the operating spec explicitly asks to hunt for.

## Problems It Can Remove
The manual ritual of checking npm, PyPI, crates.io, GitHub, and HN separately (and usually
only by keyword, missing differently-worded prior art) before starting a new tool or
product idea.

## Practical Uses
- Before starting a new micro-SaaS or internal tool, run `patent "idea in plain English"`
  and get an honest "Crowded"/"Saturated" verdict with the closest existing projects and
  how stale they are.
- Pipe `--json` output into a script to gate a project-idea backlog or CI check.
- Use as a library (`sources::search_all`, `rank::rank_async`, `verdict::assess`) inside a
  custom idea-triage tool rather than shelling out to the CLI.
- Run with `--api-base` pointed at an existing LLM gateway instead of installing Ollama
  locally.

## Product Opportunities
Not a product itself, but a genuinely useful pre-build gate for anyone doing serial
micro-SaaS/tool experiments — the kind of due-diligence step that's easy to skip under time
pressure and expensive to skip in practice.

## Agent / Automation Opportunities
The published library crate (`sources::search_all`, `rank::rank_async`, `verdict::assess`)
is directly embeddable as a coding-agent tool or a pre-flight check in an agent workflow
that proposes new tools/features — "before you scaffold this, check it's not already
built."

## Integration
Single static binary via a curl installer, Homebrew, or `cargo install`; Windows zip also
published. No server, no account, fully offline in `--fast` mode. Effort: **Low**.

## Architecture Notes
Explicitly honest about its own limits: a clean "Open" result is scoped to "nothing close
found in the sources checked," never claimed as proof nothing exists; stale matches (2+
years untouched) are flagged, not demoted, since an abandoned tool is still proof the idea
was built before. Any source that fails to respond is surfaced as "not reached" rather than
silently dropped. This is the right default behavior for a tool whose entire value is "tell
me the truth, including what you don't know."

## Maturity
**Emerging.** Six months old (created 2026-05-30), actively developed with CI, clippy/fmt
gates, a published crate on crates.io and docs on docs.rs, and multiple real contributors
beyond the primary author (dependabot plus at least five human contributors with meaningful
commit counts). 570 stars but 0 GitHub "watchers" — an odd ratio on its own, but the
multi-contributor activity and clean single-issue backlog argue against the star-inflation
pattern seen elsewhere; most people who star a CLI tool never click "Watch."

## License
Dual MIT OR Apache-2.0 — fully permissive, no commercial restrictions.

## Alternatives
Nothing directly equivalent found; closest comparisons are manually checking package
registries one at a time, or using a general web-search LLM prompt (which does not rank by
true semantic similarity across registries or flag staleness consistently).

## Risks / Limitations
Coverage is inherently incomplete — 19 sources is a lot but not exhaustive, and the tool
says so. Local semantic search quality depends on the bundled embedding model; the LLM
verdict step is optional and off by default unless Ollama (or another endpoint) is
configured.

## Recommendation
**PROTOTYPE** — low-effort, high-leverage pre-flight check to adopt immediately before the
next side-project or micro-SaaS idea gets any code.

## Change History
### 2026-11-28
Initial discovery and review.
