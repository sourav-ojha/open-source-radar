# Open Source Radar

A continuously improving personal technology radar: open-source tools worth adopting,
embedding, self-hosting, or studying — assessed, scored, and tracked over time.

Maintained by a scheduled Claude Code agent running in GitHub Actions. The agent's full
operating spec is `AGENT.md`.

## How it runs

`.github/workflows/radar.yml` fires daily at 07:00 IST (`30 1 * * *` UTC) and on manual
dispatch from the Actions tab. Fully unattended — no computer needs to be online.

| Step | What happens |
|---|---|
| Discovery | `WebSearch` / `WebFetch` — HN, blogs, directories, Awesome lists |
| GitHub metadata, search, releases, licenses | Real `api.github.com`, authenticated with the workflow's own `$GITHUB_TOKEN` (1,000 req/hr) |
| Read / write the catalog | Directly in the checked-out repo, by the agent |
| `git commit` | Done by the agent, every run |
| `git push` | A separate, deterministic workflow step — not the agent — so a push never depends on the agent remembering to run it |
| Digest delivery | The workflow opens a GitHub Issue titled `Open Source Radar — YYYY-MM-DD` with that day's digest, which triggers GitHub's normal notification email (and a phone push if you have the GitHub mobile app) |

Numbers, versions, release dates and licenses come from the GitHub API or package
registries only — never from a WebFetch of a GitHub page, which has been observed
serving stale cached HTML.

## One-time setup

This workflow authenticates with **Sourav's Claude Max plan subscription**, not a
separately-billed API key — no extra dollar cost beyond the subscription already paid
for.

1. **Generate a long-lived OAuth token.** In a real terminal (not a CI shell — this opens
   a browser login), with Claude Code CLI installed and logged into the Max plan account:

   ```sh
   claude setup-token
   ```

   This authenticates via browser OAuth and prints a token tied to that subscription.

2. **Store it as a repo secret.** **Settings → Secrets and variables → Actions →
   New repository secret**, name `CLAUDE_CODE_OAUTH_TOKEN`, paste the token value there
   directly — never in chat, never committed to the repo.

3. That's it. No GitHub PAT is needed either — the workflow's built-in token handles
   both reading GitHub's API and pushing back to this repo.

### The real tradeoff of this auth mode

Anthropic's own docs are explicit: *"Both Pro and Max plans offer usage limits that are
shared across Claude and Claude Code, meaning all activity in both tools counts against
the same usage limits."* This run is not free in the sense of "doesn't touch anything" —
it draws from the **same rolling usage window as every other Claude Code session and
claude.ai conversation on this account.**

Concretely: a ~15-25 minute daily research run (dozens of web searches, GitHub API
lookups, and file writes) will use a real chunk of the account's usage allowance before
the day's interactive coding work even starts, especially if it fires early morning
right before Sourav sits down to work. If Claude Code starts feeling rate-limited on
days the radar runs, this is why — check usage on the account page, and consider moving
the fire time later in the day or thinning `AGENT.md`'s run budget (section 3) if it
becomes a real collision.

The `--max-turns 80` cap in the workflow is a hard ceiling either way, so a single run
can't run away indefinitely regardless of which auth mode is used.

## Costs

- **GitHub Actions minutes:** a run is ~15-25 minutes on a standard Linux runner.
  Free plans include 2,000 minutes/month for private repos — a daily run uses roughly
  500-750/month, comfortably inside that.
- **Claude usage:** no separate dollar cost — see "The real tradeoff" above. The cost is
  paid in shared usage-window headroom, not billing.

## Browse the catalog

https://sourav-ojha.github.io/open-source-radar/ — searchable/filterable table, built
from `catalog.json`. Static page, no build step; auto-updates whenever `catalog.json`
changes on `main`.

## Layout

| Path | What it is |
|---|---|
| `AGENT.md` | The agent's operating spec. **Edit this to change agent behavior.** |
| `.github/workflows/radar.yml` | The scheduled workflow. Edit only for *how* it runs (schedule, permissions, model) — not *what* it does. |
| `catalog.json` | Canonical machine-readable catalog (full records) |
| `catalog.md` | Human-readable catalog table |
| `index.jsonl` | Compact dedupe index — one project per line, grepped every run |
| `state.json` | Run state: evaluated repos, rejections, known releases, source health |
| `daily/` | Daily digests, `YYYY-MM-DD.md` |
| `projects/` | One file per seriously tracked project |
| `monthly/` | End-of-month synthesis |
| `docs/index.html` | GitHub Pages catalog browser (fetches `catalog.json` from `main` at runtime) |

## Adoption statuses

**USE NOW** — immediately useful, low risk · **PROTOTYPE** — worth a real test ·
**STUDY** — architecture worth learning from · **WATCH** — interesting, not ready ·
**IGNORE** — evaluated and rejected

## Tuning

Everything the agent does is defined in `AGENT.md` — discovery emphasis, run budget,
scoring weights, the do-not-recommend list, output formats. Edit that file and push.
The next scheduled run picks it up automatically.

## Manually triggering a run

Actions tab → "Open Source Radar" → **Run workflow**. Useful for testing a spec change
without waiting for the next 07:00 IST fire.
