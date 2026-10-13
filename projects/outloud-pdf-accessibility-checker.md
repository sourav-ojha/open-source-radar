# outloud

## Summary
PDF accessibility checker that runs locally in the terminal, CI, or a local browser app: PDF/UA-1 (ISO 14289-1) conformance plus 35 semantic rules that catch what key-existence validators like veraPDF and PAC miss (alt text that is a filename, table header cells that are empty, prose marked as decorative so screen readers skip it). Pure Python (pikepdf-based), no JVM, no upload — nothing leaves the machine. Outputs exit codes, JSON, SARIF 2.1.0 (for GitHub code scanning), and single-file HTML reports; also usable as a Python library.

## Why I Should Care
Any product that generates or accepts PDFs (invoices, reports, compliance documents) and sells into enterprise or government customers eventually hits a WCAG/PDF-UA accessibility requirement. This is a zero-dependency, install-and-run way to check that before it becomes a procurement blocker.

## Problems It Can Remove
Correctly walking a PDF's content stream, merging glyph-level text ops into words/lines, and mapping findings to both ISO 14289-1/Matterhorn checkpoints and WCAG 2.2 criteria is a multi-week correctness project; this one ships with 95 tests, a fixture builder, and a published benchmark comparing itself against the two reference tools.

## Practical Uses
- CI gate that fails a build when generated/uploaded PDFs (invoices, reports, compliance docs) are not screen-reader accessible
- One-off terminal check before publishing or handing off a client deliverable PDF
- SARIF output wired into GitHub code scanning for a visible accessibility findings list on PRs
- Comparing a remediated PDF against its original to confirm structure/text/appearance was not lost in remediation

## Product Opportunities
- Accessibility compliance (WCAG 2.2/PDF-UA) is a real procurement requirement for enterprise and government contracts; a self-hosted checker is a differentiator for any document-generation feature in a SaaS product
- Could be wrapped as an MCP tool or CLI step in an automated document-QA pipeline

## Agent / Automation Opportunities
- Usable directly as a library (`from outloud import check`) inside an automated document-generation/QA agent pipeline
- SARIF output is a natural fit for a coding-agent CI loop that needs machine-readable findings with a one-line fix per issue

## Integration
Deployment: `pipx install outloud` or `uv tool install outloud`; also a local browser app served from the Python standard library (binds localhost only).
Interfaces: CLI, Python library, local web app.
Integration effort: **Low** — single pip install, no runtime dependencies beyond Python 3.10+, drops straight into a CI step.

## Architecture Notes
`model.py` reads the file once with pikepdf and resolves role-mapped types, inherited page references, and marked content inside form XObjects. `content.py` walks each page's content stream recording every painted text run with position and tag, merging glyph-level operators into words/lines (handling kerning gaps and ligatures) so rules can actually find text in files written by Chrome, Word, or TeX. Rules are declarative, data-driven entries in a YAML catalogue, each paired with a function and a fixture built from a clean baseline with exactly one thing bent.

## Maturity
Emerging — first PyPI release (0.2.1) on 2026-10-06, repo created 2026-09-17. GitHub API verified: Apache-2.0, 13 stars, pushed_at 2026-10-06. Despite the youth, the README publishes a dated (17 September 2026), reproducible benchmark against veraPDF 1.26.5 and pdfa11y across five corpora, and explicitly documents what the tool is *not* (not a certification, not legal advice, no colour-contrast checks yet).

## License
Apache-2.0, confirmed via GitHub API license field (PyPI metadata omits a license classifier; GitHub is authoritative). No restrictions on commercial use, SaaS deployment, or embedding.

## Alternatives
- PAC (proprietary, free, Windows-only) — the closest feature match for per-criterion WCAG/PDF-UA views, but no CLI or CI integration
- veraPDF (GPL/MPL, JVM) — the reference PDF/UA-1 conformance validator; outloud is explicitly calibrated to agree with it (160/160 on its own fixture+real-file test set) but 2-10x faster with no JVM startup cost
- pdfa11y (MIT) — faster still, but the author's own comparison doc states it misses most of the semantic-layer findings outloud is built around

## Risks / Limitations
- Very young (first PyPI release 2026-10-06, 13 GitHub stars) — low external validation beyond the maintainer's own published benchmark
- Single-maintainer project (Visionably); semantic rules are explicitly described in the README as heuristics, not certification-grade
- PDF/UA-2 and colour-contrast (WCAG 1.4.3/1.4.11) are not implemented yet (roadmap items)

## Recommendation
**USE NOW** (score 8.1/10) — negligible adoption cost (one pip install), narrow and well-tested scope.

## Change History
### 2026-10-13
First discovered and reviewed via Show HN. Verified via GitHub API (Apache-2.0, 13 stars, created 2026-09-17) and PyPI (version 0.2.1, uploaded 2026-10-06). Selected as today's Small but High-Leverage Utility for its unusually rigorous, dated benchmark methodology and honest limitations section for a two-week-old project.
