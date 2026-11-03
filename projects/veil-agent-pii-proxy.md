# veil

## Summary
A local privacy proxy (Python, PyPI package `veil-proxy`) that sits between a coding agent CLI (Claude Code, Codex, Aider, or any Anthropic/OpenAI-compatible client) and the model provider. It deterministically pseudonymizes names, emails, phone numbers, addresses, IDs and secrets before a request leaves the machine, then swaps the real values back into the agent's local tool output — the model reasons about real bugs without ever learning who they happened to.

## Why I Should Care
This is exactly the risk created by letting a coding agent run shell commands or DB queries against anything resembling production data. veil is the first tool found in this niche doing deterministic, reversible pseudonymization rather than one-way redaction that would break the agent's ability to act on the data at all — directly relevant both generally and specifically to the MSSP/PTaaS client-data context.

## Problems It Can Remove
Removes the manual data-scrubbing step before pointing an agent at anything production-shaped, and removes the need to trust a model provider's data-handling terms when the data at hand is a client's.

## Practical Uses
- Run Claude Code/Codex/Aider against a real production DB dump or real customer logs without the model seeing real PII.
- Team-wide `managed-settings` policy forcing Claude Code through veil so redaction can't be bypassed.
- MSSP context: investigate a client incident locally with an AI agent without sending client PII to a third-party model.
- Verify the redaction is working by diffing a captured session's "what was on disk" vs "what the model got" log lines.

## Product Opportunities
None identified — this is a safety/compliance tool, not a product-building block.

## Agent / Automation Opportunities
Sits between any OpenAI/Anthropic-compatible agent CLI and the model provider via wire-protocol adapters (Anthropic Messages, Bedrock InvokeModel, Vertex rawPredict, Microsoft Foundry, OpenAI Chat Completions/Responses). Adoption requires no agent-side code changes — just an env var or base-URL swap.

## Integration
`pip install veil-proxy` or Homebrew; runs as a local proxy on 127.0.0.1:8787. Low integration effort — point the existing agent CLI's base URL at it.

## Architecture Notes
Deterministic pseudonym vault plus a real-time Aho-Corasick-based scrub/unscrub pipeline, with per-provider wire-protocol adapters so the same proxy serves every major agent CLI. The README's Limitations section is unusually candid about where the approach breaks (short-value matching, fragment searches, local transcripts holding real data) — a good maturity signal in itself.

## Maturity
Experimental, explicitly alpha software per the maintainers. Created 2026-10-21 (13 days old at review). PyPI package `veil-proxy` at version 0.3.2; no GitHub Release tags exist. Single maintainer (lemonadesaltbagel).

## License
Apache-2.0, no restrictions.

## Alternatives
Microsoft Presidio (general PII redaction library, no agent-session pseudonym-consistency design, no per-provider wire adapters); manual data scrubbing (status quo); trusting the provider's data-handling terms (status quo).

## Risks / Limitations
- Alpha software; the local machine itself is explicitly "trusted" in the threat model — this protects what leaves the machine, not what happens on it.
- Single maintainer, no independent download-count history beyond PyPI's own numbers.
- An agent with shell access could in principle discover an unanticipated bypass transform, per the maintainers' own disclosure.

## Recommendation
PROTOTYPE — worth trialing specifically for any session where an agent needs to touch data resembling real customer/client records, given the direct MSSP relevance; not yet mature enough to treat as a guaranteed compliance control.

## Change History
### 2026-11-03
First seen and reviewed. Discovered via GitHub search for recently created AI-agent-safety tools; verified license and engagement metrics via GitHub API, version via PyPI directly (no GitHub Release exists).
