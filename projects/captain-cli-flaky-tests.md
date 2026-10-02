# Captain CLI

## Summary
An open-source CLI from RWX that detects and quarantines flaky tests, automatically retries failed tests, partitions test files for parallel CI execution, and generates markdown test-failure summaries. Compatible with most modern test runners/frameworks via CI integration rather than framework-specific plugins.

## Why I Should Care
Flaky tests are a recurring tax on any CI pipeline — they erode trust in the test suite and waste time re-running builds by hand. Captain automates the two most common manual workarounds (re-run the flaky test, mentally ignore the test everyone knows is flaky) plus adds test partitioning for parallel CI, which is otherwise something teams hand-roll with ad hoc sharding scripts.

## Problems It Can Remove
- Manually re-running CI jobs that failed only because of a known-flaky test.
- Hand-written test-sharding/partitioning scripts for parallel CI execution.
- Digging through raw CI logs to find which tests actually failed — Captain produces a markdown failure summary.

## Practical Uses
- Wrap an existing CI test step with Captain to get automatic retry + quarantine of flaky tests without changing test code.
- Use the partitioning feature to split a large test suite across parallel CI jobs without maintaining a manual shard-assignment file.
- Pipe Captain's markdown failure summary into a PR comment or Slack notification for faster failure triage.

## Product Opportunities
None directly — this is an internal CI-productivity tool, not an embeddable component.

## Agent / Automation Opportunities
Fits naturally as a CI pipeline step; the markdown failure summary output is well suited to being consumed by an agent (e.g., a coding agent that triages CI failures and proposes fixes).

## Integration
Go binary CLI, installed in a CI runner and wrapped around existing test invocation commands. Low effort for the baseline retry/quarantine/partition features. **Unclear based on available documentation** whether the flaky-test-history state that powers quarantine decisions is purely local or requires sending data to RWX's backend — the README and fetched docs page confirm the core CLI features (retry, quarantine, partition, failure summaries) are part of the open-source release, while analytics dashboards and automatic suite rebalancing are explicitly gated behind RWX's paid Cloud tier. Worth confirming data-residency requirements before adopting in a commercial pipeline with sensitive test data.

## Architecture Notes
Go CLI (Mage-based build), company-maintained with a clear open-core split: CLI is MIT-licensed and does the mechanical work (retry/quarantine/partition), while RWX's hosted Cloud product layers analytics and historical rebalancing on top. This two-tier model is a reasonable pattern to study for anyone considering an open-core CLI-plus-hosted-dashboard product structure.

## Maturity
Mature as a product — company-backed (RWX), repo created 2023, actively released (v2.9.0 as of 2026-09-30), documented, Discord community. Relatively modest star count (99) for its age, likely because the open-source CLI release itself is newer than the repo ("Captain 1.10 Generally Available Open Source Release" blog post) — i.e., it may have been closed-source/private for much of its history.

## License
MIT, verified via GitHub API. No restrictions on commercial use. The associated RWX Cloud platform is a separate proprietary paid product — flagging per AGENT.md §14 that "open source" here covers the CLI mechanics, not the full analytics/rebalancing feature set.

## Alternatives
BuildPulse, Trunk Flaky Tests, Datadog Test Visibility — these are mostly paid SaaS products for flaky-test management; Captain's distinguishing feature is that the baseline retry/quarantine/partition functionality is a free, open-source CLI rather than a paid dashboard subscription.

## Risks / Limitations
Ambiguity about exactly which features work fully standalone vs. require an RWX account is a real adoption risk — confirm before committing a CI pipeline to it. Vendor dependency on RWX for the advanced tier.

## Recommendation
PROTOTYPE — worth a trial run on a flaky CI suite to confirm how much value the open-source CLI alone delivers before considering the paid Cloud tier.

## Change History
### 2026-10-02
Initial discovery and review.
