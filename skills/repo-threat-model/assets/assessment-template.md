# Threat model assessment

## Inputs and inventory

Record purpose, assessment date/bundle, input maturity, supplied tickets/features/ideas/designs with locators and revisions/content digests, and any primary/supporting repositories, environment, exclusions, risk policy, and inspected/unavailable/deferred inventory categories. Assign next actions for gaps. Explain any zero boundary/flow inventory.

## Deployment and authorization

Record stated requirements, assumed design, repository intent when available, optional AWS identity/account/region/service scope, collection times, visibility gaps, source → artifact → resource mappings, drift, and effective authorization/tenant enforcement. Mark unknowns explicitly. Do not include credential or customer-data payloads.

## Validation and evidence review

Record `document_valid`, validator identity and output, `coverage_gaps`, `review_gaps`, and independent evidence gaps. State whether validation was executed or blocked. Verify evidence locators and exact claims; schema conformance alone is insufficient.

## Threats and decisions

For each prioritized scenario: ID, harm, impact, feasibility, confidence, evidence/assumptions, response, control, residual risk, owner, and next action. Record accepted-at, authority evidence, review/expiry, and overdue decisions for accepted risks. Keep priorities provisional when no organization policy was supplied.

## Control checks and closure

For each check: ID, scenario/control IDs, passive/active mode, target/environment, authorization reference, executor/time, expected/observed outcome, evidence, and blocked/not-run reasons. For each mitigated scenario record path-specific closure reasoning, reviewer, and unresolved assumptions.

## Engineering handoff

Link `security-controls.md`. Reconcile its IDs, priorities, owners, statuses, and acceptance criteria with the canonical model. Identify requirements before implementation and checks before release.

## Remaining work

List unresolved risks, missing evidence/access, owners or owner-needed markers, and next actions. State `security_effectiveness` and `owner_decision_needed` separately from document validity. Do not assert release readiness without the required policy and authorized decision.
