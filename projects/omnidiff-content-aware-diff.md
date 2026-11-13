# OmniDiff

## Summary
A diffing engine that selects a format-specific algorithm per file type —
code, images, cursors, fonts, archives, message catalogs, PDFs — instead of
running textual diff on everything and showing "Binary files differ" for
anything that isn't text. Ships as a Rust crate/CLI with published
per-content-type speed and accuracy figures and an in-browser showcase
comparing it side-by-side against GNU diff on real code changes.

## Why I Should Care
Most diff tooling is either purely textual (git diff) or syntax-aware for
code only (difftastic, delta). OmniDiff's actual claim — backed by a
published accuracy table — is choosing the right diff strategy per file
type, covering assets a product repo accumulates and git normally treats
as opaque (images, fonts, PDFs, archives). Worth studying as a reference
design even before deciding whether to adopt it directly.

## Problems It Can Remove
- "Binary files differ" on image/font/PDF changes in a PR, with no visual
  or structural diff at all.
- Needing a separate specialized tool per asset type (image diff, PDF
  diff) instead of one tool that dispatches by content.

## Practical Uses
- Wiring it in as a custom `git diff`/`difftool` driver to get a real diff
  on non-code assets in a PR review.
- Studying its content-type detection and per-format algorithm selection as
  a reference for building format-aware diffing into an internal
  code-review or design-review tool.

## Product Opportunities
None directly, though the dual-license (AGPL core + commercial license)
business model is worth noting as a studyable pattern independent of the
tool itself.

## Agent / Automation Opportunities
A format-aware diff is a materially better input than raw textual diff for
a coding agent reviewing a PR that touches non-code assets — it could tell
an agent what actually changed in an image or font rather than just that
the bytes differ.

## Integration
Rust crate (`cargo install`), CLI binary, docs.rs-published library API.
No JS/Node bindings found at time of review. **Integration effort:
Medium** — usable as a standalone CLI or git diff driver without code
changes, but not directly embeddable into a Node/React codebase as a
library.

## Architecture Notes
Per-content-type dispatch (98.8% of changes diffed with a content-specific
algorithm per the README) rather than one universal diff algorithm. The
published accuracy table breaks results down by content type (code,
pictures, cursors, fonts, archives, message catalogs, PDFs) with explicit
caveats on sample size for thinner categories (PDFs: fewer than ten judged
test cases).

## Maturity
Created 2025-10-17 (~1 year old), latest release v0.4.0 (2026-11-08).
1,355 stars, 36 forks, 3 subscribers, 12 open issues — CI, a coverage
badge, and docs.rs integration suggest active engineering. The
star-to-subscriber ratio (~452:1) is elevated, but the combination of
forks, open issues, CI, and a public showcase site looks like organic use
rather than fabricated engagement.

## License
**AGPL-3.0-only**, with a separate commercial license available
(`LICENSE-COMMERCIAL`) for those who don't want AGPL's terms — flagged
loudly per AGENT.md §14: embedding OmniDiff inside a hosted/commercial
product under the AGPL terms would trigger its network-use source-
disclosure requirement unless the commercial license is purchased. Running
it locally as a CLI tool or git diff driver for personal/internal work is
unaffected by that clause.

## Alternatives
- GNU diff / `git diff` (textual only, no content awareness).
- difftastic / delta (syntax-aware, code-only, no image/font/PDF support).
- Specialized single-purpose tools (perceptual image diff, PDF diff
  utilities) used piecemeal, one per format.

## Risks / Limitations
- AGPL licensing, as above — a real consideration for any commercial-
  embedding use case, not just a box to check.
- Self-reported accuracy benchmarks measured against the maintainer's own
  test corpus, not independently re-verified by this review.
- No Node/JS bindings — pure Rust/CLI integration only.

## Recommendation
**STUDY** — the per-content-type dispatch architecture is worth
understanding even without adopting the tool; try it as a git diff driver
on a repo with real image/font assets before deciding whether it earns a
PROTOTYPE upgrade.

## Change History
### 2026-11-13
Initial discovery and review. Rotation slot 3 (developer utilities,
debugging, testing).
