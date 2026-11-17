# Tego

## Summary
Tego runs any AI agent that is an ordinary Linux process — an MCP server, a
Claude Code or Codex session, a Python worker, a 24/7 assistant — under a
written YAML contract naming the files, programs and network hosts it may
touch. Enforcement happens outside the agent: the Linux kernel and Tego's
supervisor refuse what the contract rules out, can pause a file open or
connection until a human answers, and keep a hash-chained record of every
run. Tego never calls a model itself and doesn't need to know what the
agent is.

## Why I Should Care
An agent runs as you, next to your keys and your network; a prompt
injection or a plain mistake can reach both. Policy that lives inside the
agent (a system prompt rule, an MCP server's own allow-list) only holds
while the agent chooses to follow it. Tego's bet is that the enforcement
point has to sit outside the agent's control — in the kernel — so a
convinced or compromised agent has no way to talk its way past it. That's
directly relevant to any agent given real credentials, and squarely
relevant to the MSSP/PTaaS secondary context (auditable, kernel-enforced
constraints on what an automated process touched).

## Problems It Can Remove
- Hand-rolling a seccomp/landlock-style sandbox and an audit trail around an
  agent process given real credentials.
- Trusting an agent's own tool-permission logic as the only line of defense
  against a prompt-injection-driven file read, write, or outbound call.
- Manually reviewing what an unattended/24-7 agent actually touched after
  the fact, absent a tamper-evident log.

## Practical Uses
- Wrap a coding agent (Claude Code, Codex, a custom Claude Agent SDK
  process) in a contract limiting it to the repo directory and a named set
  of hosts before giving it write access to a real project.
- Run `tego watch` as a black-box recorder around any agent command to get
  a hash-chained run log without writing a contract first.
- Gate an MCP server's individual tools (allow/hold/deny per tool, not just
  per server) through the MCP relay.
- Set run limits (tokens, cost, time, stall, repeats) so a runaway or looping
  agent session hits a hard stop instead of an open-ended bill.

## Product Opportunities
None identified directly — this is infrastructure for running agents
safely, not a product-building block in itself. The contract + hash-chained
receipt model is worth studying for any internal agent-governance tooling
built for the MSSP/PTaaS context.

## Agent / Automation Opportunities
Purpose-built for this: wraps coding-agent CLIs and MCP servers directly,
ships ready-made contracts for Claude Code, OpenAI Codex, the Claude Agent
SDK and the Anthropic Python SDK, and exposes an MCP relay that applies
per-tool rules live.

## Integration
Single Go binary install on Linux (amd64/arm64). Tier 1 (supervisor-level
enforcement) needs no root; tiers 2–3 (kernel-level enforcement, live
loosening of a running contract) need root. **Integration effort: Low** for
a first trial — write a YAML contract from a preset and run one command;
**Medium** if adopting kernel-level tiers in a shared/production setting.

## Architecture Notes
The three-tier design (supervisor-enforced → kernel-enforced, with
increasing guarantees and increasing privilege requirements) is a sensible
way to let a user trial the tool at low cost before committing to the
stronger, root-requiring enforcement. The hash-chained receipt
(`tego events --verify`) plus a `drift` command comparing what was allowed
against what was actually used gives a concrete audit answer, not just a
log file. No telemetry, no update check, no crash reporting — Tego's only
outbound traffic is DNS and whatever the contract explicitly permits for the
agent.

## Maturity
Experimental — created 2026-11-02 (2 weeks old at review), 25 stars, 2
forks, 1 subscriber, latest tag v0.1.3 (2026-11-13). The project's own
README and a dedicated `docs/GAPS.md` explicitly list known limits before
asking anyone to rely on it — an unusually candid self-assessment worth
trusting over the raw version number.

## License
Apache-2.0.

## Alternatives
Coral (catalogued separately, this run) enforces policy at the agent-to-
provider call boundary rather than the OS process boundary — the two are
complementary, not substitutes. Destructive Command Guard (already
catalogued) blocks specific dangerous shell commands rather than enforcing
a full file/network contract. Generic container/VM sandboxes (Docker,
Firecracker-based agent-microVM tools) isolate the whole process rather than
expressing a fine-grained, auditable per-run contract.

## Risks / Limitations
- Explicitly early: v0.1.3, 2 weeks old, single-digit subscriber count, and
  the maintainers themselves flag known gaps rather than claiming
  completeness.
- Linux-only; no coverage for agents run on macOS/Windows developer
  machines without a Linux host or VM.
- `exec.ask` (holding a program launch for approval) is explicitly called
  out as reserved/refusing rather than implemented — read the docs for which
  controls are actually enforced today versus planned.
- Kernel-level tiers require root, which is a real deployment decision on a
  shared machine.

## Recommendation
PROTOTYPE — worth trying tier 1 (no root required) around one real agent
session with file/network access to validate the contract model, with the
expectation that known gaps mean it is not yet a complete safety net.

## Change History
### 2026-11-17
Initial discovery and review. Slot 7 (experimental projects and hidden gems)
run.
