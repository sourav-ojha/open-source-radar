# DB Agent

## Summary
A minimal, production-oriented text-to-SQL agent: natural-language questions become SELECT-only
SQL, validated against a safety layer, executed, and returned with every intermediate step
visible (schema context, generated SQL, validation result). Primary implementation is a
Node.js/React app that runs locally in one command; also deployable to Databricks Apps. Includes
an MCP gateway so Claude Desktop/VS Code/Claude Code can query the same database through one set
of rules, with per-caller keys, budgets, and an audit log.

## Why I Should Care
This is a text-to-SQL agent built in the same stack already in use (Node.js/React) rather than
the Python-first tooling that dominates this niche (WrenAI, schemagate, Vanna). The "ontology"
feature — a knowledge file of descriptions/synonyms/example queries injected into the prompt,
auto-drafted from the schema via `db-agent scan`, with an accuracy test scoring with/without it —
is a concrete, measurable way to improve text-to-SQL accuracy rather than just prompting harder.

## Problems It Can Remove
Removes the need to hand-build a safe natural-language-to-SQL layer: SELECT-only validation
before anything reaches the database, per-answer LLM token cost and warehouse compute cost
(`EXPLAIN`/`EXPLAIN COST`) shown side by side with approval gating for expensive queries, and an
MCP gateway so multiple coding agents share one governed path to the database instead of each
agent getting raw credentials.

## Practical Uses
- A natural-language query layer over an internal Postgres/MySQL database for non-technical
  stakeholders, with a visible audit trail of what SQL actually ran.
- Giving Claude Code/Claude Desktop governed, budgeted access to a database via the MCP gateway
  instead of handing out raw DB credentials to an agent.
- Cross-agent memory (redacted, shared store — local JSONL or AWS S3 Vectors) so multiple agents
  working against the same database build on each other's query history.
- A reference implementation for building a scoped, auditable text-to-SQL feature into an
  existing Node/React admin panel.

## Product Opportunities
A "query your data in plain English" feature for an admin panel or internal tool, with the
cost-visibility and approval-gating features as a differentiator against agents that just run
whatever SQL the LLM produces.

## Agent / Automation Opportunities
The MCP gateway is the standout agent feature: one governed database connection shared across
Claude Desktop, VS Code, and Claude Code, with per-caller keys and budgets rather than each tool
getting its own credentials. Directly relevant to the MSSP/PTaaS secondary context as a pattern
for governed agent-to-database access.

## Integration
`docker-compose.yml` and a `render.yaml` for Render.com deployment; runs locally in one command
per the README. Any OpenAI-compatible LLM (including local Ollama/LM Studio). **Integration
effort: Low** — Node.js/React stack matches the existing toolset directly, Docker Compose for
local evaluation.

## Architecture Notes
The "two bills, both visible" framing (LLM token cost vs. warehouse compute cost, with approval
for expensive queries) is a detail most text-to-SQL agents skip and worth borrowing even if this
specific project isn't adopted — warehouse compute cost from a runaway generated query is a real
failure mode once an agent has write-adjacent query power.

## Maturity
Emerging — presented as a reference implementation at the AAAI-25 Workshop on Open-Source AI for
Mainstream Use (March 2025) and an AWS User Group Toronto workshop (November 2026); no tagged
GitHub release. Small community (31 stars, 32 forks, 1 subscriber) relative to its documented use
in two public workshops — more forks than stars suggests people are using/forking it for their
own deployments rather than just starring it.

## License
Apache-2.0, confirmed via the GitHub API. No restrictions on commercial use or embedding.

## Alternatives
- WrenAI (Canner), schemagate — already-catalogued text-to-SQL tools, both Python-first.
- Vanna — the mainstream baseline this niche is usually compared against.
- Hand-rolled "LLM writes raw SQL" agent — what the SELECT-only safety layer and MCP gateway here
  specifically replace.

## Risks / Limitations
- No tagged release — track commits/pushed_at rather than a version number.
- Small subscriber count (1) for a project referenced in two public workshops; independent
  production usage beyond the author's own deployments is unconfirmed.
- SELECT-only validation is the stated safety boundary; treat it as a first layer, not a
  substitute for database-level read-only credentials in a production deployment.

## Recommendation
PROTOTYPE — worth trying directly against an internal Postgres database given the stack match
(Node/React) and the MCP-gateway pattern for governed agent database access.

## Change History
### 2026-11-07
Discovered via GitHub search (slot 4: data, search, documents, RAG). First catalog entry.
