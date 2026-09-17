# SnapOtter

## Summary
Self-hosted file-processing suite: 200+ tools across image, video, audio, PDF, and
general files (convert, compress, OCR, transcribe, redact, watermark), plus local AI
(background removal, upscaling, face blur/enhance, OCR, transcription) — all on hardware
you own, via a web UI, REST API, and chainable pipelines. Explicitly positioned to
replace CloudConvert, Smallpdf, TinyPNG, TinyWow, and Otter.ai.

## Why I Should Care
Consolidates five separate paid-SaaS categories into one self-hosted Docker stack, and
"your files never leave your network" is a genuine selling point for MSSP client work
where sending client documents through a third-party SaaS is a real concern, not just a
nice-to-have.

## Problems It Can Remove
Recurring subscriptions to CloudConvert/Smallpdf/TinyPNG/TinyWow/Otter.ai-class tools;
ad-hoc scripts wrapping ffmpeg/tesseract/image libraries for one-off conversion needs;
the "which SaaS do I trust with this client file" question entirely.

## Practical Uses
- Redact, watermark, or OCR client documents for MSSP consulting work without a
  third-party upload
- Batch-convert/compress images, video, or PDFs via the REST API from another script or
  service
- Chain a multi-step pipeline (OCR -> redact -> watermark -> export) as a reusable
  workflow
- Transcribe audio/video locally instead of paying for Otter.ai
- Self-host as an internal "utility API" other internal tools call instead of each
  hand-rolling conversion logic

## Product Opportunities
Could sit behind a client-facing feature via its REST API (used unmodified, not
redistributed) without taking on AGPL's network-copyleft obligations.

## Agent / Automation Opportunities
No dedicated MCP server ships today, but the REST API has interactive docs
(`/api/docs`) and API-key auth, making it straightforward to wrap as an agent tool for
file conversion/OCR/transcoding steps in a larger workflow.

## Integration
One-line `docker run` for a quick start (embedded Postgres 17 + Redis 8), or a 3-container
Compose stack for production. Multi-arch (AMD64/ARM64, including Raspberry Pi). Optional
NVIDIA GPU acceleration for AI features. OIDC/SSO login. Low effort overall — this is
close to a drop-in appliance, not a framework to integrate against.

## Architecture Notes
Single application image bundling a layer-based image editor, a local-AI layer (built-in
Fast OCR adds ~25 MiB; an optional accuracy pack installs on demand), and a pipeline
engine that chains tools into reusable, JSON-importable workflows (20 steps by default,
configurable via `MAX_PIPELINE_STEPS`).

## Maturity
Emerging. Created 2026-03-29 (~5.5 months old at review), 2,681 stars grown organically
(not a hype-wave repo), 221 open issues, 136 forks, green CI, OpenSSF Best Practices
badge, active Discord and GitHub Sponsors. Latest tagged GitHub Release (v2.2.0) is from
2026-07-29 despite commits continuing daily — Docker Hub tags likely track newer builds
than the GitHub Release; check tag freshness before pinning a version.

## License
AGPL-3.0. Fine for unmodified internal/self-hosted use. If modified and exposed as a
network service to third parties (e.g. embedded into a client-facing MSSP portal
feature), AGPL's network-copyleft clause requires releasing the modified source. Using it
unmodified behind your own API, or purely internally, avoids that obligation — read the
license carefully before any commercial embedding.

## Alternatives
Stirling-PDF (PDF only), ConvertX (conversions only), CloudConvert / Smallpdf / TinyPNG /
TinyWow / Otter.ai (the paid SaaS incumbents it explicitly targets).

## Risks / Limitations
- AGPL-3.0 licensing implications for commercial/hosted embedding
- Ships with default `admin`/`admin` credentials — must be changed on first login; do not
  expose a fresh instance publicly before changing it
- GitHub Release tag lags actual development; verify the Docker image tag you're pulling
  rather than trusting the GitHub Releases page for freshness

## Recommendation
USE NOW — low integration effort, directly replaces multiple recurring SaaS subscriptions,
and the "files never leave your network" property is directly relevant to client work.

## Change History
### 2026-09-17
Initial discovery and cataloguing.
