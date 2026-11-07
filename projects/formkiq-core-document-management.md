# FormKiQ Core

## Summary
Open-source, API-first document management backend that runs entirely inside your own AWS
account (or locally via Docker) using AWS-native managed services: S3 for storage, API Gateway
for the REST API, Lambda/SQS/SNS/EventBridge for event hooks, with built-in OCR search,
versioning, retention policies, audit trails, and multi-tenant isolation.

## Why I Should Care
This is a document-storage building block already shaped around the exact AWS primitives in
scope (S3, Lambda, SQS, EventBridge, IAM) rather than a generic self-hosted app that happens to
support S3 as one deployment target. Six years of continuous development (since 2020) on a
narrow, well-understood problem — document storage with metadata, search, and compliance-grade
audit trails — rather than feature sprawl.

## Problems It Can Remove
Removes the need to hand-build: S3-backed document upload/versioning, OCR text extraction and
search over PDFs/images, per-tenant access isolation with signed links, and event-driven
enrichment hooks (classify/notify on upload) — all things that otherwise become custom Lambda
glue code on top of raw S3.

## Practical Uses
- Per-tenant document storage for a SaaS product where customer files must stay inside the
  product's own AWS account for compliance reasons.
- A records/retention vault for HR, legal, or finance documents needing audit trails and
  versioning out of the box.
- An ingestion pipeline (S3/email/API) with OCR and metadata tagging in front of a support or
  ops search tool.
- Customer/partner file portals with signed, time-limited access links.

## Product Opportunities
A white-labelable "secure document storage" module for any product needing per-tenant file
isolation and compliance-grade audit trails (e.g., the MSSP admin-portal context) without
building S3 bucket-policy and Lambda-event plumbing from scratch.

## Agent / Automation Opportunities
REST API plus Java/Python SDKs and an OpenAPI spec make this straightforward to wrap as an agent
tool; event hooks via Lambda/SNS/SQS/EventBridge are a natural fit for triggering an agent-driven
classification or extraction step on upload.

## Integration
Deploy to your own AWS account via AWS SAM CLI or CloudFormation, or run locally with Docker
(Netty server package) for evaluation. Serverless, auto-scaling by design — no servers to patch
or capacity to plan. **Integration effort: Medium** — AWS-native deployment is the intended path
and requires comfort with SAM/CloudFormation; the local Docker path lowers this for evaluation.

## Architecture Notes
Worth studying as a reference for "document management entirely on AWS-managed services" —
useful as an architecture pattern even where FormKiQ itself isn't adopted wholesale, given it's
built by people who clearly know the AWS document-handling primitives well.

## Maturity
Mature — active since 2020, latest tagged release v1.19.1 (2026-10-06), pushed today. Smaller
community (164 stars, 6 subscribers) than its maturity would suggest, but six years of sustained
development on a narrow problem is a stronger signal than star count here.

## License
MIT, verified directly against the repository's LICENSE file (the GitHub API misreported this as
NOASSERTION — confirmed by reading the LICENSE file directly, a known API-metadata gap). No
restrictions on commercial use, SaaS deployment, or embedding.

## Alternatives
- Hand-rolled S3 + Lambda + a search index — what this replaces.
- Paperless-ngx and the self-hosted-document-management wave (today's held-out Lemmary) — aimed
  at personal/small-team document archiving rather than a multi-tenant AWS-native SaaS backend;
  different use case.
- Nutrient/PSPDFKit's hosted Document Engine (seen today as an MCP wrapper) — proprietary backend
  dependency, not comparable as a self-hosted option.

## Risks / Limitations
- Small community (6 subscribers) for a 6-year-old project — bus-factor risk if the maintaining
  company's priorities shift.
- AWS-native by design: not a portable, cloud-agnostic document store: migrating off AWS later
  means migrating off this too.
- License metadata on the GitHub API itself is wrong (NOASSERTION); verify the LICENSE file
  directly before relying on automated license scanners.

## Recommendation
PROTOTYPE — worth standing up against a real per-tenant document-storage requirement inside an
existing AWS account before building the equivalent S3/Lambda plumbing by hand.

## Change History
### 2026-11-07
Discovered via GitHub search (slot 4: data, search, documents, RAG). First catalog entry.
