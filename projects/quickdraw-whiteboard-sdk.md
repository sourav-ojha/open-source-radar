# Quickdraw

## Summary
An MIT-licensed infinite-canvas whiteboard SDK for React, React Native and vanilla JS.
Drops a complete, polished drawing surface — pressure ink, shapes, arrows, sticky notes,
real-time-sync-ready data model — into any app, positioned explicitly as a fee-free
alternative to tldraw.

## Why I Should Care
Whiteboard/canvas UI is a recurring "buy or build" decision for admin panels and
collaboration features. tldraw is the default answer but isn't free for commercial use
past a threshold; Quickdraw removes that constraint entirely while matching much of the
polish (hand-drawn shape styling, pressure-sensitive ink, palm rejection).

## Problems It Can Remove
Removes weeks of canvas/whiteboard UI engineering (undo/redo, selection, resize/rotate,
palm rejection, export) and removes a potential tldraw license fee for a commercial
product.

## Practical Uses
- Embed a whiteboard/annotation surface in an admin panel or collaboration feature
- Prototype a Miro/Excalidraw-style feature inside a product quickly
- Build a custom toolbar on top of the headless canvas primitives
- Ship architecture or incident diagrams inside a client-facing security portal

## Product Opportunities
A diagramming/annotation feature bolted onto a SaaS product (e.g. incident diagrams for
the MSSP portal) without paying per-seat whiteboard SaaS fees.

## Agent / Automation Opportunities
None identified — this is a UI/frontend SDK, not an agent-facing tool. Its diffable
JSON-safe change model could be useful if an agent needs to programmatically generate or
edit board content, but that's speculative, not documented.

## Integration
`npm install @quickdrawjs/core` (or `/react`, `/react-native`); zero runtime dependencies,
plain ESM, no build step required for the core. Low effort — one function call
(`createQuickdraw()`) to mount a working board.

## Architecture Notes
Core is dependency-free plain ESM. Every change emits a JSON-safe diff over the data
model, intended to be shippable over any transport for real-time sync — sync itself isn't
bundled, but the primitive is there to build on.

## Maturity
Experimental. Created 2026-07-31 (~5 weeks old at review), 3 contributors, npm package at
0.2.0 (ahead of the v0.1.1 GitHub tag), 440 stars.

## License
MIT. Genuinely free for commercial use, unlike tldraw. A "Made with Quickdraw" watermark
badge is on by default but removable for free with one config flag — no license key, no
signup.

## Alternatives
tldraw (feature-rich but not free past a commercial threshold); Excalidraw (MIT, more
mature and larger community, less polished pressure-ink/theming); Fabric.js/Konva
(lower-level canvas libraries, much more build effort).

## Risks / Limitations
- Very new (~5 weeks old), small team — feature set could still shift and it's unproven in
  production apps
- Excalidraw is a more battle-tested MIT alternative if maturity matters more than
  tldraw-style polish

## Recommendation
PROTOTYPE — worth a small spike to embed in an internal tool before depending on it for a
client-facing feature, given how new it is.

## Change History
### 2026-09-06
Discovered during slot 5 run. Confirmed npm publication and version via the npm registry
API (0.2.0, ahead of the 0.1.1 GitHub release tag).
