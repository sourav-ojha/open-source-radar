# CloudBurn

## Summary
CloudBurn is an open-source policy engine of 82 deterministic cost-waste rules for AWS. It runs in two modes: `scan` statically checks Terraform/CloudFormation files for CI/PR gating, and `discover` runs the same rules against a live AWS account via read-only API access plus AWS Resource Explorer. It ships as a CLI (Homebrew/npm, Node 24+), a GitHub Action that annotates and comments on pull requests and can fail a job on severity threshold, a VS Code extension (preview) that flags waste on save, an MCP plugin exposing the rules as read-only agent tools, and an embeddable SDK.

## Why I Should Care
Sourav's stack is AWS-heavy (EC2, Lambda, EKS, ALB, CloudFront, S3) — this is a direct, zero-infrastructure way to catch cost-waste patterns in Terraform before they ship, rather than discovering them on a monthly AWS bill. It's also a mature, actively-developed project from a known AWS-focused consultancy, not a weekend tool.

## Problems It Can Remove
- Discovering AWS cost waste only after it shows up on the monthly bill
- Manually reviewing every infra PR for known waste patterns (oversized instances, idle resources, missing lifecycle policies)
- Building a custom cost-waste rule engine from scratch for CI gating

## Practical Uses
- Add the GitHub Action to fail a PR when a Terraform change introduces a known AWS cost-waste pattern, before it ever applies
- Run `discover` against an existing AWS account for a one-time inventory of current waste patterns across EC2/EB/ALB/CloudFront/S3/Lambda/EKS
- On-save VS Code flagging while writing Terraform
- Give an AI coding agent the MCP plugin so it checks its own generated Terraform against the same 82 rules before proposing it
- Embed the SDK inside an internal platform tool that reviews infra-change PRs automatically

## Product Opportunities
The rule-engine-plus-CI-gate pattern (deterministic rules, not an LLM estimate) is a credible building block for a FinOps feature inside an internal platform, or a client-facing MSSP cost-review offering.

## Agent / Automation Opportunities
The MCP plugin exposes the 82-rule set as read-only tools an agent can call while drafting or reviewing Terraform. The SDK allows embedding the same rule evaluation inside a custom agent tool rather than shelling out to the CLI.

## Integration
CLI via Homebrew or npm (Node 24+); GitHub Action for CI; VS Code extension (explicitly preview); SDK for embedding. No hosted service — everything runs under your own control plane with your own AWS credentials. Integration effort: **Low** — the CLI and GitHub Action are the fastest path to value.

## Architecture Notes
- `scan` (static, Terraform/CloudFormation) and `discover` (live account, via Resource Explorer) share the same 82-rule engine rather than being separate codepaths
- `discover init` requests `resource-explorer-2:UpdateView` — a write permission beyond pure read-only — specifically to enable tag visibility for one opt-in rule; worth reviewing before granting broadly
- Rule IDs are pinned with a documented compatibility policy, so CI configs referencing specific rule IDs don't silently break on upgrade

## Maturity
Mature: created 2021-03-13 (5+ years old), 1,802 stars, 370 forks, 43 subscribers, 57 open issues, pushed 2026-11-08. Maintained by towardsthecloud, a known AWS-focused consultancy/newsletter. Healthy star:subscriber ratio (≈42:1) relative to many repos reviewed in this radar.

## License
Apache-2.0, no restrictions. The README presents only the open-source CLI/Action/extension/SDK — no hosted or paid tier is mentioned.

## Alternatives
Infracost (cost estimation, not waste-pattern detection — complementary rather than overlapping). cloud-custodian (broader cloud-governance DSL, steeper setup) and OpenCost/krr (Kubernetes-workload cost, a different layer). AWS Trusted Advisor / Compute Optimizer (built-in but less transparent rule logic, AWS-account-coupled, no CI-gate workflow).

## Risks / Limitations
- The README's own tagline — "blocks bad AWS spending patterns before they ship and remediates what's already burning" — overstates the actual capability. `discover` only reports what needs fixing; the README body does not describe any automated remediation/auto-fix action. Treat "remediates" as marketing language until verified hands-on, per AGENT.md's standard against unsupported claims.
- `discover init` requests one write IAM permission, not purely read-only
- VS Code extension is explicitly a preview; no single overall version/stability statement is given despite the project's age

## Recommendation
USE NOW — the `scan` CLI and GitHub Action are low-effort to add to an existing Terraform CI pipeline immediately; treat `discover`'s live-account mode and the "remediates" claim with the caveats above before trusting it broadly.

## Change History
### 2026-11-09
First reviewed and catalogued. Slot 6 (infrastructure, observability, deployment) run. Confirmed via GitHub API (Apache-2.0, 1,802 stars, pushed 2026-11-08, created 2021) and README inspection (raw.githubusercontent.com) covering scan/discover modes, AWS permission requirements, and the unsupported "remediates" claim.
