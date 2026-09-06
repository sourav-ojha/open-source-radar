# Capptivo

## Summary
A free, open-source desktop screen recorder (Tauri + Rust + React) with follow-cursor
zoom, click-based auto-zoom, editor presets and on-device captions — built by its author
for personal use and open-sourced as a free alternative to Screen Studio ($29/mo) and
Cursorful.

## Why I Should Care
It's a direct, no-subscription replacement for a $29/month tool used for exactly the kind
of polished screen-recording demos a fullstack dev or founder records regularly — product
walkthroughs, PR demos, or client-facing PTaaS/MSSP material.

## Problems It Can Remove
Removes a recurring SaaS subscription for demo/screen-recording software, and removes the
need to send recordings to a cloud service for captioning (captions are on-device).

## Practical Uses
- Record polished product-demo videos for the MSSP SaaS or PTaaS partnership without a
  Screen Studio subscription
- Quick PR/feature walkthroughs for teammates or clients
- On-device captioning for demo videos without sending recordings to a cloud service

## Product Opportunities
None identified — this is a personal productivity utility, not a product-shaped
component.

## Agent / Automation Opportunities
None — desktop GUI tool only, no CLI, API, or scriptable interface documented.

## Integration
Download an installer per OS from GitHub Releases (.dmg/.app.tar.gz for macOS,
.msi/.exe for Windows, .deb/.AppImage/.rpm for Linux). Low effort, but macOS builds are
currently unsigned, requiring a manual Gatekeeper bypass (right-click → Open, or allow
under Privacy & Security) plus a Screen Recording permission grant.

## Architecture Notes
Tauri shell (Rust) with a React frontend — a lightweight-binary alternative to
Electron-based recorders, likely explaining its small install footprint relative to
Electron competitors.

## Maturity
Emerging. Public since ~2026-07-28 (~1 month of history at review), 918 stars, current
release v1.0.3 (2026-08-05), single/small maintainer team.

## License
MIT. No restrictions on commercial or personal use.

## Alternatives
Screen Studio (paid, $29/mo, closed-source); Cursorful (paid); OBS Studio (free but far
more manual, no auto-zoom/follow-cursor polish).

## Risks / Limitations
- macOS builds are unsigned — requires a manual Gatekeeper bypass on first launch
- Small team, ~1 month of public history — verify stability before relying on it for an
  external-facing demo

## Recommendation
PROTOTYPE — reasonable to try immediately for internal demos; hold off using it for a
client-facing recording until it's had more real-world mileage.

## Change History
### 2026-09-06
Discovered during slot 5 run; flagged as this run's Small but High-Leverage Utility.
