# LodeDB

## Summary
An embedded, on-disk vector database (Rust "TurboVec" core, Python package) positioned as a
drop-in durable backend for LangChain, LlamaIndex, and mem0. Vendor-reported benchmarks
claim large footprint and latency wins over each framework's default store, and over
sqlite-vec/qdrant/pgvector/lancedb/chroma directly. Adds GPU-resident batched search,
incremental (O(changed)) persistence, multimodal (CLIP) indexing, and Swift/iOS bindings.

## Why I Should Care
Most "embedded vector DB" comparisons stop at sqlite-vec/LanceDB; LodeDB's own benchmark
table claims a further order-of-magnitude-plus win on durable writes and single-query
latency against those two specifically, plus drop-in adapters for the three frameworks
(LangChain, LlamaIndex, mem0) most likely to already be in an AI stack.

## Problems It Can Remove
Removes the "stand up a vector DB service" step for a self-hosted product feature (semantic
search, agent memory) — no daemon, no account, no API key, just a local file.

## Practical Uses
- Swap in as the vector store behind an existing LangChain/LlamaIndex/mem0 integration for a large drop in disk footprint and query latency, per the project's own benchmarks
- Local agent memory backend for a coding assistant via its MCP server and one-line install (`lodedb mcp install --client claude-code`)
- GPU-resident batched search for a self-hosted semantic search feature without standing up Qdrant/Milvus
- `lodedb migrate` to move an existing pgvector/Qdrant/etc. store onto a local file, via an inspect/plan/dry-run/run/validate path

## Product Opportunities
A single-file embedded vector store removes the "stand up a vector DB service" step for a
self-hosted product feature — worth prototyping as a retrieval backend before committing to
a hosted vector DB bill.

## Agent / Automation Opportunities
MCP server ships specifically for coding-assistant memory use; `lodedb mcp install` targets
Claude Code, Claude Desktop, Cursor, Codex, and LM Studio directly.

## Integration
`pip install "lodedb[embeddings]"` — prebuilt wheels for Linux, macOS (Apple Silicon and
Intel), and Windows on Python 3.11+, nothing to compile. Low effort for a Python service;
CLI includes a `lodedb doctor` health check.

## Architecture Notes
MIT-licensed "TurboVec" Rust core packs vectors into 2/4-bit codes with SIMD CPU kernels;
commits are crash-atomic and only persist changed rows (173x–1,308x faster than a full
rewrite per their numbers). GPU path copies a float32 index to the GPU and scores with a
cuBLAS GEMM plus on-device top-k. Text is stored zstd-compressed by default.

## Maturity
Emerging. Created 2026-06-14 (~3 months old at review), 98 stars, 2 contributors, 207
commits. Company-backed (Egoist Machines, Inc.) with a real benchmark suite and adapter
ecosystem, but still early.

## License
Apache-2.0 for the core — unrestricted for commercial use, embedding, and redistribution.
A separate paid Enterprise tier exists for commercial support and managed/BYOC deployment;
optional, not required to use the open core.

## Alternatives
sqlite-vec, LanceDB, Qdrant, pgvector, Chroma, Milvus.

## Risks / Limitations
- Very new (~3 months old) — be cautious about production reliance before more track record
- Only 2 contributors despite the surface area
- Benchmarks are the vendor's own numbers on their own hardware, not yet independently reproduced
- Windows CUDA embeddings need a manual PyTorch reinstall — a rough edge the project's own docs call out (with a `doctor --fix`)

## Recommendation
PROTOTYPE — worth a hands-on latency/footprint test against whatever vector store currently
backs any LangChain/LlamaIndex/mem0 usage, but hold off on treating the benchmark numbers as
verified until tested independently.

## Change History
### 2026-09-05
Discovered via GitHub search (rotation slot 4: data, search, documents, RAG). Catalogued
at PROTOTYPE, 7.9/10.
