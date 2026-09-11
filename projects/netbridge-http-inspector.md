# netbridge

## Summary
netbridge is a zero-config CLI that gives you a network tab for server-side HTTP
traffic. It launches your dev command with a `NODE_OPTIONS` preload that wraps
`fetch` and `http`/`https` in every Node process it spawns — including the worker
processes Next.js forks — and streams everything to a local live UI: method, url,
status, timing, full headers, and full request/response bodies, decompressed and
JSON-pretty-printed.

## Why I Should Care
Server-side outbound calls (a Next.js Server Component or Route Handler calling an
API, an Express backend calling a third-party service) are completely invisible to
browser DevTools. The usual workarounds are all broken in one way or another:
`--experimental-network-inspection` is banned inside `NODE_OPTIONS` so it never
reaches Next.js's spawned worker; OpenTelemetry captures spans but never bodies;
mitmproxy/Charles need certificate juggling and TLS downgrade; `console.log`
disappears on the next request. netbridge fixes exactly this gap with one `npx`
command and no code changes.

## Problems It Can Remove
- Adding and removing `console.log` around every outbound fetch call while
  debugging a MERN/Next.js backend.
- Certificate/proxy setup for MITM tools just to see one API response body.
- The blind spot around Next.js server-component/route-handler outbound calls
  specifically, which no existing browser-based tool can see.

## Practical Uses
- `npx netbridge -- next dev` to get a live request list while debugging a broken
  API integration in a Server Component.
- `netbridge -- node server.js` or `netbridge -- npm run start:dev` in front of an
  Express or NestJS backend.
- Filter out noisy OpenTelemetry exporter traffic (`--exclude localhost:4318`)
  while chasing a real bug.
- Export a session as HAR to load into Chrome DevTools or Insomnia, or copy a
  single request as curl/fetch/Python for a bug report or an AI prompt.

## Product Opportunities
None identified — this is a local dev-only debugging tool, not something to embed
in a shipped product.

## Agent / Automation Opportunities
No MCP surface currently, but the "copy request as an AI prompt" export feature is
a direct bridge to pasting real request/response evidence into an agent session
when debugging an integration issue together.

## Integration
`npx netbridge -- <your dev command>` — no install, no config file, no proxy, no
certificates. Integration effort: **Low**. Runs entirely on `127.0.0.1`.

## Architecture Notes
The interesting engineering is propagating instrumentation into every child Node
process a dev command spawns, including ones started indirectly (Next.js forking
its own server as a worker) — done via `NODE_OPTIONS="--require netbridge/preload"`,
which environment-variable inheritance carries into child processes even where a
CLI flag wouldn't. `fetch`-based clients (ky) go through a `globalThis.fetch`
wrapper; `axios`/`got`/`superagent` go through an `http`/`https.request` wrapper —
both funnel into one collector via fire-and-forget streaming so instrumentation
doesn't block the real request.

## Maturity
Very early: created 2026-06-12, 3 GitHub stars, single maintainer, no tagged
GitHub releases (npm registry, currently at v0.4.0, is the actual source of
truth), green CI badge. Small enough that "maturity" mostly means "does the
preload trick hold up across your specific Node version and framework" rather
than any track record.

## License
MIT — no restrictions identified, and it's dev-only so there's no production
licensing surface to worry about anyway.

## Alternatives
Browser DevTools (blind to server-originated requests), Node's own
`--experimental-network-inspection` flag (doesn't reach Next.js worker processes),
OpenTelemetry (metadata only, no bodies), and MITM proxies (mitmproxy, Charles,
Proxyman — external install, cert juggling, and undici ignores `HTTP_PROXY`
regardless).

## Risks / Limitations
- 3 stars, single maintainer — real bus-factor risk.
- No tagged releases yet to point to for stability guarantees.
- Local UI binds to `127.0.0.1` by default, which is correct, but worth
  double-checking if ever run on a shared dev box.

## Recommendation
USE NOW — the integration cost is a single `npx` invocation with zero setup, and
it solves a real, recurring blind spot in exactly this stack (Next.js/Express on
Node). Low risk given it's dev-only and never touches production.

## Change History
### 2026-09-11
Initial discovery and review. Slot 3 (developer utilities, debugging, testing) run.
