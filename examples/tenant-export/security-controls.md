# Security controls for engineering

Input: `ticket.md`, snapshot `bundle-ticket-0a94f286-2026-10-01`  
Maturity: ticket / pre-architecture  
Priorities: provisional; no organizational risk policy supplied

## Work to prioritize

| Priority | Control / threats | What to build | Where enforced | Owner | Status |
|---|---|---|---|---|---|
| High | CTRL-T-01 / T-T-01 | Phase-specific, server-side tenant and current-role authorization | Request, worker read, poll, download | Identity/export engineering owner needed | Proposed; CHK-T-01 not run |
| High | CTRL-T-02 / T-T-02 | Confidential, scoped, expiring, deletable export artifacts and safe audit trail | Artifact store and download service | Data/platform owner needed | Proposed; CHK-T-02 not run |
| High | CTRL-T-03 / T-T-03 | CSV formula neutralization and correct encoding | Export serializer | Export engineering owner needed | Proposed; CHK-T-03 not run |
| Medium | CTRL-T-04 / T-T-04 | Quotas, backpressure, idempotency, cancellation, and monitoring | Request, queue/worker, storage | Service owner needed | Proposed; CHK-T-04 not run |

## Copyable implementation tasks

### CTRL-T-01: Reauthorize every export phase

- **Requirement:** Derive tenant and role from trusted server-side identity. Authorize request, execution/data read, polling, and download separately. If privilege is revoked before a phase, deny that phase and cancel or quarantine pending output.
- **Why:** Prevent cross-tenant and post-revocation disclosure (T-T-01).
- **Enforcement point:** Each logical phase; if synchronous, request and response delivery still require the same tenant binding.
- **Owner and timing:** Identity/export engineering owner needed; design before implementation and verify before release.
- **Acceptance criteria:** Tenant A cannot request, execute, poll, or download tenant B's export. Revocation before execution or download denies access and exposes no records or link. Guessing or replaying job IDs fails. Denials are auditable without personal data.
- **Validation:** CHK-T-01; not run because no target exists.
- **Dependencies / decisions:** Identity source, authority tuple, revocation propagation bound, requester-versus-any-current-admin download policy, cancellation semantics.
- **Residual risk:** Races remain until the owner sets a staleness bound and tests every phase.

### CTRL-T-02: Protect the export artifact lifecycle

- **Requirement:** Bind each artifact to tenant/request/job; keep storage private; require current authorization; encrypt transport/storage; use short expiry and reliable deletion; never log customer data or reusable bearer URLs.
- **Why:** Limit durable disclosure and uncontrolled retention (T-T-02).
- **Enforcement point:** Artifact metadata, storage, download handler, logging, and deletion workflow.
- **Owner and timing:** Data/platform owner needed; retention/privacy choices before implementation, verification before release.
- **Acceptance criteria:** Unauthorized, expired, deleted, guessed, and replayed references return no artifact or usable location. Deletion meets the approved TTL. Logs identify events without containing records or reusable links.
- **Validation:** CHK-T-02; not run.
- **Dependencies / decisions:** Retention TTL, download audience, link design, encryption/key ownership, audit retention, backup deletion.
- **Residual risk:** Authorized recipients can copy files; the owner must approve this residual exposure and handling guidance.

### CTRL-T-03: Produce spreadsheet-safe CSV

- **Requirement:** Use a standards-aware CSV serializer and neutralize values beginning with formula-trigger characters according to a documented supported-client policy.
- **Why:** Prevent attacker-controlled customer fields from becoming formulas when opened (T-T-03).
- **Enforcement point:** Export serialization after data retrieval and before artifact creation.
- **Owner and timing:** Export engineering owner needed; before release.
- **Acceptance criteria:** A corpus using `=`, `+`, `-`, `@`, tabs, carriage returns, quoting, delimiters, and Unicode never evaluates as formulas in supported clients; data remains parseable and transformations are documented.
- **Validation:** CHK-T-03; not run.
- **Dependencies / decisions:** Supported spreadsheet/import clients and whether transformed values must round-trip exactly.
- **Residual risk:** Unsupported clients may interpret files differently.

### CTRL-T-04: Bound export amplification

- **Requirement:** Apply per-tenant request/concurrency/row/artifact limits, bounded retries, idempotency, cancellation, backpressure, cleanup, and abuse/capacity alerts.
- **Why:** Prevent resource exhaustion, excess cost, and unnecessarily broad extracts (T-T-04).
- **Enforcement point:** Request admission, query/data layer, worker/queue if used, and artifact storage.
- **Owner and timing:** Service owner needed; limits before release, tune after observed legitimate demand.
- **Acceptance criteria:** Duplicate/retried jobs do not duplicate work or artifacts; oversized and excessive requests fail safely; one tenant cannot starve others; cancellation stops future reads and cleans partial artifacts; alerts fire at approved thresholds.
- **Validation:** CHK-T-04; not run and future load execution requires explicit authorization.
- **Dependencies / decisions:** Scale targets, fairness policy, synchronous/asynchronous architecture, exception path.
- **Residual risk:** Legitimate large tenants can create spikes; capacity and exceptions require owner decisions.

## Evidence required before release

Provide implementation/configuration evidence for each enforcement point and execute CHK-T-01 through CHK-T-04 in an authorized non-production environment. Revisit flows and threats after architecture selection; written requirements alone do not mitigate any threat.
