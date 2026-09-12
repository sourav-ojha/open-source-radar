# Dedoc

## Summary
Dedoc is a Python library and service (Apache-2.0, maintained by ISPRAS since 2020) that parses 30+ document formats — PDF (including scanned), DOCX, HTML, images, and more — and extracts content, logical structure, tables, and metadata into one uniform structured representation, with OCR fallback for scanned documents.

## Why I Should Care
Every document-ingestion project ends up writing per-format glue code (one path for PDFs, another for DOCX, another for scanned images). Dedoc already handles that matrix and exposes it as either a Python library or a Dockerized REST service, so ingestion doesn't have to be coupled to the app's language runtime.

## Problems It Can Remove
Replaces hand-rolled per-format parsers and ad-hoc OCR fallback logic for scanned documents. Removes the need to evaluate table-extraction heuristics separately per format.

## Practical Uses
- Ingest heterogeneous document types into a RAG pipeline with one consistent structured output.
- Extract tables and logical structure from scanned or legal/financial PDFs.
- Run as a standalone REST service so document parsing can live outside the main app process.

## Product Opportunities
Could be the ingestion layer of a document-intelligence micro-SaaS, or power a generic "upload any document" import feature without per-format code.

## Agent / Automation Opportunities
Usable as a tool call in an agent pipeline (parse-then-answer over arbitrary uploaded documents); REST service makes it easy to wrap as an MCP tool.

## Integration
`pip install dedoc` or `docker run` the provided image; call as a library or hit the REST API. Low effort — no other infra required for the core parsing path.

## Architecture Notes
Produces a uniform JSON tree (logical structure + tables + metadata) regardless of input format, which is the part worth studying even independent of adoption — a consistent intermediate representation is what makes downstream RAG chunking reliable.

## Maturity
Mature — created December 2020, 734 stars, active development, latest release v2.7 (2026-06-25). 14 open issues.

## License
Apache-2.0. No restrictions on commercial or SaaS use, redistribution, or embedding.

## Alternatives
OpenDataLoader PDF (catalogued, PDF-only), unstructured.io, Docling, Apache Tika.

## Risks / Limitations
Maintained by ISPRAS (a Russian Academy of Sciences institute) — worth noting for provenance-sensitive/compliance deployments even though the license itself imposes no restriction. Open-issue count suggests some rough edges in less-common format paths.

## Recommendation
USE NOW — broader format coverage than PDF-only tools already catalogued, mature and actively maintained, low integration effort.

## Change History
### 2026-09-12
Initial discovery and review.
