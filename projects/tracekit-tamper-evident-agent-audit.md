# Tracekit

## Summary
A tamper-evident audit trail for AI agent tool calls. Every tool call an instrumented
agent makes is gated against policy before it runs, then signed into an append-only hash
chain modeled on Certificate Transparency; the result is one `.tkb` file per run that
anyone can verify offline, independent of whether the agent (or its logs) can be trusted.

## Why I Should Care
Any agent given shell, database, or API access can edit its own log after the fact — the
project's own demo shows an agent deleting 43 of 128 orders, then editing both its own log
and a naive audit record so the deletion reads as a lookup. Tracekit's hash-chained
records fail verification the moment one is altered, which is the property a plain log
file cannot offer. Directly relevant to the MSSP/PTaaS context: a verifiable record of
exactly what an agent did against client infrastructure or data is a real compliance
artifact, not just a debugging aid.

## Problems It Can Remove
- Hand-rolled "log everything the agent does" setups that trust the agent's own process
  to keep an honest record.
- The gap between "the agent says it only read the file" and proof of that claim.
- Building a custom policy-gate (allow/deny/ask) in front of agent tool calls from scratch.

## Practical Uses
- Wrap any Claude Code / Codex / Cursor / Gemini CLI session with `tracekit init --dev`
  and get a verifiable run record with no agent-side code changes.
- `tracekit audit --since 7d` retroactively shows what Tracekit would have denied or held
  in a machine's existing Claude Code session history.
- Gate an agent's unsandboxed retries (Claude Code / Codex / Gemini CLI sandbox escapes)
  behind a human approval, with the approval itself part of the signed record.
- Point it at a LangChain/LangGraph/CrewAI/MCP-based internal tool and get the same
  tamper-evident record without switching frameworks.
- Use the published open `.tkb` file format to build custom reporting/compliance tooling
  on top rather than depending on Tracekit's own viewer.

## Product Opportunities
Could underpin an auditable-agent-actions feature for an MSSP admin portal: "show the
client a signed, independently-verifiable record of every action our AI tooling took on
their systems" is a stronger compliance claim than a database audit log alone.

## Agent / Automation Opportunities
Three-line integration (`pip install tracekit-ai`, `tracekit.instrument()`, run as usual).
Stable support for Claude Code, OpenAI Agents SDK, LangChain/LangGraph, Claude Agent SDK,
and MCP; experimental support for Codex CLI, Cursor, Gemini CLI, CrewAI, Browser Use,
Pydantic AI, Google ADK, Vercel AI SDK, and sandbox runtimes (E2B, microsandbox, NVIDIA
OpenShell). TypeScript SDK exists alongside the Python one.

## Integration
`pip install tracekit-ai` (PyPI package `tracekit-ai`, not `tracekit` — that name is an
unrelated project). Runs on your own machines; the signer should run somewhere the agent
can't reach to get the tamper-evidence guarantee (the README's dev-mode demo deliberately
runs the signer under the agent's own user to show what that weakens). No SaaS dependency
for the core signing/verification loop. Integration effort: Low for the dev/local case,
Medium to get the "signer isolated from the agent, with an independent witness" production
posture the security claims rely on.

## Architecture Notes
Modeled explicitly on Certificate Transparency: signed records in an append-only log,
independent witnesses cosign, open file format with separate Python and TypeScript
verifiers so no one has to trust Tracekit's own code to check a record. Published proof
section claims 8/8 tamper attacks caught across 480 trials with a witness, 788/788 harmful
tool calls stopped across 11 classes (1 false alarm in 849 trials), under 1ms added per
tool call at p99 (0.91ms), and 8/8 insider-attack scenarios caught via a merge-gate CI
check run on every PR. These are the project's own stated benchmarks, not independently
reproduced here.

## Maturity
Emerging. Created 2026-09-26 (39 days old at review), PyPI package at v1.5.3, 407 stars /
59 forks / 3 subscribers, 50 open issues (real engagement signal, not a near-zero-activity
repo). The stars:subscribers ratio (~136:1) is elevated versus a typical organic repo and
is flagged for tracking, but unlike prior star-inflation cases rejected from this catalog,
this repo pairs it with a healthy fork ratio, active issues, a real PyPI package, and a
detailed, falsifiable benchmark methodology rather than bare marketing claims.

## License
MIT. No restrictions on commercial use, self-hosting, or embedding.

## Alternatives
Plain application logging (status quo, editable by the agent); OpenTelemetry tracing
(observability, not tamper-evidence); Decionis/AgentSafe (a competing "gate every agent
action" approach found the same day, rejected here — see Rejected / Duplicate Candidates
in the 2026-11-04 daily digest — because its actual authorization decision runs on a
proprietary hosted control plane and the open-source piece "decides nothing itself").
Tracekit's core signing/verification loop works standalone with no required SaaS backend.

## Risks / Limitations
Pre-1.0 and single-digit subscriber count despite real star/fork activity; worth
re-verifying independent adoption on a future pass. The dev-mode quickstart runs the
signer under the agent's own user, which the README itself says an agent could defeat —
the tamper-evidence guarantee requires deploying the signer where the agent can't reach
it, which is a deliberate operational step, not the default.

## Recommendation
PROTOTYPE — wrap one real agent session (ideally one with filesystem/DB/API access) with
`tracekit init --dev`, deliberately try to make the agent hide an action, and confirm
verification actually fails before relying on it for anything resembling a compliance
claim.

## Change History
### 2026-11-04
Discovered via GitHub search (`mcp observability`, `mcp gateway` keyword sweeps). First
catalog entry.

### 2026-11-11
Meaningful update: v1.5.3 -> v1.6.0. New hooks for Kilo Code and Qwen Code
(Windows/PowerShell), plus experimental Devin CLI/Desktop support, widen agent coverage.
New policy TK-A055 blocks destructive database drop/reset commands, and TK-F009/F015 flag
PowerShell-profile writes as harness-setting changes — real new policy enforcement, not
just bug fixes. Added "model receipts" verifying weights files written during a call. Go
verifier now reaches the same mutation-testing parity as the Python/TypeScript verifiers.
Score raised 8.0 -> 8.2.
