# S3 Encryption Gateway

## Summary
A transparent HTTP proxy that sits between any S3-compatible client and an S3-compatible backend
(AWS, MinIO, SeaweedFS, Hetzner, etc.), encrypting objects client-side on write and decrypting on
read, without changing application code. Supports envelope encryption (AES-256-GCM or
ChaCha20-Poly1305 with a KEK/DEK split) or password-derived keys, chunked streaming, and range
requests.

## Why I Should Care
The realistic S3 data-exposure risk isn't a compromised cloud provider — it's a misconfigured IAM
policy, an overly broad bucket policy, or a leaked access key. This adds a second line of defense
that holds even when the first one fails, in one place, for every application that writes to a
bucket, with zero code changes.

## Problems It Can Remove
- Having to modify every individual tool (backup tooling, log shippers, CI/CD artifact uploaders)
  to add client-side encryption, some of which are closed-source or not under your control
- The gap between "we have server-side encryption" and "we have client-side encryption," which
  compliance questionnaires increasingly distinguish

## Practical Uses
- Encrypt CloudNativePG/Postgres WAL backups and dumps written to S3 so a bucket misconfiguration
  exposes only ciphertext
- Add a client-side crypto layer in front of Velero/Restic backups, Loki/Thanos logs, or Terraform
  state files (which often contain secrets) without touching those tools' own code
- A defense-in-depth layer for an MSSP/PTaaS client-data bucket, answering "is this encrypted
  client-side" with yes for every application at once rather than case by case
- Independent-of-provider crypto for multi-tenant shared S3 infrastructure or a customer-managed-
  key requirement

## Product Opportunities
A sellable "encrypt everything that touches S3" add-on for the MSSP admin-portal SaaS, aimed
directly at the compliance question a CISO or DPO is likely to ask.

## Agent / Automation Opportunities
None directly — this is infrastructure, not an agent-facing tool. It is, however, a natural
companion to any agent-driven backup/ops automation that writes to S3 and would otherwise need to
be trusted individually with encryption correctness.

## Integration
Point an application's S3 endpoint URL at the gateway instead of the real backend; deploy via
Docker or the published Helm chart (Artifact Hub). **Integration effort: Low** for anything using
a standard S3-compatible SDK, with the caveat that the S3 API compatibility matrix should be
checked against the specific operations the application actually uses.

## Architecture Notes
Random per-object data-encryption keys wrapped by a key-encryption key (envelope encryption)
avoid password derivation on the hot path; large files are encrypted in independently-seekable
chunks with per-chunk IVs so range requests still work against ciphertext. The project runs
conformance, mutation, and security-scanning CI on every change — unusually rigorous for its
star count.

## Maturity
Emerging. Created November 2025, actively developed, 39 stars/3 subscribers/10 forks. The
authors describe it as already running in their own production (encrypting CNPG Postgres
backups), but the latest tagged release (v0.12.3) is pre-1.0 and a v0.13.0-rc3 candidate is
explicitly flagged "for isolated non-production validation only."

## License
MIT. Unrestricted for commercial use, modification, and redistribution.

## Alternatives
Intrinsec/s3proxy (AGPL-3.0, older, less actively pushed), guided-traffic/s3-encryption-proxy (far
smaller, 5 stars), or modifying every application individually (impractical across languages,
SDKs, and closed-source tools).

## Risks / Limitations
- Still pre-1.0; the current release candidate is explicitly marked not for production
- A known upstream SeaweedFS versioned-listing bug can silently omit keys from listings — do not
  rely on affected listings for delete-enabled sync or completeness-critical migration
- Browser POST Object form uploads are not yet supported in the stable release (presigned PUT is)
- Small team so far (3 subscribers) — verify against the specific backend/SDK combination in use
  before any production deployment

## Recommendation
PROTOTYPE — worth trialing against a non-critical backup or log-archival bucket before trusting it
with anything production-critical, given both its own pre-1.0 status and the blast radius of
getting encryption wrong.

## Change History
### 2026-11-26
Discovered via GitHub search (slot 2: product infrastructure, APIs, backend components). First
catalog entry.
