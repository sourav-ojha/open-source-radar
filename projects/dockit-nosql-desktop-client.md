# DocKit

## Summary
An open-source native desktop GUI (Tauri/Rust) for Elasticsearch, OpenSearch, DynamoDB,
MongoDB, and EasySearch in one app, with a Monaco-powered query editor and optional
bring-your-own-key natural-language-to-query assistance (OpenAI, Anthropic, or
DeepSeek). Positioned explicitly by its own README as a replacement for browser consoles
like Kibana and the AWS Console.

## Why I Should Care
It covers the two NoSQL-adjacent stores he actually touches — MongoDB directly, and
Elasticsearch/OpenSearch as AWS-adjacent infra — in one native app, with DynamoDB
coverage as a bonus if that ever enters the stack. One tool instead of switching between
Compass, a browser-based Kibana/OpenSearch Dashboards tab, and the AWS Console cuts real
context-switching during ad hoc database debugging.

## Problems It Can Remove
Removes the need to keep a Kibana/OpenSearch Dashboards instance running just to poke at
an index, open the AWS Console for a quick DynamoDB lookup, or switch to a separate
Mongo-only GUI — all three become one desktop app.

## Practical Uses
- Browse and query MongoDB collections without a separate Compass window.
- Inspect Elasticsearch/OpenSearch indices with natural-language-to-query generation
  instead of hand-writing Query DSL for a one-off lookup.
- Browse DynamoDB tables without opening the AWS Console.
- Use the Monaco editor directly for anything the natural-language assistant doesn't
  cover well.

## Product Opportunities
None specific — this is an internal tooling/debugging aid, not something to embed in a
product.

## Agent / Automation Opportunities
Has a companion MCP server (`geek-fun/data-studio`, listed on the MCP Toplist registry),
giving an agent the same multi-protocol database access the desktop app has, though this
run did not independently verify the MCP server's feature parity.

## Integration
Desktop app install for macOS, Windows, or Linux — no server component to run. Low
integration effort. AI assistance requires supplying your own OpenAI/Anthropic/DeepSeek
API key.

## Architecture Notes
Native Tauri/Rust shell rather than Electron, keeping the app lightweight across all
three desktop platforms. Query generation is LLM-assisted but editable before running —
the README frames it as "describe what you need, get runnable queries" rather than
autonomous execution.

## Maturity
Mature for this niche: created 2023-07-22 (3+ years old), 1,145 stars, 72 subscribers,
122 forks, 6 open issues — a healthy ~16:1 star-to-subscriber ratio and real fork
engagement, actively pushed as of this review.

## License
Apache-2.0. No restrictions on commercial use or embedding.

## Alternatives
- MongoDB Compass — Mongo-only.
- Kibana / OpenSearch Dashboards — require the full stack running; not a lightweight
  desktop client.
- AWS NoSQL Workbench — DynamoDB-only, tied to an AWS account.
- mqlens/mqlens-mongodb — a newer, thinner (61 stars, 2 subscribers) MongoDB-only
  desktop alternative seen the same day; held to Worth Watching rather than catalogued
  given its narrower single-database scope and a notably high open-issue-to-star ratio
  (46 open issues on 61 stars) suggesting rougher edges.
- geek-fun/sqlkit — DocKit's own SQL-side sibling product (308 stars, 4 subscribers);
  held out as thinner and redundant given DocKit already covers the NoSQL side he
  actually needs.

## Risks / Limitations
- Desktop-app-only; no headless/server mode for shared team access.
- AI query assistance requires bringing your own API key — an ongoing cost, not a
  one-time setup.
- Built by a product studio (Geek Fun) that sells other tools; worth watching for any
  future shift toward an open-core model, though there is no sign of that currently —
  the full feature set reviewed here is in the Apache-2.0 repo.

## Recommendation
PROTOTYPE — install it and point it at one existing MongoDB or Elasticsearch/OpenSearch
deployment to see whether it actually replaces the Compass/Kibana tab-switching in
practice.

## Change History
### 2026-11-20
Discovered via a `topic:database-gui` GitHub search during slot 3 (developer utilities,
debugging, testing) discovery, in a notably crowded same-day wave of new lightweight
native DB clients (GoNavi, TablePro, tabletist, dbine, data-peek) — none of which showed
clear differentiation over this or the already-catalogued WhoDB/LibreDB Studio.
Catalogued as PROTOTYPE, 7.8/10.
