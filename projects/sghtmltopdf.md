# sghtmltopdf

## Summary
An HTML-to-PDF renderer written in Rust, aimed squarely at wkhtmltopdf's and wicked_pdf's old job
(invoices, receipts, reports — documents that flow top to bottom with explicit page breaks) but
with no Chromium, WebKit, or Gecko dependency. One binary, a Ruby gem/Rails integration, an HTTP
server mode, and a Rust crate, all sharing the same engine and CLI-flag-compatible option set.

## Why I Should Care
wkhtmltopdf is effectively abandoned and wicked_pdf inherits its problems; the common modern
replacement is driving headless Chrome via Puppeteer/Playwright, which means shipping a browser
process for every PDF-generating request. Invoice, receipt, and report generation is a routine
need in any billing-adjacent SaaS product, and this removes the headless-browser weight entirely
while staying close enough to wkhtmltopdf's CLI flags that migration is close to drop-in for that
specific use case.

## Problems It Can Remove
Removes the operational cost of running headless Chrome just to turn HTML into a PDF: memory
overhead, `document.fonts.ready` race conditions, and the attack surface of a full browser engine
for a task that's really "lay out a document and write PDF bytes." Streaming output also avoids
buffering an entire multi-page document in memory before the first page is written.

## Practical Uses
- Invoice/receipt generation in a billing flow — the README's own worked example.
- PDF report generation from a server-rendered HTML template in an admin panel or internal tool.
- Replacing an existing wkhtmltopdf or wicked_pdf integration that's stuck on an abandoned engine.
- Running as a separate HTTP server process (`sghtmltopdf server`) when you'd rather not spend the
  main app's CPU on rendering.

## Product Opportunities
A lighter-weight PDF-generation sidecar for any product that currently pays the Puppeteer/
headless-Chrome tax purely for invoice/report rendering — meaningful infra cost reduction at
scale (no browser process per render).

## Agent / Automation Opportunities
CLI and HTTP server modes make this trivial to wrap as an agent tool or CI step (e.g., rendering
a generated report to PDF as a pipeline artifact) without any browser-automation dependency.

## Integration
CLI binary, Rust crate (crates.io), Ruby gem (RubyGems, with a Rails integration), or HTTP server
mode. **No native Node.js/npm binding yet** — a Node/React stack integrates via the CLI binary as
a subprocess or by calling the HTTP server mode over REST, both straightforward. **Integration
effort: Low** for the CLI/HTTP path; a native JS binding would need to be contributed or wrapped.

## Architecture Notes
Explicitly does not try to be a full browser: HTML parsing (html5ever), CSS parsing (cssparser),
and selector matching (selectors) are the same Servo-derived, Firefox-shipped crates used by
Stylo, but everything after that — box generation, line breaking, table layout, pagination, PDF
writing — is custom code in this repository, scoped to flowed documents rather than arbitrary web
pages. That scoping is exactly why it can skip the browser process.

## Maturity
Emerging — created 2026-07-20, actively pushed (commits as recently as today), MIT license, CI
configured, published on crates.io, RubyGems, and docs.rs. 378 stars, 21 forks, 2 subscribers —
small community for a young project, but real published packages across three registries is a
stronger signal than stars alone.

## License
MIT. No restrictions on commercial use, SaaS deployment, or embedding.

## Alternatives
- wkhtmltopdf — effectively abandoned; what this explicitly replaces.
- wicked_pdf — the Ruby-ecosystem wrapper around wkhtmltopdf; this project ships a one-by-one
  migration guide from it.
- Puppeteer/Playwright-driven PDF generation — the common modern approach; heavier (full browser
  process) but handles arbitrary web pages and JavaScript, which this does not (yet) support.

## Risks / Limitations
- No JavaScript execution and no claim of pixel-perfect browser parity — explicitly out of scope
  for now, listed as a future goal.
- No native Node.js binding at review — integration requires a subprocess or HTTP call rather
  than an in-process library call from a Node app.
- Young project (created July 2026) with a small subscriber count; CSS coverage is partial (see
  the project's own limitations page).

## Recommendation
USE NOW (as a utility) — for the specific job of rendering flowed documents (invoices, receipts,
reports) to PDF, this is a lower-overhead drop-in for an existing wkhtmltopdf/Puppeteer-based
pipeline; not a general web-page-to-PDF tool.

## Change History
### 2026-11-07
Discovered via GitHub search (slot 4: data, search, documents, RAG). Today's Small but
High-Leverage Utility. First catalog entry.
