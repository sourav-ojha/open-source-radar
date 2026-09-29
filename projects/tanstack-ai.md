# TanStack AI

## Summary
A type-safe, provider-agnostic TypeScript SDK (MIT) from the TanStack team (makers of
TanStack Query/Router/Table) for streaming chat, tool-calling agents, structured
outputs, multimodal generation, and realtime voice. Built from composable "activities"
and provider adapters, with native React/Vue/Svelte/Solid/Preact bindings and a
sandboxed "Code Mode" where an LLM writes and executes TypeScript to orchestrate
multi-step tool loops. Won the 2026 JS Open Source Award for AI Project of the Year.

## Why I Should Care
Vercel AI SDK is the default choice for AI features in a React/Next.js stack; TanStack
AI is a credible, typed, framework-agnostic alternative from a team with a long track
record of maintaining exactly this kind of shared infrastructure — and it ships its own
head-to-head comparison against Vercel AI SDK.

## Problems It Can Remove
- Hand-parsed SSE streaming and ad hoc tool-call plumbing between client and server
- Re-implementing structured-output validation per schema library (Zod/ArkType/Valibot)
- Rewriting the chat/tool layer every time a model provider changes

## Practical Uses
- Streaming chat UI in a React/Next.js app with typed tool calls and reasoning parts
- Shared client+server tool definitions via one `toolDefinition()` contract
- Structured extraction backed by Zod/ArkType/Valibot/plain JSON Schema
- Realtime voice sessions with provider-adapter token minting
- "Code Mode" agents that write and run TypeScript in an isolated sandbox to orchestrate
  loops, branches, and parallel tool calls
- Swapping model providers without rewriting the chat/tool layer

## Product Opportunities
Could become the AI layer for any SaaS product's chat/agent feature without hard
vendor lock-in to a single model provider.

## Agent / Automation Opportunities
Ships its own Claude Code/Cursor plugin and skill installer
(`npx skills add TanStack/ai`) so a coding agent learns the library's own idioms before
generating code against it. "Code Mode" is itself an agent-orchestration primitive —
an LLM-authored TypeScript tool loop, not just a tool-calling wrapper.

## Integration
`npm install` the core package plus the provider/framework packages actually used.
Pure library, no infrastructure to run. Low integration effort, but effort scales with
how many activities (chat, image, audio, video, realtime) are adopted.

## Architecture Notes
Composable "activities" plus provider adapters is the same design lineage as TanStack's
other libraries — small composable primitives rather than one monolithic SDK. Worth
studying even independent of adoption, as a model for building a typed, multi-provider
abstraction layer.

## Maturity
Emerging. Created 2025-10-08, 3,147 stars, 341 forks. Pre-1.0 (v0.63.0 on npm) but
actively released and backed by a healthy multi-contributor team, not a solo project.

## License
MIT. No commercial restrictions. Verified via npm registry metadata.

## Alternatives
- **Vercel AI SDK** — the incumbent; TanStack AI ships its own comparison doc.
- **LangChain.js** — heavier, less type-safe.
- **Raw provider SDKs** (OpenAI/Anthropic directly) — no shared abstraction across
  providers or frameworks.

## Risks / Limitations
- Pre-1.0 — API surface can still break between releases.
- Award momentum is real but production track record outside TanStack's own usage is
  still short.
- Multi-package structure means real integration effort grows with adopted scope.

## Recommendation
PROTOTYPE — worth testing against Vercel AI SDK on a real chat/agent feature before
committing either way.

## Change History
### 2026-09-29
Initial discovery and review. Catalogued as PROTOTYPE, 8.2/10.
