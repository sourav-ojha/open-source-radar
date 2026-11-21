# Paperless-NGX Dedupe

## Summary
A companion service for the self-hosted document manager paperless-ngx: syncs
documents, finds near-duplicates via MinHash/LSH fuzzy matching (with a discriminative
penalty to avoid false positives on boilerplate-heavy documents), and optionally
extracts metadata (correspondents, document types, tags) with an LLM, surfacing
confidence scores and evidence snippets for review before applying. Single-container
Docker deployment with embedded SQLite and OpenTelemetry observability built in.

## Why I Should Care
A narrow, single-purpose add-on that removes a specific recurring chore — manually
finding and merging duplicate scans — for anyone already running paperless-ngx as
self-hosted document infrastructure.

## Problems It Can Remove
Manually scanning a growing paperless-ngx library for duplicate uploads; manually
tagging/classifying a large backlog of untagged documents.

## Practical Uses
- Run against an existing paperless-ngx instance to find and clear out duplicate scans
  accumulated over time.
- Use the AI metadata suggestions to backfill correspondents/tags on a large existing
  document library.
- Export a CSV duplicate report for a one-time library cleanup.

## Product Opportunities
None — purely an operational add-on for an existing self-hosted tool.

## Agent / Automation Opportunities
None beyond the optional LLM-based metadata extraction step, which is a one-shot
classification call rather than an agent loop.

## Integration
Single Docker container, embedded SQLite, connects to an existing paperless-ngx
instance via its API. Low effort.

## Architecture Notes
MinHash signatures + Locality-Sensitive Hashing give O(n log n) candidate discovery
instead of pairwise comparison; a discriminative penalty down-scores pairs that only
share boilerplate text, reducing false-positive duplicate matches.

## Maturity
Emerging: created 2025-08-15 (~15 months), 28 stars, 2 forks, 0 subscribers, 18 open
issues. Latest tagged release v0.18.1 (2026-08-11); repo still actively pushed.

## License
GPL-3.0 — copyleft. Fine for internal/self-hosted use; redistributing a modified
version requires releasing source.

## Alternatives
Manual review inside paperless-ngx itself — what this replaces for any library past a
few hundred documents.

## Risks / Limitations
Only useful if/when paperless-ngx is actually adopted — not standalone infrastructure.
GPL-3.0 copyleft applies to redistribution of modifications.

## Recommendation
USE NOW (conditional on running paperless-ngx) — today's Small but High-Leverage
Utility pick.

## Change History
### 2026-11-21
First reviewed and catalogued.
