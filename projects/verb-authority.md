# Verb Authority

## Summary
A Python library that scans exported AI-agent tool schemas (including MCP schemas) and builds a per-argument "authority map" distinguishing which tool-call arguments must be supplied by trusted application code versus which may legitimately be filled by the model or by untrusted retrieved data — then provides a local runtime gate that blocks a call when an untrusted source tries to author a value flagged as trusted-only.

## Why I Should Care
Standard JSON Schema tool definitions treat all string arguments identically — a schema has no way to say "the `to` field of `send_email` must come from authenticated session data, not from a webpage the model just read." That gap is exactly the shape of a large class of prompt-injection attacks against tool-calling agents: a poisoned document or tool result rewrites a "safe" argument. Verb Authority targets this narrow but real gap directly, which is squarely relevant to any agent tooling Sourav builds and to the MSSP/security side of his work.

## Problems It Can Remove
Removes the need to hand-write per-tool authority checks and audit logic for agent tool-call arguments — a category of validation that tool schemas alone cannot express and that is easy to skip under deadline pressure.

## Practical Uses
- Scan tool schemas for any custom MCP server or agent tool before shipping it, to get a reviewable map of which arguments are injection-exposed.
- Drop the runtime gate in front of any tool with a genuinely sensitive fixed argument (recipient addresses, destination URLs, account IDs) in an agent pipeline.
- Use the offline quickstart (`python -m verb_authority quickstart`) as a teaching example for what "argument authority" means when reviewing other people's agent tools.

## Product Opportunities
The authority-mapping concept is generic enough to be pitched as a security review step inside an MSSP offering for clients building or exposing AI agent tools — a genuine "AI agent AppSec" angle beyond Sourav's own use.

## Agent / Automation Opportunities
Directly an agent-security building block — schema scanner + runtime gate, usable standalone or with the optional Pydantic AI adapter, and the docs confirm it has been exercised against a real frozen Playwright `browser_tabs` schema, not just synthetic examples.

## Integration
`pip install "verb-authority==0.10.0b14"` (PyPI-confirmed), Python 3.10–3.14, no network or model calls during scanning. Python-only; a Node/TS agent stack would need to run it as a separate check step (e.g., in CI against exported schemas) rather than inline. Integration effort: **Low** for schema auditing as a CI step, **Medium** for wiring the runtime gate into a live non-Python agent process.

## Architecture Notes
Separates two concerns cleanly: (1) static schema-scan producing a reviewable authority map, and (2) a runtime boundary enforced immediately before tool execution — the same distinction Sourav would want in any of his own agent-facing product surfaces (schema is a contract, authority is a runtime-enforced property, not a shape).

## Maturity
Explicitly self-described by the maintainer as "early, research-grade work" and "not described as production-ready." v0.9.0 is the latest stable tag; v0.10.0-beta.14 is the first PyPI distribution. ~86 commits from 2 contributors since creation (2026-05-26) — sustained, careful development (beta numbering with skipped identifiers, a documented case-study/evidence directory, a "Tool Authority Atlas") rather than a weekend drop, despite 0 GitHub stars at review time.

## License
Apache-2.0 (GitHub API-confirmed). No commercial-use restrictions.

## Alternatives
General prompt-injection defenses (input sanitization, LLM-based classifiers on tool results) that operate on content rather than on argument provenance; most agent frameworks currently have no equivalent of this at all.

## Risks / Limitations
- 0 GitHub stars, pre-1.0, maintainer explicitly disclaims production-readiness — do not depend on it for anything client-facing without independent verification.
- Python-only, so a JS/TS-first agent stack gets it as an offline audit tool, not an inline runtime dependency, without extra plumbing.
- Narrow scope by design: solves argument-provenance, not the full space of prompt-injection risks.

## Recommendation
PROTOTYPE — run the schema scanner against any MCP server or agent tool Sourav builds or evaluates as a free audit step; treat the runtime gate as an experiment, not a production control, given the maintainer's own caveats.

## Change History
### 2026-09-01
Initial discovery and review. Rotation slot 7 (experimental / hidden gems).
