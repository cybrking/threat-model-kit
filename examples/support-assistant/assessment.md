# Threat model assessment — support assistant idea

## Engineering handoff summary

Default to suggestion-only. CTRL-I-01 and CTRL-I-03 are architecture gates if any account-changing capability is contemplated. CTRL-I-02, CTRL-I-04, and CTRL-I-05 are required before release consideration. Copyable tasks and negative cases are in [security-controls.md](security-controls.md). Every control is proposed and every check is `not_run`.

## Inputs and inventory

- Assessment date: 2026-10-01. Bundle: `bundle-idea-106a9950-2026-10-01`.
- Sole source: `idea.md`, SHA-256 `106a99504cc68ca0b5a8c80e24114258cc4131a37febdc5d90713c7a64b9ce82`; maturity is an early fictional idea.
- Inspected: intended support-assistant purpose, untrusted uploads, sensitive information, summarization, recommended account changes, and undecided execution authority.
- Inventory: application, identity/tenant/case authorization, file parsing, model/provider, retrieval/memory/cache, tools/action API, workers, data stores, infrastructure, dependencies, CI/CD, deployment, logging, and external integrations are **unavailable/not yet designed**. Product, identity, AI platform, privacy, support, account-platform, and security owners must resolve them before implementation review.
- Excluded: implementation/deployment verification and active testing. No repository, AWS account, or external provider evidence was supplied.

## Deployment and authorization

Only idea-level intent exists. Source → artifact → deployed resource mapping is **unknown/not applicable yet**. No model, provider, parser, data store, credential, account API, cloud, environment, or runtime was observed; no AWS collection occurred.

Effective authority must be independent of uploaded content: current support principal + allowed action + tenant/account resource + support-case/policy context + enforcement point. Suggestion-only means the assistant possesses no execution credential or route. If execution is later approved, every action needs a typed allowlist, current server-side authorization, exact human approval, and independent parameter/policy enforcement.

## Validation and evidence review

- `document_valid`: **true** (validator exit 0).
- Validator: bundled `scripts/validate_model.py`, required workspace Python, expected snapshot `bundle-idea-106a9950-2026-10-01`, as-of `2026-10-01`.
- `coverage_gaps`: `COV-I-CO`, `COV-I-TO`, and `COV-I-AO`; operations is deferred at all boundaries because parsing, malware controls, quotas, identity, cache, logging, audit, rollback, monitoring, and incident response are undecided.
- Validator `review_gaps`: `check_unexecuted` for CHK-I-01 through CHK-I-05. Independent narrative gaps: named owners/risk policy are absent; execution mode, model/provider, data policy, and authorization design remain owner decisions.
- Evidence review: E-I-1 resolves to the source and supports only feature intent/unknowns. Logical storage/UI/action components are explicitly provisional assumptions, not observed technology. No code/runtime/test/external/cloud evidence is claimed.
- Independent review pass: challenged prompt/data separation, tenant context propagation, excessive agency, provider/data lifecycle, hallucination/overreliance, approval replay, and rollback. T-I-03 is conditional on execution; it remains a high-impact hypothesis rather than being dismissed. No threat is confirmed, mitigated, accepted, or dismissed.

## Threats and decisions

| ID | Provisional priority | Impact / feasibility / confidence | Response and next action | Residual risk / owner |
|---|---|---|---|---|
| T-I-01 | Critical | Critical / plausible / medium | Reduce with CTRL-I-01 and CTRL-I-03; design instruction isolation and independent authority | Probabilistic behavior; AI/security owner needed |
| T-I-02 | High | High / unknown / medium | Reduce with CTRL-I-02; define tenant/case binding everywhere | Shared/operational access; identity/data owner needed |
| T-I-03 | Critical | Critical / unknown / medium | Avoid by default via suggestion-only; if reopened, implement CTRL-I-03 and seek explicit approval | Authorized mistakes and irreversible actions; product/account/security owners needed |
| T-I-04 | High | High / unknown / high | Reduce with CTRL-I-04; approve provider and lifecycle | Approved-party exposure; privacy/AI owner needed |
| T-I-05 | High | High / plausible / medium | Reduce with CTRL-I-05 and CTRL-I-03; define evaluation and review | Incomplete but grounded outputs; support/product owner needed |

No risk has authorized acceptance. Priorities are provisional without organizational policy.

## Control checks and closure

CHK-I-01 through CHK-I-05 are active AI/security/implementation checks with outcome `not_run`. Actual target/environment: none. Authorization reference: none; execution was not authorized. Executor/time: not applicable. Expected outcomes are in `model.json`; observed outcome is none. Only passive source inspection occurred on 2026-10-01.

No threat is mitigated, so no closure claim exists. A future reviewer must verify exact attack-step interruption using current model/config/code/runtime evidence. Prompt-injection evaluation alone cannot establish safe authorization; logging alone cannot establish confidentiality or change prevention.

## Remaining work and owner decisions

1. **Product + security owners:** suggestion-only versus execution. Recommended: suggestion-only. If execution is chosen, approve exact action catalog, impact tiers, approval/separation, rollback, and acceptance authority.
2. **Identity/support owners:** principal/tenant/case/action model, enforcement points, revocation, escalated/admin access.
3. **AI platform owner:** model/provider, document parsers, retrieval/memory/cache, tool boundaries, languages, versioning, evaluations, monitoring.
4. **Privacy/legal/data owner:** allowed data/purpose, provider/subprocessors, training use, residency, retention/deletion/legal hold, logging, customer terms.
5. **Support/product owner:** citation and quality thresholds, abstention/escalation, human-review UX, accountability and audit needs.
6. **Risk/release owner:** policy, provisional-priority calibration, required check evidence, and any explicit residual-risk acceptance.

`security_effectiveness`: not established; no implementation, architecture, provider evidence, or executed check exists.  
`owner_decision_needed`: yes, for all six groups above. Document validity is not release approval.

## Validator result

Actually executed on 2026-10-01 using `<isolated-evaluation-python>` and the bundled validator version `0.2.1`, schema SHA-256 `c6f65c0cc1b805ce00d68e50252aea40b06cbb181e53d3754fbb7ce27ba31418`. Command parameters included expected snapshot `bundle-idea-106a9950-2026-10-01` and as-of `2026-10-01`. Result: exit 0, `document_valid: true`, no errors, coverage gaps `COV-I-CO`, `COV-I-TO`, `COV-I-AO`, review gaps for five unexecuted checks, and `security_effectiveness: not_assessed`.
