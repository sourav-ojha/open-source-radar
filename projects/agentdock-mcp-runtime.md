# AgentDock (uvwt)

## Summary
AgentDock is a standalone MCP-based tool runtime — not a chat client and not a model — that gives AI agents (ChatGPT, Claude, Codex, or anything else that speaks MCP) controlled file, shell, Git, browser-automation, and task-execution access to real machines: local computers, LAN hosts, remote servers, and containers. Multiple AgentDock instances can be connected simultaneously so one conversation can coordinate work across several machines at once.

## Why I Should Care
This targets exactly the AWS/EC2/dev-box context in the operator profile: instead of switching between SSH sessions to inspect logs, deploy, or run commands on different boxes, an agent (or Sourav, through an agent) operates all of them from one conversation, with the runtime itself handling permissioning and structured, traceable results rather than raw shell output.

## Problems It Can Remove
- Manually switching SSH sessions between local dev machine, staging server, and production-adjacent infra to let an agent (or himself) inspect state or make a change.
- Burning a coding-agent vendor's own execution quota for tasks that are really "run a command on a server," not "reason about code."
- One-off scripts to expose a VPS's Docker/reverse-proxy state to an LLM session.

## Practical Uses
- Ask ChatGPT/Claude to inspect logs, processes, and ports on a remote AWS box without opening a terminal.
- Run Git operations and code edits directly in the environment where the app actually runs (staging server, container) instead of only locally.
- Coordinate a deploy that touches multiple hosts (app server + reverse proxy + DB host) from a single agent conversation.
- Extend via Skills/dynamic MCP servers for project-specific operational tasks.

## Product Opportunities
The "explicit permission boundaries + structured, traceable, verifiable results" framing is the right shape for any internal tool that hands an LLM real infrastructure access — worth studying even independent of adopting this specific project, given the MSSP/security-admin context.

## Agent / Automation Opportunities
This is entirely agent-tooling infrastructure: an MCP server designed to be the execution layer under any MCP-capable client, with multi-instance coordination for cross-device workflows.

## Integration
- **Installed locally**: official packaged builds for macOS/Linux/Windows; also Docker Hub (`agentdockio/agentdock`) and GHCR images.
- **Self-hostable**: yes, run per machine you want an agent to reach.
- **Accessed via MCP**: primary interface, multiple instances addressable from one client.
- **Integration effort: Medium-High** — the tool itself installs easily, but safe deployment requires deliberately scoping what "explicit permission boundaries" actually means per host before pointing a chat interface at production infrastructure.

## Architecture Notes
One AgentDock instance per machine (local Mac, LAN host, cloud VPS in its own architecture diagram), each exposing the same tool surface (files, shell, Git, Skills, MCP, browser automation, task execution) over MCP; a client connects to several instances to run cross-machine workflows in one conversation. Deliberately scoped to be execution-only — no chat UI, no model inference — which keeps it composable with whatever client is already in use.

## Maturity
Emerging-to-established: active daily commits, Docker Hub pull badge and a Trendshift trending badge suggesting real adoption, v0.8.3 tagged release, 1,072 stars / 133 forks, documentation site and a Chinese-language community channel alongside the English README — an internationally used project, not a weekend build.

## License
Apache-2.0. No commercial-use restrictions.

## Alternatives
Direct SSH + a coding agent's own shell tool, `mngr`/Paseo (cataloged today, focused specifically on coding-agent lifecycle rather than general infra operation), bespoke internal MCP servers wrapping SSH. AgentDock's differentiation is being infra-operation-focused (files/shell/Git/Docker/reverse-proxy/deploy) rather than coding-agent-lifecycle-focused.

## Risks / Limitations
- **Security is the central risk, not an afterthought**: this grants an LLM-driven client real command/file/Git execution on real machines. A prompt-injection or over-broad-scope misconfiguration here has a materially larger blast radius than a typical dev-tool — treat the "explicit permission boundaries" claim as something to verify hands-on (scope tightly, test on a disposable VM) before pointing it at anything resembling production or client infrastructure, especially given the MSSP/PTaaS context where a security incident in his own tooling would be reputationally costly.
- Documentation and some community links are Chinese-first in places, which may slow troubleshooting for an English-only workflow.
- Younger project (1,072 stars) relative to the operational trust it's asking for.

## Recommendation
PROTOTYPE, with caution — trial it against a disposable sandbox/VM to evaluate the actual permission-boundary model hands-on before considering it for any real AWS infrastructure.

## Change History
### 2026-09-23
Initial discovery and review. Rotation slot 1 (AI agents, MCP, coding productivity).
