# redStackPRO

## Summary
redStackPRO is a visual canvas for composing red-team/cyber-range infrastructure as a topology, which it then compiles into a self-contained export of roughly 200 Terraform and Ansible files you deploy yourself — the project never holds or touches cloud credentials ("the export is the boundary"). It covers both offense (split-horizon C2 infra fronting Sliver/Mythic) and defense/training ranges (Active Directory labs including the GOAD lab), for use only in environments you own or are explicitly authorized to test.

## Why I Should Care
Standing up a cyber range or a C2 lab by hand means writing and maintaining Terraform/Ansible for networking, jumpboxes, and per-tool provisioning every time — exactly the kind of one-off infra scaffolding that eats a day before any actual PTaaS engagement or AD-lab training starts. redStackPRO turns that into a validated topology document that compiles to a reviewable, runnable export, with lifecycle scripts (status/start/stop/teardown) and a scrubbed deploy log included.

## Problems It Can Remove
- Hand-written Terraform/Ansible for range networking, jumpbox access, and per-operator VPN/portal credentials on every new engagement
- Ad hoc tracking of "what's planted where" in a lab (the export includes a briefing document listing credentials and planted items)
- Rebuilding known AD-lab topologies (a two-domain forest, or the full three-domain GOAD lab) from scratch each time

## Practical Uses
- Spin up an isolated, authorized C2 lab (Sliver behind Apache, Mythic behind Nginx, peered networks behind a jumpbox) for internal red-team practice or client-authorized PTaaS work
- Stand up a GOAD-based Active Directory range for defensive training or tool validation
- Let an automated agent validate/fix/compile a topology document via the REST API, without going through the canvas UI

## Product Opportunities
Directly relevant to the PTaaS partnership context: a validated, reviewable topology-to-Terraform pipeline for range setup could shorten engagement-prep time, provided every deployment stays inside explicitly authorized scope.

## Agent / Automation Opportunities
The API/CLI (`serve`, `validate`, `compile`) is explicitly designed to be driven by an automated agent independent of the GUI — a coding agent could validate and iterate on a topology document directly.

## Integration
Single Docker container, with an optional Postgres backend for shared/multi-user use. `deploy.sh` in the export applies the Terraform and then provisions through the range's jumpbox. Integration effort: **Medium** — the compile step and credential/VPN setup require actually reading the generated export before running it, not a one-command deploy.

## Architecture Notes
- Topology documents are JSON validated against a published schema; the compiler refuses to emit anything from a document with validation errors
- Export includes verification scripts and timestamped, scrubbed deploy logs, not just raw Terraform/Ansible
- API currently trusts the local caller with no multi-tenant auth — the project states multi-tenant auth is an enterprise-edition feature, not present in the open-source core
- AWS quota limits (Elastic IPs, vCPUs) are not checked at compile time

## Maturity
Pre-1.0 beta (v0.9.1 as of 2026-10-03). Solo-maintainer project (1 contributor) under an organization created 2026-09-25, with 141 stars and 16 forks in roughly two weeks — fast enough traction for a two-week-old security tool that it's worth watching whether that holds up rather than taking it as settled community validation yet. GCP and AWS are tested end-to-end; Azure/Proxmox/ESXi are roadmap items. All operators currently share one lab portal password (per-operator passwords are planned).

## License
MIT for the core project. The bundled GOAD lab templates (`frontend/public/goad/`) are derived from the upstream GOAD project and remain GPLv3 with attribution — relevant only if redistributing those specific templates, not for using the tool itself.

## Alternatives
Hand-rolled Terraform/Ansible per engagement (the status quo this replaces), Ludus (self-hosted cyber-range automation, more mature/established in this space), direct use of GOAD/Detection Lab templates without a compiling topology layer on top.

## Risks / Limitations
- Dual-use by nature (C2/red-team infrastructure generation) — the README itself scopes this to authorized lab use only, and that scoping should be treated as load-bearing, not boilerplate
- Solo maintainer, two weeks old; the star/fork count is notable for that age and worth re-checking in a few weeks rather than treated as proven traction now
- No multi-tenant auth in the open-source core — don't expose the API beyond a trusted operator without adding a boundary in front of it
- AWS quota limits unchecked at compile time can produce a partially-deployable export

## Recommendation
PROTOTYPE — strictly for authorized lab/range use (internal red-team practice, AD-lab training, or client-authorized PTaaS engagements). The export-as-boundary design and GOAD integration are a genuine time-saver for range setup; the newness and solo-maintainer status mean it belongs in a throwaway lab account first, not a client engagement, until it's had more real-world mileage.

## Change History
### 2026-10-12
First reviewed and catalogued. Confirmed via GitHub API (MIT core license, GOAD templates GPLv3, 141 stars, 16 forks, 1 contributor, org created 2026-09-25) and README inspection (raw.githubusercontent.com).
