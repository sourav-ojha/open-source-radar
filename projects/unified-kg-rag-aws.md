# Unified Knowledge Graph RAG on AWS

## Summary
An AWS Labs framework that runs four published graph-RAG methods — Microsoft GraphRAG
(community summaries), LightRAG (dual-level keywords), HippoRAG 2 (personalized PageRank over
entities/passages), and an opt-in RAPTOR summary tree — on one shared index, selectable per
query rather than per deployment. Hybrid retrieval fuses BM25, vector search, and Amazon Neptune
graph traversal with Reciprocal Rank Fusion, reranked with a Bedrock rerank model. Ships both a
fully local mode (SQLite + Ollama, no AWS account) and an AWS-native mode (Neptune + OpenSearch +
Bedrock).

## Why I Should Care
Graph-RAG is usually a research-paper reimplementation exercise: pick one method, hand-roll the
ingestion/indexing/eval harness, and commit to it. This collapses four methods behind one
ingestion pipeline and lets you compare them on the same corpus with a built-in evaluation CLI
(LLM-free metrics plus LLM judges, paired confidence intervals, per-query cost). Built and
maintained by AWS Labs, directly targeting the Bedrock/Neptune/OpenSearch stack already in scope
for the AWS side of the work.

## Problems It Can Remove
Replaces the custom plumbing of standing up GraphRAG or LightRAG from their reference repos:
document-status-aware incremental indexing (delete removes only what no other document states),
a seed-graph merge path for bringing in an existing KG (JSONL/CSV/RDF), multilingual analyzers,
and a measurement harness to decide which method is actually worth running in production instead
of assuming one paper's benchmark transfers.

## Practical Uses
- Comparing GraphRAG vs. LightRAG vs. HippoRAG 2 on a real corpus (internal docs, support
  tickets) before committing engineering time to one architecture.
- A Bedrock-native knowledge base for a client-facing support or research tool where answers
  must cite source passages.
- Prototyping locally with Ollama before deciding whether the AWS-native deployment (Neptune +
  OpenSearch + Bedrock) is worth the infrastructure cost.
- Merging an existing taxonomy/ontology (seed graph) into an LLM-extracted graph rather than
  starting from zero.

## Product Opportunities
A packaged "ask your documents" feature for a client-facing product where provenance (cited
source passages) matters — e.g., compliance or research-document Q&A — without building a
graph-RAG pipeline from scratch.

## Agent / Automation Opportunities
Exposes a CLI (`run-ingestion`, `run-rag`, `run-eval`); no MCP server yet, but the CLI surface is
narrow enough to wrap as an agent tool or MCP server directly. The evaluation CLI is itself useful
as an automation step in a CI pipeline for a RAG product (regression-test retrieval quality on
every corpus change).

## Integration
Local mode: `uv sync --extra local` + Ollama, no AWS account, five-minute quickstart confirmed in
docs. AWS mode: Bedrock + Neptune + OpenSearch, deployed via the repo's infra code — meaningfully
more setup than the local path. **Integration effort: Medium** — low for evaluation/prototyping
locally, medium-to-high once the Neptune/OpenSearch/Bedrock stack is involved.

## Architecture Notes
The "one index, many retrieval algorithms, selectable per query" design is the interesting part —
most graph-RAG tools hard-wire a single method at indexing time. Reciprocal Rank Fusion across
BM25/vector/graph plus a Bedrock rerank pass is a reasonable hybrid-retrieval default worth
studying even outside AWS.

## Maturity
Emerging — created 2026-07-20, actively pushed (commits as recently as today), Apache-2.0, CI
configured, no tagged GitHub release yet (version managed via `pyproject.toml`/docs rather than
release tags). AWS Labs org backing is a real maturity signal but this is not yet a 1.0 product.

## License
Apache-2.0. No restrictions on commercial use, modification, or redistribution.

## Alternatives
- Rolling your own GraphRAG/LightRAG from the original research repos — what this replaces.
- Amazon Bedrock Knowledge Bases GraphRAG — the README documents its own comparison against this;
  worth reading before choosing between the managed and self-hosted paths.
- Already-catalogued RAG infra (CocoIndex, Antfly, PageIndex) — none combine multiple graph-RAG
  methods behind one evaluation harness the way this does.

## Risks / Limitations
- No tagged release yet — track `pushed_at` and commits rather than a version number.
- Full AWS-native mode requires Neptune + OpenSearch + Bedrock, a real infrastructure and cost
  commitment relative to the five-minute local quickstart.
- 39 stars / 0 subscribers at review — an AWS Labs repo, not yet broadly adopted; treat benchmark
  numbers as the project's own until independently reproduced.

## Recommendation
PROTOTYPE — worth running the local quickstart against a real document set to decide whether any
of the four methods clears the bar for a client-facing Q&A feature, before considering the
AWS-native deployment.

## Change History
### 2026-11-07
Discovered via GitHub search (slot 4: data, search, documents, RAG). First catalog entry.
