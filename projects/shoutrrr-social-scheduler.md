# Shoutrrr

## Summary
Self-hosted social media scheduling tool from the team behind Coolify: compose once, publish to X, Bluesky, LinkedIn, Facebook, Instagram, Threads, and Discord from a single calendar/queue. Workspaces with roles and email invites, per-post analytics where the platform API supports it, encrypted token storage with auto-refresh. Single Docker image running web app, queue worker, and scheduler together; defaults to SQLite.

## Why I Should Care
A second, independently-built entrant in the self-hosted social-scheduler niche from a maintainer with a strong track record (Coolify, 63.7k stars) is a useful signal the space is maturing beyond single-maintainer side projects. The simpler single-container/SQLite deploy is a lower-friction starting point than a Docker-Compose-stack alternative if an approval workflow and client portal aren't needed yet.

## Problems It Can Remove
Replaces a Buffer/Typefully/Hootsuite subscription for scheduling posts across seven networks, and avoids an aggregator holding your platform tokens and analytics data.

## Practical Uses
- Schedule and publish posts across X, LinkedIn, Bluesky, Facebook, Instagram, Threads, and Discord from one calendar.
- Agency-style multi-workspace setup keeping separate clients' or brands' accounts isolated.
- Self-hosted alternative where token custody and analytics-data ownership matter more than aggregator convenience.

## Product Opportunities
Could be white-labeled as a client-facing social-scheduling add-on, though the lighter feature set (no approval workflow, no client portal) makes it a weaker fit for agency resale than the already-catalogued BrightBean Studio.

## Agent / Automation Opportunities
None beyond standard webhook/API automation; no MCP server or agent-specific surface documented.

## Integration
Single prebuilt Docker image (`ghcr.io/coollabsio/shoutrrr`) runs the web app, queue worker, and scheduler together. Defaults to SQLite with no external services; can switch to Postgres/Redis to scale out. Low integration effort.

## Architecture Notes
Laravel (PHP 8.5) + React 19 + Inertia + Tailwind stack — outside the core Node/Express toolchain, but irrelevant for a Docker-deployed self-hosted tool rather than something embedded into existing code. OAuth 2.0 / ATProto / webhook-URL connections per platform, with encrypted, auto-refreshing tokens.

## Maturity
Emerging and young: created 2026-06-12 (~4 months old), pushed 2026-10-24, 454 stars, 62 forks, 3 subscribers, 10 contributors. Top contributor andrasbacsai is Coolify's founder, confirming genuine coollabsio backing rather than a name-squatting repo. Latest release (v1.5.7) is a security patch for a laravel/mcp SSRF/credential-exposure advisory — a good sign of active maintenance attention.

## License
Apache-2.0 — unrestricted commercial use including white-label resale.

## Alternatives
Buffer, Typefully, Hootsuite (paid SaaS). BrightBean Studio (already catalogued, 2026-10-04) covers a broader feature set (10+ platforms, approval workflows, client portal, unified inbox) aimed at agency resale; Shoutrrr is narrower but from a more proven team with a simpler deploy.

## Risks / Limitations
- Only 3 GitHub subscribers; maintenance commitment beyond the parent org's current attention is unproven.
- PHP/Laravel backend sits outside the core Node/Express stack.
- No approval workflow or client portal, so it does not replace BrightBean Studio for agency/client-resale use cases.

## Recommendation
PROTOTYPE — worth testing for simple personal/brand scheduling, but BrightBean Studio remains the stronger pick for agency-style client resale.

## Change History
### 2026-10-25
Discovered via GitHub search for self-hosted SaaS alternatives (slot 5). First-time review; compared directly against already-catalogued BrightBean Studio.
