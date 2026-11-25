# phonedriver

## Summary
An MCP server and CLI that gives a coding agent (Claude Code, Codex, Cursor, VS Code, Gemini CLI and others) control of a real or virtual Android phone or iPhone. It reads the screen as a short numbered list of UI elements rather than a screenshot, taps/types/swipes by element reference, reports what changed after each action, and stops to ask for human approval before any action that pays. The README claims 6.9x fewer tokens than mobile-mcp on a nine-job benchmark.

## Why I Should Care
Turns "test the mobile app" or "automate a repetitive phone task" from a manual chore into something a coding agent can do directly inside the same Claude Code/Codex session already used for backend and web work. The "asks before it pays" guard is a concrete, reusable safety pattern for any agent tool that can take a real financial action, and the accessibility-tree-style text rendering (not screenshots) is the reason it is claimed to be dramatically cheaper per job than a vision-based approach.

## Problems It Can Remove
- Manual mobile QA passes: navigating several screens deep to toggle a setting, or walking through a signup flow by hand to find where it breaks.
- The token cost of screenshot-based mobile automation, by reading the accessibility tree as a compact numbered list instead.
- The risk of an agent executing a real purchase/payment unattended.

## Practical Uses
- "Open my app and test signup. Try invalid inputs. Report each bug with steps to reproduce it."
- Mobile regression checks in a PTaaS/MSSP engagement context where a client's mobile app needs a scripted UI walkthrough.
- Drive a multi-step mobile task (e.g. an order or checkout flow) up to the point of payment, then hand off for a human tap.
- CI-adjacent exploratory testing on an Android emulator driven entirely from an agent prompt.

## Product Opportunities
None directly — this is a testing/automation tool, not a building block for a product.

## Agent / Automation Opportunities
Ships as a standard MCP server over stdio (works with any MCP client) and is additionally packaged as installable plugins for Claude Code and Codex that bundle the server with a skill file teaching the agent how to use it. The payment-approval interception pattern is worth reusing in any other agent tool capable of taking a real-world financial action.

## Integration
```sh
npx -y phonedriver init
```
Detects agents already installed (Claude Code, Codex, Cursor, VS Code, Gemini CLI) and wires itself in; also installable via Homebrew (`brew install hridaya423/tap/phonedriver`) or as a standard MCP server entry for any other client. Requires an Android emulator/device with `adb`, or Xcode/iOS Simulator for iPhone. Integration effort: **Low** for the install itself, though device setup (adb, Xcode) is an extra local dependency beyond the tool.

## Architecture Notes
Renders each screen as a short numbered list of elements (the README shows a Settings screen as numbered rows a model reads and references by index) instead of a screenshot — this is the stated basis for the claimed 6.9x token reduction versus mobile-mcp. The payment guard holds the tap and asks the human via the MCP client before completing a "Buy"-type action; the demo shows this working on a real e-commerce checkout.

## Maturity
Emerging. Created 2026-11-08, v0.1.7 (tagged 2026-11-21) at review — about two and a half weeks old, but with a docs site, published benchmarks, CI, and localized READMEs in four languages, suggesting more engineering discipline than a weekend project despite the short age.

## License
AGPL-3.0, confirmed via npm registry metadata (`phonedriver@0.1.7`, license `AGPL-3.0-only`) matching the repo's license badge. Flagged per licensing policy: fine for local/personal use as a CLI or MCP server; embedding it into a hosted, multi-tenant product or offering it as a network service to others would trigger AGPL's source-disclosure requirement, so review before any commercial-product integration.

## Alternatives
mobile-mcp — phonedriver's own benchmark claims 6.9x fewer tokens on the same class of job by reading the accessibility tree as text instead of screenshots. Appium or Maestro with a vision-model wrapper is much heavier to set up and not natively agent/MCP-oriented. Manual QA on a physical device or emulator is the default this replaces for scripted or exploratory agent-driven passes.

## Risks / Limitations
- AGPL-3.0 — see License section above.
- Very young (17 days old at review) with a small subscriber count (1) relative to its 372 stars; worth re-checking engagement and continued maintenance in a future run.
- Requires local device/emulator setup (adb, Xcode) rather than being a zero-dependency install.

## Recommendation
PROTOTYPE — worth trying against a real mobile app's signup or checkout flow to validate both the token-efficiency claim and the payment-approval guard before relying on it for anything unattended.

## Change History
### 2026-11-25
First seen and reviewed. Discovered via GitHub search for mobile/Android/iOS agent-control keywords (slot 1: AI agents, MCP, coding productivity). Verified license, stars/forks/subscribers and release tag via the GitHub API and npm registry; README read directly to confirm the text-based screen-reading architecture and payment-approval behavior.
