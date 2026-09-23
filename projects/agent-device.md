# agent-device

## Summary
`agent-device` (by Callstack, a well-known React Native consultancy) gives AI coding agents a live feedback loop on mobile and desktop apps: a CLI, a bundled stdio MCP server, and a typed Node.js API let an agent open an app, read its accessibility-tree snapshot, tap/fill/scroll via stable refs, wait for the UI to settle, and capture screenshots/video as evidence — on iOS, Android, HarmonyOS, tvOS, Android TV, Amazon Vega OS, web, macOS, and Linux (simulators, emulators, and physical devices).

## Why I Should Care
Coding agents that generate UI changes currently have no way to verify them beyond reading source and running unit tests. This closes that loop for mobile and cross-platform UI work specifically — an agent can make a change, drive the running app, and check that the result actually looks/behaves right, with token-efficient accessibility snapshots instead of screenshot-only reasoning.

## Problems It Can Remove
- Manual "build the app, tap through the flow, eyeball it" verification after every agent-generated UI change.
- Writing one-off Appium/Detox/XCUITest scripts just to let an agent check its own work.
- Screenshot-only agent verification, which burns tokens and misses non-visual state.

## Practical Uses
- Let a coding agent verify a React Native or Flutter UI change against a real simulator/device before opening a PR.
- Automated regression evidence capture (screenshots/video) attached to agent-authored PRs.
- Cross-platform manual QA scripts replaced by agent-driven flows across iOS/Android/web/desktop from one CLI.
- Parallel agent workstreams coordinated against a pool of devices without stepping on each other's sessions.

## Product Opportunities
The accessibility-snapshot-plus-ref model (read app state as structured data, act via stable refs, diff after each action) is a reusable pattern worth studying for any internal "let an agent verify its own UI work" tooling built for other platforms.

## Agent / Automation Opportunities
Primary interface is CLI + MCP server (`agent-device mcp` exposes the CLI as MCP tools) or a typed Node.js client (`createAgentDeviceClient()`) for building custom agent orchestration on top. Works out of the box with Claude Code, Codex, Cursor, Windsurf, Cline, Goose, or any MCP-compatible agent.

## Integration
- **Installed locally**: `npm install -g agent-device@latest`, requires Node.js 22.12+ (24+ for web automation).
- **CLI-invoked**: yes, primary interface; `agent-device doctor` validates environment setup.
- **MCP server**: `agent-device mcp` (stdio).
- **Embeddable**: typed Node.js API for custom orchestration.
- **Integration effort: Medium** — the CLI itself installs in minutes, but real use requires simulators/emulators or device access already set up per target platform.

## Architecture Notes
Snapshots come from each platform's native accessibility tree rather than screenshots, which is what makes agent verification token-efficient and reliable — clear labels/roles/test IDs on the app under test directly improve agent run quality. Refs are scoped to the latest command output (a `--settle` diff invalidates prior refs), which avoids a common class of stale-selector bugs in UI automation tools.

## Maturity
Emerging-to-established: created January 2026, actively pushed daily, v0.21.12 tagged release, real CI, MIT license, 4,739 stars / 311 forks. README documents real production usage at Expensify (bug evidence/profiling, per a published Callstack blog post) and a public mention from a Shopify engineer — this is used in the wild, not just a demo.

## License
MIT — no commercial-use restrictions.

## Alternatives
Appium, Detox, Maestro (mobile-testing Maestro, not the agent-orchestration RunMaestro/23blocks tools also seen this run), XCUITest/Espresso directly. None of these are purpose-built for AI-agent consumption (token-efficient snapshots, ref-based action model, MCP-native).

## Risks / Limitations
- Requires real device/simulator infrastructure to be useful — not a zero-setup tool.
- Mobile/cross-platform automation is a narrower need than general coding-agent tooling; only relevant when shipping mobile or cross-platform UI.
- Young in its current open-source form (created 2026-01) despite real backing and usage.

## Recommendation
PROTOTYPE — worth wiring into any React Native or cross-platform UI workflow where agent-authored changes need verification beyond source review; low risk to trial given MIT license and a real npm CLI.

## Change History
### 2026-09-23
Initial discovery and review. Rotation slot 1 (AI agents, MCP, coding productivity).
