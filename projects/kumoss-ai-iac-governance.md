# Kumoss

## Summary
Kumoss is an AI-agent-driven Infrastructure-as-Code platform from Inditex (Zara's parent company). A user describes an infra change in plain language; Kumoss gathers the live deployed state, the existing IaC repository, and the organization's compliance standards, generates Terraform/OpenTofu code, runs and retries a plan until it passes, corrects drift against live state in the same pull request, runs a compliance-agent review that flags high-impact or non-compliant changes for administrator approval, and opens the final pull request with an estimated cost and impact summary. Multicloud (AWS, Azure, GCP, OCI, Kubernetes, VMware vSphere); bring-your-own LLM via LiteLLM.

## Why I Should Care
This is the first project found in this radar that treats natural-language-to-infra-change as a governed pipeline rather than just pointing a coding agent at a Terraform repo and hoping for a clean diff — and it's open-sourced by a company running it internally, not a weekend demo. The policy-gate pattern (lock the session, require admin approval for anything flagged high-impact or non-compliant) is directly reusable for any product that needs to give an AI agent write access to something sensitive.

## Problems It Can Remove
- Hand-writing Terraform diffs for routine day-2 infra changes (resize, add a resource, adjust an IAM policy)
- Manual `terraform import` sessions for console-created resources
- Ad hoc drift discovery — drift is fixed automatically against live state as the source of truth, folded into the same PR as the requested change

## Practical Uses
- Day-2 AWS/Terraform changes requested in plain language instead of hand-authored diffs
- Giving a non-infra teammate a gated, reviewable path to request an infra change without direct cloud console or Terraform access
- Importing console-created resources into existing state without a manual import workflow
- Studying the compliance-agent-review / policy-locked-session pattern as architecture worth copying into any product giving an AI agent write access to production systems

## Product Opportunities
The policy-gate / compliance-review pattern is reusable well beyond IaC — directly relevant to both his own AWS infra and the MSSP/PTaaS side, where "an AI agent proposes a change, a compliance step reviews it, a human approves anything risky" is a pattern clients would pay to have built into their own tooling.

## Agent / Automation Opportunities
Kumoss is itself an agent product (an LLM-driven pipeline: request → plan → drift-fix → compliance review → PR) rather than a tool an external agent calls. MCP/A2A support is explicitly listed on the roadmap but not yet implemented, so it isn't currently usable as a tool inside an existing agent stack. Bring-your-own LLM via LiteLLM means a self-hosted model can drive the whole pipeline, avoiding a hosted API key dependency.

## Integration
Local/full-stack run via Docker Compose: FastAPI API, React frontend, PostgreSQL, Redis, RustFS, SearXNG, nginx, OpenTelemetry, and Arize Phoenix for tracing. A separate "deploy to production" guide is referenced but not detailed in the README. Identity (any OIDC provider), the model (any LiteLLM-supported model), authorization (adopter must implement the authorization contract), and the IaC executor runtime are all bring-your-own. Integration effort: **High** — this is a multi-service platform, not a single binary, and authorization is not provided out of the box.

## Architecture Notes
- Nine-step flow: request → auth/project-access check → gather deployed state + IaC repo + request + org standards → generate code and run/retry a plan → fix drift and report impact/cost → compliance-agent review flags high-impact/non-compliant proposals → risky changes lock the session for admin approval → user applies and gets a result report
- Deployed cloud state is treated as source of truth for drift; the fix is included in the same PR as the requested change rather than a separate drift-remediation step
- Clean separation of bundled vs. bring-your-own: model, identity, and authorization are all adopter-supplied against documented contracts; only the IaC executor (OpenTofu/Terraform) and Slack notifications are bundled by default

## Maturity
Emerging: created 2026-05-06 (~6 months old), 145 stars, 8 forks, 1 subscriber, 57 open issues, v1.0.8 released 2026-11-05. An OpenSSF Scorecard badge and a security disclosure program are both present — a stronger-than-typical signal for a 6-month-old repo. The README does not explicitly call the project production-ready, and states the public roadmap "reflects intent, not commitment."

## License
Apache-2.0, SPDX header confirms copyright INDITEX S.A. (2026). No restrictions on commercial, internal, or embedded use.

## Alternatives
Manual Terraform/OpenTofu workflows with a human-authored PR (the status quo this replaces). Atlantis, Terraform Cloud, Spacelift, env0, and the already-catalogued Terraducktel (self-hosted Terraform Cloud alternative) all gate plan/apply on human-authored code — none of them generate the code itself from a natural-language request. Generic "ask a coding agent to write Terraform" prompting has no drift correction, no compliance-agent review, no cost/impact summary, and no audit trail.

## Risks / Limitations
- Significant self-hosting footprint (six-plus supporting services) — not a lightweight tool to stand up just to try it
- Authorization must be implemented by the adopter against Kumoss's contract; nothing ships out of the box
- Scheduled drift detection and the Helm chart are both listed as in progress
- Only 1 subscriber against 145 stars and 8 forks — an elevated ratio, though 57 open issues suggest real usage rather than inflated stars; weigh against this being a single corporate sponsor's 6-month-old project

## Recommendation
PROTOTYPE — the governance pattern (plan retry, drift-as-source-of-truth correction, compliance-gated approval) is genuinely novel and worth running against a real sandbox AWS account, but the multi-service setup and unimplemented authorization mean it needs a deliberate trial rather than a quick spin-up.

## Change History
### 2026-11-09
First reviewed and catalogued. Slot 6 (infrastructure, observability, deployment) run. Confirmed via GitHub API (Apache-2.0, 145 stars, pushed 2026-11-09, v1.0.8) and README inspection (raw.githubusercontent.com) covering the request-to-PR flow, bundled-vs-BYO component table, and policy-gate behavior.
