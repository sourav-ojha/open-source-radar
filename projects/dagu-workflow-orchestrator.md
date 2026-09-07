# Dagu

## Summary
Dagu is a local-first workflow engine for operations and internal automation. It ships as a single self-hostable binary with a built-in Web UI, no external database or message broker, and runs DAGs defined in declarative YAML on Linux, macOS, and Windows. It wraps existing shell commands, Docker containers, Kubernetes Jobs, and SSH commands without requiring them to be rewritten into a framework's task/decorator model.

## Why I Should Care
Cron gives no dependencies, retries, or history. Airflow gives all of that but requires operating a second platform (scheduler, metadata DB, workers, a Python runtime) and rewriting jobs as `@dag`/`@task` code. Temporal moves business logic into its SDK. Dagu keeps orchestration as configuration sitting next to scripts that already work — delete the YAML and the scripts still run exactly as before.

## Problems It Can Remove
- Replaces brittle, unmonitored cron jobs with dependency graphs, retries, and a run-history UI.
- Removes the need to stand up Airflow (scheduler + Postgres + Redis/RabbitMQ + workers) just to get scheduling and observability around a handful of scripts.
- Removes the "why did this silently fail at 3am" investigation cycle — logs, retries, and notifications are built in.

## Practical Uses
- Turn data-extraction scripts, SQL queries, and dbt commands into observable ETL pipelines.
- Orchestrate maintenance/deploy scripts over SSH across a small server fleet.
- Wrap ffmpeg media-conversion jobs with parallel worker fan-out.
- Trigger workflows from GitHub events to run automation on private infrastructure without exposing servers publicly.
- Give non-engineering teammates a self-service Web UI button for routine diagnostics instead of escalating to engineering.

## Product Opportunities
- Internal ops backbone for the MSSP admin-portal SaaS: scheduled scans, report generation, client onboarding sequences, without owning a Celery/Airflow stack.
- Because it's a single binary with file-backed state, it fits well as an embedded automation layer inside a larger self-hosted product.

## Agent / Automation Opportunities
Ships a built-in MCP server for inspecting workflows and runs, maintaining wiki pages, applying changes, and controlling runs — usable directly as an agent tool without building a wrapper.

## Integration
Single binary install (`dagu start-all`), or Docker. No external DBMS or message broker required. Scales from a single node to a fleet of workers when needed. Integration effort: **Low**.

## Architecture Notes
State is stored in local files rather than a database, which is what allows a single machine to reach thousands of workflow runs/day without external services. Workflow structure is treated as pure configuration (YAML) — the engine that runs it is a separate single process from the scripts it calls, so nothing gets locked into a bespoke SDK.

## Maturity
Mature. Created 2022, actively maintained (v2.16.2 released 2026-09-02, days before this review). 3,841 stars, 88 contributors.

## License
GPL-3.0. Not AGPL/SSPL, but still copyleft. No practical restriction for running it internally as an ops tool (not modifying and redistributing it). Would need review before embedding/redistributing it as part of a shipped commercial product.

## Alternatives
Apache Airflow, Temporal, n8n, plain cron, GitHub Actions self-hosted runners. Dagu's distinction is zero external infrastructure and zero rewrite of existing scripts.

## Risks / Limitations
- GPL-3.0 requires care if ever embedding/redistributing rather than just running it internally.
- Single-node file-backed state is the well-trodden path; multi-worker scale-out is supported but less proven than the single-node case.
- Smaller community than Airflow means fewer third-party integrations and community answers.

## Recommendation
USE NOW — low integration risk, immediately useful as a cron/Airflow replacement for internal automation, and the built-in MCP server is a bonus for agent-driven ops.

## Change History
### 2026-09-07
Initial discovery and review. Slot 6 (infrastructure, observability, deployment) run.
