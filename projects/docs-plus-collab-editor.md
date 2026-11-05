# docs.plus

## Summary
docs.plus is a self-hostable, open-source real-time collaborative document editor descended from Etherpad, built on Tiptap/ProseMirror/Yjs. Its distinguishing feature is a chatroom attached to every heading, with per-section unread counts, filterable/shareable document views, and an MCP integration letting Claude, ChatGPT and Cursor edit documents directly. MIT, 6 years old, actively maintained.

## Why I Should Care
A 6-year-old, MIT-licensed, actively maintained collaborative editor with a genuinely distinct UX (comment threads scoped to a document section rather than the whole page) removes a multi-week realtime-editor build if a product idea needs this kind of feature — and it's a plausible embeddable building block or standalone micro-SaaS starting point.

## Problems It Can Remove
- Building CRDT-based realtime sync (Yjs + Hocuspocus), presence, and conflict-free editing from scratch.
- A Notion/Confluence subscription for straightforward team docs with discussion.
- Building document comment-threading from scratch when the obvious choice (whole-page comments) doesn't fit.

## Practical Uses
- Embeddable collaborative-editing feature for a product, e.g. shared notes/specs inside the MSSP admin portal.
- Internal team wiki/spec tool with a chatroom per section.
- Self-hosted Google Docs alternative for teams that need documents off third-party servers.
- A micro-SaaS starting point around the chatroom-per-heading UX.

## Product Opportunities
The chatroom-per-heading UX is distinct enough from generic collaborative editors (Liveblocks-style whole-doc presence, or plain Google Docs comments) to be repackaged as a narrow product — e.g. collaborative spec/contract review — rather than staying an internal tool.

## Agent / Automation Opportunities
Ships an MCP server so Claude/ChatGPT/Cursor can read and write directly inside a shared document — a genuine agent-capability hook layered on top of a product feature, not a separate tool.

## Integration
Docker Compose stack of 8 services (Traefik, Redis, REST API, Hocuspocus collaboration socket + worker, webapp, admin dashboard) plus three externally-provided dependencies: a Postgres server, a Supabase project (cloud or self-hosted, needs `pg_cron` and `pgmq` extensions), and S3-compatible object storage (local disk is explicitly unsupported). None of this is exotic for someone who already runs Postgres/S3/Redis, but it is not a single-container drop-in. Integration effort: **Medium**.

## Architecture Notes
TypeScript throughout (webapp, REST API, workers), Yjs CRDTs synced via a Hocuspocus server, with a separate worker process that owns document persistence (saves sit in Redis and expire after an hour without it running). Traefik terminates TLS and load-balances across replicas of the stateless services.

## Maturity
Mature core, modest community. Created 2020-07-16, 98 stars, 6 subscribers, current release v2.2.0 (2026-10-22), pushed same day as review. Long history but small visible community relative to its age.

## License
MIT, confirmed via GitHub API and README badge. No restrictions on commercial use, embedding, or redistribution.

## Alternatives
- **Liveblocks, Yjs (raw)** — lower-level realtime-collab primitives that would need to be assembled into a product; docs.plus is the already-assembled product.
- **Outline, HackMD** — other self-hosted collaborative-doc tools; docs.plus's per-heading chatroom and MCP integration are the differentiator against either.

## Risks / Limitations
- Requires Supabase specifically for auth/storage policies (self-hosted Supabase supported, but it's another moving part) rather than a generic auth provider.
- Modest community size (98 stars, 6 subscribers) relative to 6 years of age — verify ongoing maintenance beyond the maintainers' own hosted instance before depending on it.
- S3-compatible object storage is mandatory even for small deployments.

## Recommendation
**PROTOTYPE** — worth a local `make dev-local` trial to evaluate the editor UX and MCP integration before committing to the full self-hosting stack.

## Change History
### 2026-11-05
First catalogued. Discovered via GitHub search while reviewing slot 2 (product infrastructure / APIs / backend components), under the "product building blocks / collaboration" sub-area.
