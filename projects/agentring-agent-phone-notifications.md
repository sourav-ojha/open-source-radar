# agentring

## Summary
Lets a coding agent reach you by phone call or message, even on a locked device, and lets
you call your agents back and talk to them by voice. Self-hosted over VoIP on your own
server (no Twilio, no per-minute fees); a hosted version exists for anyone who doesn't want
to run the server.

## Why I Should Care
The common "agent needs a human decision mid-run" problem is usually solved with a Slack
message or a terminal prompt that waits for someone to notice it. agentring instead rings
the phone — including a locked phone, bypassing Doze — with a voice AI that explains why
the agent is calling, takes the answer, and hands the transcript back to the agent so it
can continue. That is a genuinely different notification channel from anything else in
this catalog's "agent reaches a human" niche.

## Problems It Can Remove
- A long-running migration, deploy, or CI job that silently waits (or silently fails) for a
  decision nobody saw in time.
- Building a custom "text me when the agent needs something" integration on top of Twilio
  or a paid notification SaaS.

## Practical Uses
- "Run the migration and call me if anything fails."
- "Ask me before you deploy" as a standing instruction to a coding agent.
- A daily voice check-in with an agent about overnight CI results or inbox triage while
  away from a screen.
- Calling an agent from the app mid-drive to ask a quick status question by voice.

## Product Opportunities
None specific to a commercial product; this is personal/ops infrastructure for working
with agents, not a component to embed in a product.

## Agent / Automation Opportunities
MCP server for any agent to call `send_message`/`get_replies`/`call`; also exposes a REST
API and CLI for non-MCP automation (n8n, scripts). Harness-agnostic by design — works with
Claude Code, Codex, opencode, OpenClaw, and Hermes via documented recipes per harness.

## Integration
Self-hosted: your own server running the VoIP/signaling piece plus an Android app on the
phone that receives the call; Apache-2.0, no Twilio account or per-minute billing. Hosted
alternative at agentring.io for anyone who doesn't want to run a server. Integration
effort: Low-Medium — most of the setup is the Android app and MCP wiring, not infrastructure.

## Architecture Notes
VoIP calls run server-to-phone without a telephony provider in the loop, which is the part
worth studying even without adopting the project: most "agent calls a human" designs reach
for Twilio by default, and this avoids both the vendor dependency and the per-minute cost.

## Maturity
Pre-release. Android only (iOS not mentioned), tested so far on a single phone (Pixel 6,
Android 16) per the README's own disclosure. Created 2026-10-23 (12 days old), v0.2.0, 273
stars / 34 forks / 3 subscribers, 4 open issues. Small but plausible engagement profile for
its age; no red flags in the fork/issue ratio.

## License
Apache-2.0. No restrictions.

## Alternatives
Slack/email/webhook notifications (status quo, easy to miss or ignore); Twilio-based custom
paging (per-minute cost, more setup); push notifications (no voice interaction, no
handing a transcript back to the agent).

## Risks / Limitations
Pre-release and tested on exactly one device so far; Android-only. Treat as an experiment
to run personally before depending on it for anything time-sensitive.

## Recommendation
PROTOTYPE / Small but High-Leverage Utility — worth installing on one personal device to
try the "call me if the deploy fails" pattern on a real long-running job.

## Change History
### 2026-11-04
Discovered via GitHub search (`mcp recent` keyword sweep, slot 1 rotation). First catalog
entry; today's Small but High-Leverage Utility.
