# Security controls for engineering

Input: `idea.md`, snapshot `bundle-idea-106a9950-2026-10-01`  
Maturity: idea / pre-architecture  
Priorities: provisional; no organizational risk policy supplied

## Work to prioritize

| Priority | Control / threats | What to build | Where enforced | Owner | Status |
|---|---|---|---|---|---|
| Critical | CTRL-I-01 / T-I-01 | Typed separation of untrusted document data from instructions/tools | Upload/parser, prompt assembly, orchestrator, output validator | AI platform/security owner needed | Proposed; CHK-I-01 not run |
| Critical | CTRL-I-03 / T-I-01,T-I-03,T-I-05 | Suggestion-only default; narrowly authorized, validated, approved actions only if execution is accepted | Orchestrator, action gateway, support UI | Product/account platform owner needed | Proposed; CHK-I-03 not run |
| High | CTRL-I-02 / T-I-02 | Server-side tenant/case binding at every data and action path | All storage, retrieval, context, cache, logs, output, actions | Identity/data platform owner needed | Proposed; CHK-I-02 not run |
| High | CTRL-I-04 / T-I-04 | Minimized, contractually approved sensitive-data lifecycle | Upload, storage, model/provider, telemetry, deletion | Privacy/AI platform owner needed | Proposed; CHK-I-04 not run |
| High | CTRL-I-05 / T-I-05 | Grounded, uncertainty-aware outputs and impact-based human review | Assistant output and support UI | Support/product owner needed | Proposed; CHK-I-05 not run |

## Copyable implementation tasks

### CTRL-I-01: Isolate untrusted documents from control instructions

- **Requirement:** Represent document content only as typed untrusted data. It cannot supply policy, authorization, tenant IDs, tool names/arguments, or hidden-context requests. Validate model output against a narrow schema and treat it as untrusted input to downstream code.
- **Why:** Reduce indirect prompt injection leading to disclosure, deception, or action (T-I-01).
- **Enforcement point:** Ingestion/parser, prompt/context assembly, model gateway/orchestrator, output validation, tool gateway if any.
- **Owner and timing:** AI platform/security owner needed; architecture gate and before release.
- **Acceptance criteria:** Multilingual, encoded, split, quoted, and visually hidden injection attempts cannot reveal system/other-tenant context, change policy, select tools, or trigger actions. Invalid output is rejected safely and logged without sensitive content.
- **Validation:** CHK-I-01; not run.
- **Dependencies / decisions:** Model, document formats/parsers, retrieval/memory, tools, context design, supported languages.
- **Residual risk:** No prompt-only defense guarantees model behavior; independent authorization in CTRL-I-03 remains mandatory.

### CTRL-I-02: Enforce tenant and case isolation end to end

- **Requirement:** Derive tenant/operator/case scope server-side and bind it to documents, chunks, embeddings, caches, conversations, prompts, outputs, logs, and account resources. Reauthorize each read and action using current scope.
- **Why:** Prevent cross-tenant disclosure or impact (T-I-02).
- **Enforcement point:** Every storage/retrieval/cache/model/action boundary; include operational/admin access.
- **Owner and timing:** Identity/data platform owner needed; before implementation and release.
- **Acceptance criteria:** Tenant A cannot enumerate, infer, retrieve, summarize, cache-hit, log-read, or act on tenant B data. Case closure and permission revocation deny subsequent access within the approved bound. Errors reveal no cross-tenant metadata.
- **Validation:** CHK-I-02; not run.
- **Dependencies / decisions:** Tenant and case identity sources, revocation timing, shared-index/cache design, support escalation policy.
- **Residual risk:** Approved support/admin access and shared provider layers require explicit governance and monitoring.

### CTRL-I-03: Keep changes suggestion-only unless bounded execution is approved

- **Requirement:** Ship without execution credentials by default. If owners approve execution, expose only typed allowlisted actions with least-privilege short-lived credentials, server-derived tenant, independent authorization/policy validation, explicit confirmation of the exact diff, idempotency, audit, rollback, and step-up or separation of duties for high-impact actions.
- **Why:** Break prompt-to-privileged-action paths and prevent overbroad or ambiguous approvals (T-I-01, T-I-03, T-I-05).
- **Enforcement point:** Orchestrator, action gateway/API, identity layer, support approval UI, audit/rollback system.
- **Owner and timing:** Product/account platform and security owners needed; execution is an owner decision before architecture approval.
- **Acceptance criteria:** Suggestion mode has no usable execution path or credential. In execution mode, altered/stale/replayed/wrong-tenant approvals and non-allowlisted parameters fail. Audit attributes proposer, approver, tenant, before/after state, and result; retries do not duplicate changes; approved rollback works.
- **Validation:** CHK-I-03; not run.
- **Dependencies / decisions:** Suggestion vs execution; action catalog and reversibility; approval roles; impact tiers; emergency process.
- **Residual risk:** Correctly authorized changes may still be wrong; owners must set action-specific tolerance and accountability.

### CTRL-I-04: Constrain sensitive-data use and lifecycle

- **Requirement:** Classify/minimize content, redact where feasible, approve provider/region/subprocessors and no-training terms, encrypt data, prohibit sensitive telemetry, and enforce tenant-specific retention/deletion/legal hold.
- **Why:** Prevent secondary use, over-retention, provider exposure, and privacy/contract violations (T-I-04).
- **Enforcement point:** Client/upload, parsing, storage, retrieval, prompts, provider API, logs/traces, backups, deletion workflow.
- **Owner and timing:** Privacy/AI platform owner needed; provider and policy approval before implementation.
- **Acceptance criteria:** A data-flow inventory shows every copy and subprocessor. Synthetic markers do not appear in prohibited logs/training paths. Access is least privilege. Expiry/deletion covers derived data and backups according to approved policy. Customer isolation/residency commitments are testable.
- **Validation:** CHK-I-04; not run.
- **Dependencies / decisions:** Data classes, purpose/consent, provider contract, residency, retention, legal hold, incident duties.
- **Residual risk:** Approved processing exposes data to designated parties; legal/contract acceptance remains human-owned.

### CTRL-I-05: Make outputs grounded and reviewable

- **Requirement:** Attach visible source locations to material claims, label inference and uncertainty, expose document coverage/limitations, prevent summaries from masquerading as account truth, and require human review proportionate to action impact.
- **Why:** Reduce harmful decisions from fabrication, omission, or overreliance (T-I-05).
- **Enforcement point:** Retrieval/prompt strategy, output schema, support UI, approval workflow, evaluation pipeline.
- **Owner and timing:** Support/product owner needed; thresholds and workflow before release.
- **Acceptance criteria:** Representative/adversarial evaluation meets owner-approved thresholds for citation support, material omissions, contradictions, and fabricated claims. Operators can inspect sources before acting. Low-confidence or insufficient-coverage cases abstain/escalate. UX testing measures automation bias.
- **Validation:** CHK-I-05; not run.
- **Dependencies / decisions:** Supported document types/languages, quality thresholds, action impact tiers, escalation route.
- **Residual risk:** Grounded outputs can remain incomplete; accountable human review is required for material decisions.

## Evidence required before release

After architecture selection, update the component/data/tool inventory and provide code/config/provider-contract evidence. Execute CHK-I-01 through CHK-I-05 in an explicitly authorized non-production environment with versioned datasets and model/config identifiers. Proposed controls and ticket-style acceptance criteria do not establish mitigation.
