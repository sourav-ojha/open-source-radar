# Bookshelf

## Summary
A self-hosted library for ebooks you already own: drop EPUBs/PDFs in a folder, sync, and read them in a browser or on a Kobo via OPDS. Runs entirely on object storage — either a Cloudflare Worker over R2, or a plain Node server over a local directory — with no database at all; the same codebase and library format work in both deployment modes.

## Why I Should Care
The interesting part isn't the ebook-reader feature set, it's the architecture: a real multi-user-feeling app (covers, search, reading-position sync, a profile switcher) built with zero database, using object storage as the only source of truth. That's a reusable pattern worth remembering for small internal tools or content-serving side projects where standing up Postgres is overkill.

## Problems It Can Remove
- Setting up Calibre-web or a similar heavier self-hosted ebook server just to read personally-owned EPUBs across devices.
- The instinct to reach for a database by default for small, mostly-static content-serving tools.

## Practical Uses
- Personal ebook shelf accessible from any browser or a Kobo e-reader, deployed for free on Cloudflare's R2 free tier.
- A local-only version for a NAS or home server via the Node/filesystem provider.
- A reference to copy the "storage-only, no-DB" pattern for a different small content tool.

## Product Opportunities
None directly commercial — this is a personal-productivity utility, not a product-building block.

## Agent / Automation Opportunities
None notable; this is a personal-use application, not an agent-facing tool.

## Integration
`npm install && npm run demo` for a local trial; Docker Compose for a self-hosted instance, or `wrangler deploy` for the Cloudflare/R2 route. Effort: **Low**.

## Architecture Notes
Storage-provider abstraction (`fs` vs `r2`) behind one interface is the reusable idea — the same sync/build/serve code runs unmodified against either backend, since ebook metadata is derived from the files themselves rather than stored in a separate database.

## Maturity
Emerging — created 2026-08-17, 360 stars within a month, has CI. The maintainer explicitly documents that **there is no authentication** — anyone who can reach the deployed URL can read/download everything and switch profiles freely.

## License
MIT — no commercial-use restrictions.

## Alternatives
Calibre-web, Kavita, COPS — all of these are heavier (require a database and typically a persistent server process) and include built-in auth, which Bookshelf deliberately omits in favor of "put it behind your own network/auth layer."

## Risks / Limitations
- No authentication built in — must be placed behind Tailscale, a reverse-proxy auth layer, or kept genuinely private before storing anything sensitive.
- Windows unsupported for the sync tooling (relies on `which`).

## Recommendation
WATCH / personal-utility pick — genuinely useful as a personal tool and a nice architecture reference, but out of scope for product-development relevance. Featured as this run's "Small but High-Leverage Utility."

## Change History
### 2026-09-15
Initial discovery and review.
