# Open Source Radar

A continuously improving personal technology radar: open-source tools worth adopting,
embedding, self-hosting, or studying — assessed, scored, and tracked over time.

Maintained by a scheduled Claude agent. The agent's full operating spec is `AGENT.md`.

## How it runs

The radar is a git clone on the Mac at `~/workspace/personal/open-source-radar`, connected
as a folder to the scheduled Claude session. **That clone is the single source of truth.**

| Step | Where it happens | Why |
|---|---|---|
| Web discovery (HN, blogs, directories, Awesome lists) | Cloud session, `WebSearch` / `WebFetch` | Only path to the open web |
| GitHub metadata, search, releases, licenses | Mac shell, real `api.github.com` | Cloud container's GitHub traffic is intercepted and refused |
| Reading / writing the catalog | Mac shell, in this folder | Cloud container is destroyed after every run |
| `git commit` | Mac shell, in this folder | Automatic, every run |
| `git push` | **Manual — you, from your own terminal** | Remote is SSH; the session shell has no key |

Numbers, versions, release dates and licenses come from the GitHub API only. WebFetch on
GitHub pages returns cached HTML that has been observed months stale — the spec forbids
sourcing figures from it.

## Layout

| Path | What it is |
|---|---|
| `AGENT.md` | The agent's operating spec. **Edit this to change agent behavior.** |
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

## Pushing

The agent commits but never pushes. To sync:

```sh
cd ~/workspace/personal/open-source-radar && git push
```

## Tuning

Everything the agent does is defined in `AGENT.md` — discovery emphasis, run budget,
scoring weights, the do-not-recommend list, output formats. Edit that file and commit.
The next run picks it up. The scheduled task itself never needs to change.

## Optional: higher GitHub rate limits

Unauthenticated GitHub API is 60 requests/hour, which constrains each run. To lift it to
5,000/hour, export `GH_RADAR_TOKEN` on the Mac with a fine-grained read-only token. The
spec uses it automatically if present, and works without it.
