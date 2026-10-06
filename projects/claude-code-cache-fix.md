# claude-code-cache-fix

## Summary
A local proxy that sits between the Claude Code CLI and Anthropic's API, rewriting outgoing `POST /v1/messages` requests to repair prompt-cache bugs the maintainers attribute to Claude Code itself: unstable request-prefix ordering, silent TTL 5m downgrades, thinking-desync 400s, and image-retry storms — all of which can cause repeated `cache_creation_input_tokens` spikes instead of cheap `cache_read` hits, inflating token spend on long or resumed sessions by as much as 20x per the project's own claim.

## Why I Should Care
This is one of the few radar entries that applies directly to how this very radar runs — Claude Code is the tool. If cache misses on resumed sessions are real and this fixes them, it is a direct, measurable cost reduction on a tool already in daily use, not a hypothetical "could be useful someday" find.

## Problems It Can Remove
- Removes unexplained token-cost spikes on long-running or resumed Claude Code sessions caused by cache-prefix instability rather than by the work actually done.
- Removes the need to manually notice and work around TTL downgrades or retry storms — the proxy normalizes request structure before it reaches Anthropic.

## Practical Uses
- Run it locally in front of any Claude Code session to see whether cache-read ratio improves on resumed/long sessions.
- Use the bundled diagnostics to confirm whether a specific project's sessions are actually hitting the bug before deciding to keep it running permanently.

## Product Opportunities
None directly — this is a personal/developer-tooling fix, not a product building block.

## Agent / Automation Opportunities
Not an agent tool itself; it's infrastructure that makes an existing agent tool (Claude Code) cheaper to run. Relevant as an item to evaluate for any team running Claude Code at volume.

## Integration
`npm install -g claude-code-cache-fix`, binds to `127.0.0.1` by default, and is idempotent — if nothing needs fixing it passes requests through unmodified. Integration effort: **Low** to try, but see Risks before leaving it enabled by default.

## Architecture Notes
It is explicitly a request-rewriting proxy, not a passive logger: the README states plainly that it reads and rewrites `POST /v1/messages`, because that rewrite *is* the fix. By default it makes no outbound calls beyond relaying to Anthropic and writes telemetry only to local files under `~/.claude/`. Two opt-in features add their own egress: OAuth-refresh calls to Anthropic's token endpoint, and a forward-proxy download-acceleration mode. A separate `--remote-control` forward-proxy mode terminates TLS for `api.anthropic.com` using a locally generated CA that the client must trust — a materially higher-trust mode than the default.

## Maturity
Emerging but actively maintained: created April 2026, latest release v4.3.0 (July 2026), 436 stars, 37 forks including a GUI and a VS Code extension built on top of it, 80 closed / 42 open issues via GitHub search (responsive issue turnover). One independent third-party assessment is linked from the README as calling it "a legitimate tool," which is useful context but not a substitute for reading the code.

## License
MIT — confirmed from the raw `LICENSE` file and the npm registry metadata. (Note: the GitHub API's license-detection endpoint reports `NOASSERTION` for this repo, likely because of the non-standard multi-author copyright line in the LICENSE file's header; the file content itself is standard MIT. Don't trust the API field alone here — this is exactly the kind of detail AGENT.md flags as worth double-checking against the actual file.)

## Alternatives
Doing nothing and eating the cost regression; filing/following the underlying Claude Code bug reports directly; the handful of thinner forks and reimplementations around this repo (a GUI wrapper, a VS Code extension, a mitmproxy-based clone) which have far less traction and no independent assessment.

## Risks / Limitations
- It intercepts and rewrites live traffic to Anthropic's API for a tool used daily — treat it as security-sensitive infrastructure, not a casual npm install. Read the "Security model" section of the README before enabling anything beyond the default local-proxy mode.
- The optional `--remote-control` TLS-termination mode requires trusting a locally generated CA; only worth it if the default mode doesn't capture the savings and the trust tradeoff is deliberately accepted.
- Open-issue count (42) is nontrivial for a 436-star repo — worth checking current open issues for anything that looks like a correctness regression before relying on it.
- Built against Anthropic's own undocumented cache behavior; a future Claude Code release could change the request format in a way that breaks or obsoletes this fix without notice.

## Recommendation
PROTOTYPE — worth running in default (non-TLS-terminating) mode on a real Claude Code workflow for a week to measure the actual cache-read ratio before and after, given the direct relevance to daily tool cost. Do not enable the forward-proxy/TLS-termination mode without a specific reason to need it.

## Change History
### 2026-10-06
Initial discovery and review. Slot 7 (experimental projects and hidden gems) run.
