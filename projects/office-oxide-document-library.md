# Office Oxide

## Summary
A memory-safe Rust library for text extraction from Office documents (DOCX, XLSX, PPTX,
DOC, XLS, PPT) with first-class native bindings for Python, Go, C#/.NET, Node.js (native
and WASM), and a stable C FFI. Claims 5-10x the speed of python-docx/openpyxl/python-pptx
/xlrd and a 97.3% open rate across a 10,386-file test corpus.

## Why I Should Care
Office document parsing in a Node/TS stack usually means either a thin, format-limited JS
library or shelling out to a Python/JVM process. This gives one native binding — including
a real Node.js package, not just a WASM shim — with benchmarked performance across six
Office formats, from a single maintained codebase.

## Problems It Can Remove
Removes the need to run a separate Python microservice (or LibreOffice headless) just to
extract text from uploaded Office documents. Removes the slow-parsing tax of
python-docx/openpyxl-style libraries when that path is already in a pipeline.

## Practical Uses
- Extract text/data from user-uploaded DOCX/XLSX/PPTX files inside a Node.js backend
  without a separate parsing service.
- Replace a slow python-docx/openpyxl ingestion step with a native binding 5-10x faster
  per the project's benchmarks.
- Parse legacy DOC/XLS/PPT uploads (not just modern XML-based formats) from the same
  library.
- Run the WASM build client-side in a browser for a privacy-preserving "preview this
  Office file" feature with no server round-trip.

## Product Opportunities
Backend component for any product feature that ingests customer-uploaded Office documents
(invoicing, reporting, contract review) without standing up a separate parsing service.

## Agent / Automation Opportunities
No MCP server; a library, so an agent tool wrapping it would need a thin CLI or API shim.

## Integration
Deployment: library via npm (native + WASM), PyPI, crates.io, NuGet, or Go module —
embeddable directly in a Node.js/TS backend. Integration effort: **Low**.

## Architecture Notes
Single Rust core with bindings generated/maintained for six language ecosystems rather
than six independent reimplementations — worth noting as a reference approach for "ship
one correctness-critical parser, bind it everywhere" rather than maintaining parallel
implementations per language.

## Maturity
Emerging. Created 2026-03-01 (~8 months), 167 stars, 27 forks, 1 subscriber. CI configured,
docs.rs documentation, latest release v0.1.17 (2026-11-11). Not archived.

## License
Dual MIT/Apache-2.0, no restrictions on commercial use, SaaS deployment, or embedding.

## Alternatives
- python-docx / openpyxl / python-pptx / xlrd — Python-only, slower per the project's own
  benchmarks.
- Apache POI / Tika — JVM-dependent, different deployment shape, explicitly out of scope
  for this project's own comparison.
- mammoth.js / docx4js — Node-native but narrower format coverage.

## Risks / Limitations
- 167 stars / 1 subscriber / 27 forks, created March 2026 — young; verify the claimed
  97.3% real-world open rate against your own document corpus before depending on it for
  user-facing uploads.
- Text extraction only — not a full Office rendering/editing library; formatting
  preservation or document generation needs a different tool.

## Recommendation
PROTOTYPE — try the Node native binding against a real sample of user-uploaded Office
documents (including a few legacy DOC/XLS/PPT files) before replacing an existing
Python-based parsing step.

## Change History
### 2026-11-14
Initial discovery and review. GitHub API verified: dual MIT/Apache-2.0, 167 stars, 27
forks, 1 subscriber, created 2026-03-01, pushed_at 2026-11-11, not archived.
