# Code-Graph-RAG

## Summary
Parses a multi-language codebase with Tree-sitter (plus an ast-grep fallback tier and
compiler-grade frontends where available) into a code knowledge graph, stored in an
embedded SQLite file by default — no database server required — with Memgraph/Neo4j
and Qdrant available as opt-in backends for scale. Published to PyPI as `cgr`;
supports natural-language querying and structured editing of the graph.

## Why I Should Care
A zero-dependency, embeddable way to get structural/RAG-style queries over a
multi-language monorepo (Python, TypeScript, JS, Go, Rust, Java, C/C++, C#, PHP and
more) without running a separate graph database, and with real engineering quality
signals (Codecov, SonarCloud, OpenSSF Scorecard/Best Practices) rather than just a
marketing README.

## Problems It Can Remove
Many ad hoc grep/file-read round trips an agent would otherwise need to understand
codebase structure; hand-rolled recursive SQL for relationship queries.

## Practical Uses
- Ask structural questions about an unfamiliar multi-language repo without many manual
  file reads.
- Query and edit a codebase's knowledge graph directly inside an agent session.
- Start with the zero-server embedded-SQLite backend on a single repo, then scale to
  Memgraph+Qdrant for a large monorepo without changing the query interface.

## Product Opportunities
None specific — this is developer/agent tooling rather than an embeddable product
component.

## Agent / Automation Opportunities
A coding-agent-facing structural-query layer that complements raw grep/file-read tool
use with graph queries and explicit data-flow edges. Zero-server default install makes
it droppable into a CI job or agent sandbox without provisioning a database.

## Integration
`pipx`/`uv tool install` from PyPI (`code-graph-rag[treesitter-full,ast-grep,semantic]`).
Low effort by default (embedded SQLite, no Docker/cmake); Memgraph/Neo4j+Qdrant are
opt-in for larger corpora. Native extension requires CPython 3.12+ on Linux
x86_64/aarch64, macOS, or Windows x64.

## Architecture Notes
Pluggable graph-backend abstraction (embedded SQLite vs. Memgraph/Neo4j) running the
same Cypher queries against either; an ast-grep tier covers languages without a full
Tree-sitter grammar (Ruby, Kotlin, Swift, Elixir, Haskell, Solidity, Bash, Nix).

## Maturity
Emerging but well-engineered: created 2025-06-16 (~17 months), 5,393 stars, 726 forks,
35 subscribers, 304 open issues. Trendshift-featured. Latest release v0.7.1
(2026-11-17).

## License
MIT — no restrictions. A separate paid "Enterprise Support & Services" offering exists
but does not gate open-source functionality.

## Alternatives
Graphify (already catalogued, PROTOTYPE 8.5) — broader input scope (docs/PDFs/
screenshots via Claude vision, not just code) and ships as a Claude Code skill;
Code-Graph-RAG is code-only but has a more conventional embeddable-library/CLI shape,
explicit data-flow edges, and more conventional CI/quality tooling. ripwire (already
catalogued) — change-impact/blast-radius focused rather than general structural RAG.

## Risks / Limitations
Overlaps enough with the already-catalogued Graphify that running both would be
redundant for most use cases — pick based on whether the corpus is code-only (this) or
mixed-media (Graphify). Native-extension platform constraints limit install targets.

## Recommendation
PROTOTYPE — try it against one real multi-language monorepo already on hand and
compare the structural-query experience directly against Graphify before picking one.

## Change History
### 2026-11-21
First reviewed and catalogued.
