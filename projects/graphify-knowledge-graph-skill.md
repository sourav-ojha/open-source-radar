# Graphify

## Summary
A Claude Code skill (`/graphify`) that turns any mixed folder — source code, markdown
docs, PDFs, screenshots, whiteboard photos — into a queryable, persistent knowledge
graph. Code is parsed deterministically via tree-sitter; everything else goes through
Claude vision/text extraction. No vector store, no server: everything runs locally and
produces a graph.json plus an interactive HTML viewer.

## Why I Should Care
This is both a codebase-onboarding tool and a "second brain" for scattered personal
research (papers, screenshots, notes) — the multimodal input handling is what separates
it from code-only graph tools already in the catalog (ripwire).

## Problems It Can Remove
- Re-reading whole files/folders repeatedly to reconstruct how a codebase or a pile of
  notes/papers relates together.
- Manually building an onboarding doc or internal wiki for a new codebase.
- Losing track of how a scattered "raw notes" folder (the Karpathy `/raw` pattern cited
  in the README) actually connects.

## Practical Uses
- `/graphify .` on a codebase to get a structural map plus a wiki an agent can navigate
  by reading files instead of parsing JSON.
- `/graphify add <arxiv-url>` / `add <tweet-url>` to pull external material straight into
  the graph.
- `/graphify query "..."` / `path A B` / `explain "X"` to answer structural questions
  without manually re-reading source.
- `graphify hook install` to auto-rebuild the graph after every commit, keeping it
  current for multiple agents working in parallel.
- `--wiki` export as an agent-crawlable internal knowledge base with zero manual writing.

## Product Opportunities
The "god nodes / surprising connections / suggested questions" report format is a
reusable onboarding artifact — could be generated once per repo and handed to any new
hire or agent as a starting point instead of a stale README.

## Agent / Automation Opportunities
Ships a native MCP stdio server (`--mcp`) so any MCP-compatible agent can query the graph
directly. Claims 71.5x fewer tokens per query on a mixed corpus vs. reading raw files,
with worked examples checked into the repo (`worked/`) so the number can be verified
rather than taken on faith.

## Integration
`pip install graphifyy && graphify install` (package temporarily renamed during a PyPI
name dispute — CLI/skill command is still `graphify`). Requires Claude Code and Python
3.10+. No server, no database — everything is local files. Integration effort: **Low**.

## Architecture Notes
NetworkX + Leiden community detection (via graspologic) + tree-sitter for code AST +
Claude for concept/relationship extraction on non-code content + vis.js for the
interactive viewer. Every edge is explicitly tagged `EXTRACTED`, `INFERRED`, or
`AMBIGUOUS`, so the output is honest about what was found vs. guessed rather than
presenting inference as fact.

## Maturity
Created 2026-04-03, 118,116 stars, 11,415 forks, 266 contributors, current release
v0.9.62 (2026-09-15). Verified via the GitHub API that this is not a fork
(`fork: false`) and that watchers_count matches stargazers_count, ruling out the most
obvious kind of star-count anomaly. The growth curve is unusually steep for a five-month
old project — worth continued observation, but the contributor count and worked-examples
transparency argue against it being a shallow hype vehicle.

## License
Apache-2.0. No commercial restrictions.

## Alternatives
- ripwire (already catalogued) — deterministic, code-only call-graph/blast-radius tool
  for "what does this change touch." Graphify is broader (docs, PDFs, images) and
  concept-oriented rather than change-impact-oriented — complementary, not competing.
- Traditional RAG/vector-store pipelines — Graphify explicitly avoids embeddings in favor
  of a deterministic graph with honest edge provenance labeling.

## Risks / Limitations
- PyPI package name mismatch (`graphifyy` vs. `graphify`) is a live, unresolved
  trademark/name situation — worth checking before recommending broadly.
- Non-code extraction (docs, PDFs, images) calls Claude directly, so cost scales with
  corpus size; the in-repo benchmark is honest about this but it is a real, ongoing cost.
- Explosive growth (118k stars in ~5 months) warrants normal skepticism about sustained
  maintenance quality even though the fork/watcher check and contributor count look
  legitimate.

## Recommendation
**PROTOTYPE** — Run `/graphify` on one real, mixed corpus (a current codebase plus its
docs, or a personal notes/papers folder) and check the worked-example claims hold up
before making it a standing part of the workflow.

## Change History
### 2026-09-16
Initial discovery and review. Rotation slot 1 (AI agents, MCP, coding productivity).
