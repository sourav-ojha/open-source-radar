# nestjs-doctor

## Summary
A deterministic (non-LLM) static analysis tool for NestJS. It reads every `@Module()`,
provider, HTTP endpoint, and ORM entity directly from source and scores the project
0–100 across security, correctness, architecture, performance, and schema rules (~52
rules total). It is explicitly positioned to catch mistakes introduced by AI coding
agents — since scans are rule-based and reproducible, the same code scores the same on a
laptop and in CI regardless of which model wrote it.

## Why I Should Care
NestJS is part of his core stack, and his workflow is AI-agent-heavy (this radar itself
is agent-produced). This is a cheap, deterministic backstop for exactly the failure mode
that creates: an agent silently leaving `synchronize: true` on, adding an endpoint with
no guard, or writing a fire-and-forget promise that an LLM-based reviewer might miss or
flag inconsistently.

## Problems It Can Remove
Removes the need to hand-build NestJS-specific ESLint rules for the same checks, and
removes the need to re-review an agent's NestJS output by eye for the specific classes
of mistake it catches (security, correctness, architecture, performance, schema).

## Practical Uses
- Install the agent skill (`npx nestjs-doctor@latest --init`) so a scan re-runs
  automatically after every change a coding agent makes to a NestJS service.
- Gate CI on only the findings a PR introduces (not the existing backlog) via the
  included GitHub Action, with a sticky PR comment and inline review comments.
- Generate the HTML report's module graph, traced HTTP endpoints, and ORM ER diagram
  (Prisma/TypeORM/Drizzle/MikroORM) when onboarding onto an unfamiliar NestJS codebase.
- Add a boot-trace tab to see what each module costs at startup, for a slow-starting
  service.
- Author project-specific rules via the included `nestjs-doctor-create-rule` skill.

## Product Opportunities
Directly applicable as a cheap CI gate for any NestJS service in his stack, including the
MSSP admin-portal SaaS, with no per-scan LLM cost.

## Agent / Automation Opportunities
Ships first-class skills for Claude Code, Cursor, Codex, OpenCode, Windsurf, and Gemini
CLI — the scan is designed to run unprompted immediately after an agent writes NestJS
code, closing the loop on exactly the category of mistake it targets.

## Integration
`npx nestjs-doctor@latest .` — no install required. Also available as a pre-commit hook,
a GitHub Action (`npx nestjs-doctor@latest ci install`), and a VS Code extension. Low
integration effort; a clean scan prints only the score and nothing else.

## Architecture Notes
Pure static analysis over NestJS's own metadata (decorators, modules, providers) and ORM
entity definitions — no LLM call at scan time, which is the deliberate design choice
behind "deterministic." Findings carry a file, a rule id, and (where applicable) a line
number, and every rule/category can be individually configured or disabled via
`nestjs-doctor.config.json`.

## Maturity
Emerging: created 2026-02-18 (~9 months old), actively published to npm with a VS Code
marketplace listing. 185 stars, 4 subscribers, 7 forks, 15 open issues on GitHub — a
thin community-engagement signal on GitHub itself, though the npm/VS Code distribution
suggests real usage outside star-watching.

## License
MIT. No restrictions on commercial or embedded use.

## Alternatives
- NestLens (already catalogued) — a different layer entirely: runtime request/query/
  exception monitoring, not source-level static analysis.
- Hand-rolled custom ESLint rules — would require building all ~52 checks from scratch.
- SonarQube — much heavier general-purpose static analysis, not NestJS-aware out of the
  box.

## Risks / Limitations
- Small project with what looks like a single or very small maintainer team; thin
  community signal (4 subscribers) despite real npm/VS Code distribution.
- Rules could false-positive on unconventional NestJS patterns the rule authors didn't
  anticipate — review findings before wiring `min-score` to block CI.
- Primary docs live on an external site (nestjs.doctor) rather than solely in the repo.

## Recommendation
USE NOW — run `npx nestjs-doctor@latest .` against one existing NestJS service and read
the findings before deciding whether to install the agent skill or wire it into CI.

## Change History
### 2026-11-20
Discovered via a direct `nestjs+devtools` GitHub search during slot 3 (developer
utilities, debugging, testing) discovery, alongside the already-catalogued NestLens and a
thinner Symfony-inspired competitor (eleven-labs/nest-profiler, 18 stars, held out as too
new). Catalogued as USE NOW, 8.4/10.
