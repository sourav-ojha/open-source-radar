# deja-vu

## Summary
Local agent-session memory built entirely from the plaintext session logs that Claude Code, Codex, Cursor, and 38 other agent harnesses already write to disk — no LLM calls, no embeddings, no server. `deja` indexes months of history on first run, then serves recall over MCP and hooks at session start, before an edit, and after a command fails. Claims to survive context compaction (keeping 77% of decisions across 43 measured compactions, per the maintainer's own benchmark) and tracks which past decisions were later reverted so a later hit can say a fix was tried and dropped. Ships as a single Go binary; can sync session history between machines over SSH with no cloud step.

## Why I Should Care
Every agent-memory tool already in the catalog (Hippo, and previously-evaluated MemPalace) extracts and scores memory using an LLM. deja-vu instead indexes transcripts the agents are already producing, which means zero added inference cost per recall and no risk of a hallucinated memory being treated as fact. Directly useful across a MERN+AI-agent workflow that already spans Claude Code, Codex, and Cursor in the same projects.

## Problems It Can Remove
Re-explaining a decision made in a different agent harness months ago (a fix found in Codex surfaces again automatically in Claude Code), and re-deriving context lost to a `/clear` or a context-window compaction.

## Practical Uses
- Session-start auto-recall so an agent already knows what was solved in a project when a new session opens
- Manual search (`deja "connection pool exhausted"`) over any agent's history, filterable by harness, project, or date
- `deja promote <id> --state rejected` to mark a reverted decision so future hits warn it was tried and dropped
- `deja sync ssh laptop` to carry agent-session memory between two machines with no cloud intermediary
- `deja handoff --to codex` to continue a Claude Code session in Codex with what was asked, decided, and left open

## Product Opportunities
None directly — this is personal/team tooling rather than an embeddable product component.

## Agent / Automation Opportunities
Wires into an agent via MCP and hooks in one command (`deja install --auto`); also leaves a `SKILL.md` teaching an agent the CLI contract directly, so recall works even without MCP wired up.

## Integration
Deployment: `curl ... | sh` + `deja install --auto` (ten seconds to install, ten to index); also via Homebrew, Scoop, winget, npm (`npx @vshulcz/deja-vu`), or `go install`.
Interfaces: CLI, TUI, MCP server.
Integration effort: **Low** — single binary, no server, no model to host.

## Architecture Notes
Parses each supported harness's own on-disk session format directly rather than normalizing through an LLM. Recall quality is driven by structural indexing (files touched, commands run with exit status, exact spans an edit replaced) rather than semantic embeddings, which is also why it reports zero added inference cost per query.

## Maturity
Emerging. Repo created 2026-07-14, current release v0.22.1 (2026-10-12), MIT license, active CI. Star:watcher ratio is elevated (1,192 stars vs 5 watchers) for a 3-month-old, single-maintainer project — treat growth as unverified. All recall/token/index-speed benchmark numbers in the README are self-run by the maintainer against a synthetic corpus, not independently reproduced.

## License
MIT, confirmed via GitHub API license field. No restrictions.

## Alternatives
- Hippo (`kitfunso/hippo-memory`, already catalogued) — LLM-based memory with an explicit wrongness/decay signal; different approach (inference-based extraction vs. direct indexing)
- MemPalace — evaluated previously but not catalogued; deja-vu's README benchmarks directly against it (claims 97.2% R@5 on LongMemEval-S vs. MemPalace's claimed 96.6%)
- Headroom (already catalogued) — solves a different problem: compressing tool/RAG output before it hits the context window, not recalling past sessions

## Risks / Limitations
- All comparative benchmark claims are self-reported by the maintainer; no third-party reproduction found
- Elevated star:watcher ratio for a young, single-maintainer project
- Depth of harness-specific integration likely varies across the 41 claimed supported agents; the install script itself only targets macOS/Linux (Windows goes through Scoop/winget/a release zip instead)

## Recommendation
**PROTOTYPE** (score 8.0/10) — worth a real trial across one real multi-agent project (e.g. Claude Code + Codex on the same repo) before trusting it as a daily driver.

## Change History
### 2026-10-14
First discovered via GitHub search API (`claude code hook`, pushed:>2026-09-20) during the slot 1 run.
