# Headroom

## Summary
Headroom is a local context-compression layer for AI agents: it compresses tool
outputs, logs, files, RAG chunks, and conversation history before they reach the LLM
(and trims what the model writes back), usable as a Python/TypeScript library, a
transparent local proxy, an MCP server, or a one-command wrapper around 15+ agent CLIs
(Claude, Codex, Cursor, Aider, OpenCode, Goose, and more).

## Why I Should Care
This attacks token cost directly on the coding-agent sessions already run daily, with a
wrap-and-forget integration model — `headroom wrap claude` requires no workflow change.

## Problems It Can Remove
- Burning tokens on verbose tool outputs, log dumps, and JSON blobs that an agent only
  needs a fraction of.
- Hand-rolled truncation/summarization logic for tool output (the actual status quo in
  most agent setups today).
- Each agent (Claude, Codex, Gemini, Grok) re-reading the same files independently with
  no shared memory between them.

## Practical Uses
- `headroom wrap claude` / `codex` / `cursor` / `aider` — wrap any supported CLI, undo
  with `headroom unwrap`.
- `headroom proxy --port 8787` in front of any OpenAI-compatible endpoint for a
  language-agnostic, zero-code-change integration.
- Inline library use: `from headroom import compress` in a Python service, or the
  TypeScript SDK equivalent.
- `headroom learn` mines failed sessions and writes corrections back into
  CLAUDE.md/AGENTS.md/GEMINI.md automatically.
- Cross-agent shared memory store with automatic dedup across Claude/Codex/Gemini/Grok.

## Product Opportunities
The CCR (compress-cache-retrieve) pattern — store the original locally, let the model
call back for full detail only when it actually needs it — is a reusable design for any
internal tool-output or logging pipeline that feeds an LLM.

## Agent / Automation Opportunities
MCP server exposes `headroom_compress` / `headroom_retrieve` / `headroom_stats` directly
to any MCP client. `headroom dashboard` gives live savings visibility while a proxy is
running.

## Integration
`pip install "headroom-ai[all]"`, `uv tool install`, or `npm install headroom-ai`
(TypeScript SDK only, no CLI). No server required for library/wrap use; proxy mode is a
local process. Integration effort: **Low** for wrap/library use; slightly higher on
Intel macOS, where the ONNX Runtime dependency for content-detection/embedding features
has no prebuilt binary (documented workaround via Homebrew).

## Architecture Notes
A `ContentRouter` detects content type and dispatches to a `SmartCrusher` (JSON), a
`CodeCompressor` (AST-based), or `Kompress-v2-base` (a small purpose-trained model on
Hugging Face, for prose) — plus a `CacheAligner` that flags volatile content which would
otherwise bust a provider's KV-cache prefix, without rewriting prompts itself. All
compression runs locally; the README states no prompt/file content leaves the machine.

## Maturity
Created 2026-01-07, 72,362 stars, 265 contributors, current release v0.37.0
(2026-08-27). Trendshift "#1 Repository Of The Day" badge on the README. Legitimate
scale per contributor count and API-verified star count, though young relative to how
large it already is.

## License
Apache-2.0. No commercial restrictions.

## Alternatives
- mcp-compressor (already catalogued, Atlassian Labs) — compresses MCP tool
  *schemas/descriptions* so an agent doesn't burn tokens on a large tool surface before
  picking one. Headroom compresses tool-call *outputs*, logs, and RAG chunks after the
  fact. Different layer of the same overall token-cost problem — complementary, not a
  straight substitute.
- Manual truncation/summarization of tool output (the real-world default today).

## Risks / Limitations
- ONNX-backed features need a precompiled ONNX Runtime; Intel macOS has no prebuilt
  binary as of a currently open issue, requiring a manual `brew install onnxruntime`
  workaround.
- Compression is inherently lossy in the general case; correctness depends on the router
  picking the right compressor per content type and the CCR retrieval path actually
  firing when detail is needed.
- Scale (72k stars) arrived fast for an eight-month-old project — worth normal
  skepticism about maintenance sustainability even with a large, real contributor base.

## Recommendation
**PROTOTYPE** — Trial `headroom wrap claude` on real daily sessions and check
`headroom dashboard` savings numbers before deciding whether to keep it wrapped
permanently or fold it into a CI/agent pipeline more broadly.

## Change History
### 2026-09-16
Initial discovery and review. Rotation slot 1 (AI agents, MCP, coding productivity).
