# Coral

## Summary
Coral is a self-hosted gateway that sits between AI agents and the systems
they work on. Instead of an agent calling fixed MCP tools, it writes short
TypeScript programs against a catalog of providers (GitHub, Slack, Linear,
any OpenAPI doc or MCP server); Coral injects credentials and checks every
underlying call against a per-tool Allow/Ask/Block policy before it goes out.
When a generated program works, it can be saved as a reusable custom tool
that the agent (or a teammate) calls directly next time, skipping the
code-writing step and the token cost that comes with it.

## Why I Should Care
Most MCP gateways only decide which tools an agent can see — once it can
call `update_record`, it can call it however the tool allows. Coral's unit
of control is a custom tool, which is code, so a rule like "only close
tickets with 30+ days of inactivity" can be enforced at the tool level, not
just the provider level. That is a materially finer permission model for
exactly the use case in play for Sourav's MSSP/PTaaS secondary context:
letting an agent touch customer-facing systems with constrained, auditable
blast radius.

## Problems It Can Remove
- Hand-building an MCP tool-permission layer (Allow/Ask/Block per action) in
  front of internal or customer systems an agent needs to touch.
- Agents re-deriving the same multi-step API choreography from scratch every
  run, burning tokens and time re-reading tool schemas.
- The all-or-nothing tool-visibility model of most MCP gateways when a
  narrower, code-level rule is what's actually needed.

## Practical Uses
- Gate an agent's access to a GitHub/Slack/Linear workflow behind
  Allow/Ask/Block policy instead of trusting the agent's own judgment.
- Let an agent compose a one-off integration (e.g. "post unreviewed PRs to
  Slack") as code, then promote it to a shared, named tool once it's proven
  out, so the whole team reuses it.
- Bring your own OpenAPI spec or MCP server as a provider for an internal
  system, and get the same policy/audit layer in front of it for free.
- Use the desktop app solo with zero infra, or run the shared server (Docker
  Compose/Kubernetes + OIDC + Postgres) for a team.

## Product Opportunities
The "save a working agent interaction as a reusable, policy-gated tool"
pattern is itself a reusable building block — worth studying for any
internal agent-ops tooling built for the MSSP/PTaaS side, where constrained,
auditable third-party-system access is the actual product requirement.

## Agent / Automation Opportunities
This is an MCP server by construction — add a workspace's MCP URL to Claude
Code, Codex, or any MCP-compatible client and every provider behind Coral
becomes reachable under its policy layer. The code-generation step also
works as a general pattern for reducing repeated agent token spend on
multi-step API tasks.

## Integration
Desktop app (macOS/Windows/Linux) for solo use is a straight download. Team
use needs a shared server: Docker Compose or Kubernetes, an OIDC identity
provider, a reverse proxy for TLS, and SQLite or Postgres as the backing
store. **Integration effort: Medium** — solo trial is trivial, team rollout
requires real infra decisions (OIDC, Postgres, TLS).

## Architecture Notes
The core idea — giving an agent a code execution environment against a tool
catalog, rather than a flat list of callable tools — sidesteps the context
bloat of exposing dozens of raw tool schemas, and the saved-custom-tool step
turns one-off agent code into a shared, policy-checked asset. Every call
inside a custom tool is still checked against policy individually, so saving
a tool doesn't bypass the gate it was written under.

## Maturity
Emerging but well-backed — created 2026-04-03, 5,003 stars, 224 forks, 14
subscribers, 143 open issues, latest tag v1.8.0 (2026-11-13), pushed same
day as this review. README claims "thousands of developers at more than 450
companies"; unverified independently beyond the GitHub activity itself. The
elevated stars:subscribers ratio (~357:1) is noted but not disqualifying
given the real fork count and continuous release cadence.

## License
Apache-2.0, confirmed by reading the LICENSE file directly. No split
licensing observed in the top-level LICENSE; the desktop app and hosted
Coral Cloud offering sit alongside the open self-hosted path but don't gate
the open-source gateway itself.

## Alternatives
ThinkWatch (AI API + MCP governance gateway, BSL-1.1), Midplane (Postgres
statement-level agent gateway), Octelium (ZTNA platform with an added MCP
gateway), DatI (database-scoped MCP gateway) — all already catalogued.
Coral is differentiated by its code-as-custom-tool mechanism and
general-purpose provider catalog, rather than being scoped to one system
(a database, an API gateway, a network layer).

## Risks / Limitations
- 450+-companies and token/latency-savings claims are self-reported in the
  README, not independently benchmarked here.
- Team deployment is a real infra project (OIDC + Postgres + reverse proxy),
  not a one-command install.
- Elevated star:subscriber ratio versus fork/issue activity is a minor but
  present engagement-pattern flag, consistent with venture-backed marketing
  rather than organic-only growth.

## Recommendation
PROTOTYPE — worth standing up the desktop app solo against one real
provider (GitHub or Slack) to see whether the custom-tool-save step actually
reduces repeated agent work, before considering the team server rollout.

## Change History
### 2026-11-17
Initial discovery and review. Slot 7 (experimental projects and hidden gems)
run.

### 2026-11-24
Meaningful update: v1.8.0 -> v1.10.0. Coral now reports the Edition and each
Enterprise Feature's state, and trusts a production license-signing key —
the clearest sign yet of an open-core split forming around a paid Enterprise
edition. The self-hosted gateway evaluated here remains Apache-2.0 with no
capability regression observed, but this is worth re-checking over time as
more features may move behind the paid tier. Recommendation unchanged
(PROTOTYPE).
