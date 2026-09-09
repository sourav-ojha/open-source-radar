# ripwire

## Summary
ripwire is a zero-dependency, single-binary C++23 CLI and MCP server that builds a
ranked, deterministic call graph of a repository — what a change touches, what it
breaks, and which tests to run — so a coding agent can act on a map instead of grepping
and reading whole files first.

## Why I Should Care
Token spend and time on "figure out the codebase first" is one of the biggest hidden
costs of agent-driven development. A precomputed, deterministic blast-radius map that an
agent queries instead of exploring by hand is a direct, measurable reduction in both.

## Problems It Can Remove
- An agent reading whole files or repeatedly grepping to understand call relationships
  before making a change.
- Manually figuring out which tests are relevant to a given change.
- Ad-hoc, per-project scripts for "what does this function touch" that most teams never
  get around to writing.

## Practical Uses
- Point it at a repo before letting an agent make a change, so the agent gets a call
  graph instead of exploring blind.
- Use it to identify blast radius and relevant tests before merging an agent-authored
  change.
- Run it as an MCP server so any MCP-compatible agent can query the graph directly
  instead of shelling out to the CLI.

## Product Opportunities
- The "context map before code read" pattern is reusable as a pre-processing step in any
  internal agent-orchestration pipeline, not just interactively.

## Agent / Automation Opportunities
- Ships both a CLI and a native MCP server — usable as a direct agent tool without a
  wrapper.
- Positions itself explicitly as pre-flight context for a coding agent (find what to
  touch) and post-flight verification (check what was actually built matches intent).

## Integration
Single compiled executable, zero runtime dependencies, supports a long list of languages
(Rust, C++, Objective-C/C++, C, Python, Go, Swift, TypeScript/JavaScript, Java, Ruby,
PHP, Lua, Elixir, Bash, C#, and structured formats). Integration effort: **Low** — no
API key, no embeddings, no external service; drop the binary in and point it at a repo.

## Architecture Notes
Deterministic, non-embedding-based call-graph construction (no vector search, no API
key) is a meaningfully different architecture from the embedding-heavy "codebase RAG"
tools in this space, and worth studying even independent of adoption for that reason
alone.

## Maturity
Emerging, with unusually fast traction: created 2026-07-29 (~6 weeks old) but already at
1,780 stars, hosted under Red Hat's experimental/incubation GitHub org (`redhat-et`).
Despite the org affiliation, commit history is effectively single-author (1,944 commits
from one contributor vs. single digits from the next seven). The README leans heavily on
self-reported, self-verified benchmark claims (specific byte-reduction percentages,
counts of "papers folded," a script that checks the README's own claims against its own
tables) — a pattern worth treating with the same caution as previously-flagged
single-author, marketing-forward projects in this catalog (see trace-mcp, 2026-09-08).
Star velocity for a 6-week-old repo is itself worth noting as unusual and not, on its
own, evidence of merit.

## License
Apache-2.0 — no commercial-use restrictions identified.

## Alternatives
Embedding-based codebase-RAG/context tools (many catalogued or discovered this run under
"agent memory"); trace-mcp (already catalogued, 2026-09-08) for framework-aware codebase
graphs — different approach (framework-specific vs. deterministic multi-language call
graph) and worth comparing directly if adopting either.

## Risks / Limitations
- Effectively single-author despite the Red Hat org affiliation — verify whether this is
  an official, maintained Red Hat effort or a personal project hosted under the org
  before relying on it long-term.
- Headline claims (byte-reduction percentages, "50 years of software-engineering
  results") are self-verified by the project's own test script, not independently
  audited — treat as unconfirmed until checked hands-on.
- Very new; expect rapid, possibly breaking iteration.

## Recommendation
PROTOTYPE — the deterministic call-graph approach and zero-dependency single binary are
worth testing directly against a real repo, but verify the specific claims (byte
reduction, blast-radius accuracy) hands-on rather than taking the README's own
self-checks at face value.

## Change History
### 2026-09-09
Initial discovery and review. Slot 1 (AI agents, MCP, coding productivity) run.
