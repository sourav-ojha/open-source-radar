# model-compose

## Summary
A declarative, docker-compose-style way to define AI systems in one YAML file: chat APIs, RAG pipelines, ReAct agents, and MCP servers, composed from interchangeable components (models, agents, tools, memory) with no orchestration code. Supports local models and cloud APIs interchangeably, ships native drivers for Chroma/Milvus/Qdrant/FAISS/Neo4j/ArangoDB/Redis, and can expose any defined workflow as an MCP server by changing one adapter line.

## Why I Should Care
Collapses the repetitive orchestration boilerplate (component wiring, streaming, MCP exposure) that gets rebuilt per AI-pipeline project into one portable, git-friendly YAML file — useful for validating an AI product idea's pipeline shape before investing in custom code.

## Problems It Can Remove
Replaces hand-written Python glue code for chaining models, retrieval, and tools. Replaces custom MCP server wrapping — a workflow becomes an MCP server with a one-line adapter change.

## Practical Uses
- Quickly prototyping a RAG pipeline (embed → retrieve → generate) without writing glue code
- Turning an existing workflow into an MCP server for Claude/Cursor/ChatGPT with a one-line config change
- Defining a ReAct agent (tools, system prompt, iteration limit) declaratively for fast iteration
- Validating an AI product idea's pipeline shape before committing to hand-written orchestration code

## Product Opportunities
Fast way to stand up an internal AI-pipeline prototype to validate a product idea before building it properly in the real stack.

## Agent / Automation Opportunities
Directly exposes agents and MCP servers as first-class YAML constructs — one of the more direct MCP-authoring shortcuts reviewed recently; no code required to turn a defined workflow into an MCP tool.

## Integration
`pip install model-compose` or `uv pip install`, define `model-compose.yml`, run `model-compose up`. Same file runs locally, in Docker, or in production. Integration effort: **Low**.

## Architecture Notes
Four stated design principles: composable (interchangeable building blocks), portable (define once, run anywhere), model-agnostic (mix local and cloud models), stream-native (tokens/audio/video as first-class values flowing through workflows).

## Maturity
Emerging. Created May 2025, 117 stars, 17 forks, 5 open issues. Apparent single-maintainer project (hanyeol). GitHub release tags are stale (latest tag v0.4.0 from September 2025) despite active daily commits and PyPI publishing continuing to 0.4.113 as of this run — track the PyPI version, not the GitHub release tag, for currency.

## License
MIT, verified via GitHub API. No restrictions on commercial use, SaaS deployment, or embedding.

## Alternatives
- **LangChain/LlamaIndex** (mainstream, excluded from fresh discovery) — model-compose's difference is declarative YAML with zero Python glue code rather than a Python framework to code against.
- **n8n** (mainstream, excluded from fresh discovery) — model-compose is code-first and git-friendly (a text file, not a visual canvas) and ships native vector-DB drivers out of the box.

## Risks / Limitations
- Single-maintainer project — real bus-factor and longevity risk beyond prototyping use.
- Weak GitHub release-tagging discipline despite continuous PyPI publishing; don't rely on GitHub releases to judge currency.
- Declarative YAML pipelines hit a complexity ceiling quickly for genuinely custom agent logic.

## Recommendation
**PROTOTYPE** — low integration cost makes it worth trialing for quick AI-pipeline prototypes despite the maturity and bus-factor risk; not a long-term production dependency as-is.

## Change History
### 2026-10-01
Initial discovery and review. Found via GitHub Search API workflow-engine query, slot 2 (product infrastructure) run; AI-pipeline angle treated as an exceptional out-of-slot find.
