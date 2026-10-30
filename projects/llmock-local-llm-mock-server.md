# llmock (llm-mock)

## Summary
A local mock LLM API server that speaks the OpenAI, Claude, and Gemini wire formats, so code that calls any of those APIs can be pointed at `localhost` instead during development and testing: fixture replies, Claude-style tool calls, streaming responses, error simulation, and a dashboard for inspecting what was sent.

## Why I Should Care
Testing AI-agent integrations against the real API means burning tokens, hitting rate limits, and getting non-deterministic responses in CI — exactly the opposite of what a test suite needs. This gives deterministic, offline, zero-cost responses for the same wire protocol, including tool-call payloads, which is the part most hand-rolled mocks get wrong.

## Problems It Can Remove
- Paying for and waiting on real model calls just to test integration code paths (error handling, streaming, tool-call parsing).
- Flaky CI runs caused by real API non-determinism or rate limits.
- Hand-writing mock HTTP responses per provider instead of reusing one server that already speaks all three wire formats.

## Practical Uses
- Point an agent-integration test suite's base URL at llmock instead of OpenAI/Anthropic/Gemini for deterministic fixture-driven tests.
- Simulate provider error responses (rate limit, 500, malformed stream) to test an application's retry/fallback logic without waiting for a real failure.
- Develop a Claude tool-calling feature offline using canned tool-call fixture responses before wiring up the real API key.
- Use the dashboard to inspect exactly what payload a client library actually sent, useful when debugging an SDK integration issue.

## Product Opportunities
Not a product itself, but removes a real cost/flakiness tax from building and testing any product that integrates an LLM API — directly relevant given the AI-agent-leverage focus of this catalog.

## Agent / Automation Opportunities
Usable as a CI fixture for agent-framework test suites (LangChain, Vercel AI SDK, custom agent loops) that call OpenAI/Claude/Gemini-shaped endpoints — swap the base URL, no code changes needed in most SDKs.

## Integration
`npx llmock` or local install; no external dependencies beyond Node. Low effort — a base-URL swap in whatever SDK is already in use.

## Architecture Notes
Multi-provider wire-format compatibility in one server (rather than one mock per provider) is the practical design choice that makes it worth adopting over writing a one-off mock.

## Maturity
Mature for its scope: created May 2024 (2+ years old), actively maintained (latest release the day before this review), 22 stars, 2 forks.

## License
MIT, verified against the repo's package.json. No commercial-use restrictions.

## Alternatives
Hand-rolled `nock`/`msw` mocks per provider (more setup, no built-in streaming/tool-call fixture support); hitting the real API in a sandboxed test account (cost and flakiness).

## Risks / Limitations
Small single-repo project with modest traction (22 stars) — worth a quick smoke test against the exact SDK version in use before relying on it for CI, since wire-format compatibility can drift as providers update their APIs.

## Recommendation
PROTOTYPE — drop it into one existing AI-agent integration test suite as a base-URL swap; low cost to try, clear payoff if it holds up against the SDK versions in use.

## Change History
### 2026-10-30
Initial discovery and review — today's Small but High-Leverage Utility.
