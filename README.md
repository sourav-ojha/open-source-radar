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

1. **Add a Claude API key as a repo secret.** Create one at
   [platform.claude.com](https://platform.claude.com) (Console → API Keys), then in this
   repo: **Settings → Secrets and variables → Actions → New repository secret**, name
   `ANTHROPIC_API_KEY`. Set it there directly — never paste an API key into chat.
   Consider setting a monthly spend limit on the key in the Console as a safety cap.
2. That's it. No GitHub PAT is needed — the workflow's built-in token handles both
   reading GitHub's API and pushing back to this repo.

## Costs

- **GitHub Actions minutes:** a run is ~15-25 minutes on a standard Linux runner.
  Free plans include 2,000 minutes/month for private repos — a daily run uses roughly
  500-750/month, comfortably inside that.
- **Anthropic API usage:** billed to the `ANTHROPIC_API_KEY` above — separate from any
  claude.ai subscription. Covers model tokens plus web search ($10 per 1,000 searches).
  Actual daily cost depends on run size; watch the first week in the Console's usage
  dashboard and set a budget alert.

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
