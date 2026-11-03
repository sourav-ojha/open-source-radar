# Kensaku

## Summary
A cross-platform local file-search launcher (Rust) that indexes file names, text inside documents (PDF/DOCX/EPUB and 50+ formats), and OCR'd text inside images and screenshots, plus semantic/natural-language queries. Nothing leaves the machine. Ships a desktop app, a `kensaku-cli`, and an MCP server.

## Why I Should Care
The closest comparable tools — Everything, Listary, PowerToys Run — only index filenames. Kensaku indexes file content, OCRs images, supports semantic queries like "the PDF about database indexing," and exposes all of it over MCP. That's directly useful for an agent trying to locate a referenced file without being told the exact path.

## Problems It Can Remove
Removes the "which folder did I put that in" problem for both a human and an agent, and removes the need to build a custom local-search MCP tool for an agent that needs to find files by content rather than name.

## Practical Uses
- Find a file by what was in it, not its filename.
- Search screenshots by OCR'd text (e.g. the one with a specific error message).
- Give a coding agent MCP access to search the filesystem for a referenced file.
- Natural-language time filters ("invoices from the last 30 days").

## Product Opportunities
None identified beyond its own use case — this is a personal-productivity tool, not a building block for a product.

## Agent / Automation Opportunities
MCP server over stdio (`kensaku-cli mcp`) or HTTP with a loopback-only bearer token (`kensaku-cli serve`). Responses are size-budgeted and file content is returned as data, never as instructions — a sane design choice for feeding an LLM search results safely.

## Integration
Desktop installer per OS (unsigned builds, expected SmartScreen/Gatekeeper warning on first run) or build from source; `kensaku-cli` ships with every release. Low integration effort once installed; first-run indexing cost on a large disk was not tested here.

## Architecture Notes
Full-text + OCR + semantic indexing with sub-millisecond queries over 100,000+ files is the core engineering challenge; worth noting as a reference if ever building local search into another tool.

## Maturity
Experimental. Created 2026-10-24 (10 days old at review), v1.0.12 (multiple releases already), 613 stars / 44 forks / 3 subscribers / 9 open issues. 6 contributors, one dominant author.

## License
MIT, no restrictions.

## Alternatives
Everything (Windows, filenames only); Listary, PowerToys Run (filename/launcher only, no content/OCR); macOS Spotlight/mdfind (no OCR, no MCP).

## Risks / Limitations
- Pre-1.0 maturity in practice despite the v1.0.x version numbers; builds not yet code-signed.
- Model/OCR packs downloaded on first run — initial indexing cost on a large disk untested here.
- One dominant author among 6 contributors — bus-factor risk.

## Recommendation
PROTOTYPE — worth installing and running against a real machine's file set for a week to judge real-world OCR/semantic search quality before relying on it.

## Change History
### 2026-11-03
First seen and reviewed. Discovered via GitHub search for recently created developer-productivity tools; verified license, stars/forks/subscribers via API directly.
