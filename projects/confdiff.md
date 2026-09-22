# confdiff

## Summary
confdiff is a small CLI/npm package that diffs config and structured-data files (JSON, YAML, TOML, INI, .env, .properties, CSV, XML) by parsing them into a data model and comparing values, not text — so reordered keys, reflowed arrays, requoting, and comment/indentation changes don't show up as noise. A `--redact` flag masks secret-looking values as stable fingerprints so a password/token rotation is visible without the actual secret ever landing in a diff, PR comment, or CI log.

## Why I Should Care
`git diff` on a reordered YAML file or a re-serialized `.env` is a wall of noise that hides the one real change. This is exactly the kind of small, narrow, recurring annoyance the "hidden gem" test in AGENT.md section 11 is built for — a few minutes saved per config change, many times a week, across every service in a MERN/microservice stack.

## Problems It Can Remove
- Manually eyeballing large config diffs to find the one meaningful change among reordering/requoting noise.
- Accidentally pasting a secret value into a PR review comment or CI log when diffing environment files.
- Writing one-off `jq`/`yq` comparison scripts per format.

## Practical Uses
- Reviewing infra config PRs (Kubernetes manifests, `docker-compose.yml`, Terraform `.tfvars`-adjacent JSON) without noise from key reordering.
- Comparing `.env` files across environments (staging vs. prod) safely, with `--redact` protecting credential values.
- CI gate: fail a pipeline only on semantically meaningful config drift, not formatting churn.

## Product Opportunities
Small enough to vendor the redaction/semantic-diff logic directly into an internal admin panel's "config change preview" feature rather than shelling out.

## Agent / Automation Opportunities
Good fit as a coding-agent tool call — an agent proposing an infra config change could run confdiff to summarize its own diff for a human reviewer in plain "what changed" terms instead of a raw text diff, and the redaction feature makes it safe to include that summary directly in an agent's output.

## Integration
- **Installed locally**: `npm install -g confdiff` (or npx), also usable via a 100%-client-side browser page (paste two configs, nothing uploaded).
- **CLI-invoked**: yes, primary interface.
- **Interfaces**: CLI; no library API, HTTP API, or MCP surface documented.
- **Integration effort: Low** — single small CLI tool, no configuration required to get useful output.

## Architecture Notes
Nothing unusual architecturally — parse-to-model-then-diff is a well-understood pattern (similar in spirit to Difftastic's AST-aware diffing, applied to config formats instead of source code). The notable detail is process, not architecture: the README states the project "is built and maintained by an autonomous AI agent" (Esperanza Volkov) that reads and acts on issues/PRs directly — worth knowing before filing an issue and expecting a human on the other end.

## Maturity
Emerging but usable. Created August 2026 (~1 month old), 41 stars, published to npm with CI badge, v0.17.4 tagged release. Small utility scope limits how much "maturity" even means here — the surface area is narrow.

## License
MIT — no commercial-use restrictions.

## Alternatives
`dyff`, `json-diff`, `yq`+`diff` one-liners, Difftastic (general-purpose semantic diff, not config-specific). confdiff's differentiation is the secret-redaction feature combined with broad format coverage (8 formats) in one small tool — none of the alternatives combine both.

## Risks / Limitations
- Very new (1 month) and small (41 stars) — limited real-world battle-testing yet.
- Maintained by an autonomous AI agent rather than a human maintainer — response quality/judgment on edge-case bug reports is unproven.
- Narrow scope by design; not a replacement for a full diff/review tool, just the config-noise problem specifically.

## Recommendation
USE NOW — low-risk, immediately useful, install and start using it on the next config-heavy PR review.

## Change History
### 2026-09-22
Initial discovery and review. Filed as Small but High-Leverage Utility of the day, rotation slot 7 (experimental/hidden gems).
