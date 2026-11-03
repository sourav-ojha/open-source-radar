# pngdeer

## Summary
A lossy PNG/JPEG/AVIF/animated-PNG compressor (Rust) with no quality slider: for each image it searches palette sizes 2-256, with and without dithering, scores results with the SSIMULACRA2 perceptual metric, and keeps the smallest file that still looks right. The same Rust code runs identically as a CLI and as a browser WASM app (nothing uploaded).

## Why I Should Care
Directly replaces a recurring paid SaaS dependency (TinyPNG) with an open-source tool the README claims beats it by 8-12% at equal perceptual quality on the median image. Because the same code runs in both the CLI and the browser WASM build, the claim is at least structurally checkable rather than purely vendor-reported.

## Problems It Can Remove
Removes a TinyPNG subscription or manual per-image quality-slider guessing from an asset pipeline.

## Practical Uses
- A pre-commit or CI image-optimization step for a web app's asset pipeline.
- One-off compression of screenshots or marketing assets before upload.
- Batch AVIF/WebP conversion for a product site, same tool for PNG and JPEG photo sources.

## Product Opportunities
The palette-search-plus-perceptual-scoring engine could be wrapped as a small internal API or Lambda for an asset pipeline instead of paying for TinyPNG.

## Agent / Automation Opportunities
None beyond being shell-scriptable from a CI pipeline or an agent's tool-call surface — no MCP server or library package currently.

## Integration
Homebrew tap or `cargo install`; CLI binary and a browser WASM app. Low integration effort for ad hoc use; embedding in a Node/Next.js build pipeline currently means shelling out to the binary rather than importing a library, since no npm/library package was found.

## Architecture Notes
Automatic palette-size search with perceptual (SSIMULACRA2) scoring, applied consistently across PNG, JPEG (mozjpeg) and AVIF encoders, with an identical WASM port of the same code path used in the browser — worth studying as a reference for "measure, don't guess" compression tooling.

## Maturity
Experimental. Created 2026-10-25 (9 days old at review), latest tag v0.2.1 (no GitHub Release notes), 427 stars / 39 forks / 2 subscribers / 1 open issue. Only 2 contributors (company account heydeer plus an AI coding-assistant commit co-author).

## License
MIT, no restrictions.

## Alternatives
TinyPNG (proprietary SaaS, the project's explicit benchmark target); pngquant/oxipng/ImageOptim (open-source, but the README's leaderboard claims to beat them); Squoosh (Google's browser tool, manual quality slider instead of automatic selection).

## Risks / Limitations
- Benchmark claims against TinyPNG and 8 open-source quantizers are self-reported and not independently reproduced here.
- No published npm/library package — CLI/browser only today.
- Very new (9 days), thin contributor base.

## Recommendation
PROTOTYPE — low-risk to trial directly against a real asset set and compare file sizes/quality before swapping out an existing pipeline step.

## Change History
### 2026-11-03
First seen and reviewed. Discovered via GitHub search for recently created small CLI utilities; verified license and engagement metrics via GitHub API directly.
